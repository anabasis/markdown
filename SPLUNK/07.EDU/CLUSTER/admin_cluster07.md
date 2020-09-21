# Splunk Cluster Administration 7.0

## Module 7: SHC Management and Administration

### Module Objectives

- Deploy apps to a search head cluster
- Describe when and how to transfer captaincy
- Monitor SHC health with Monitoring Console
 
### Search-time Configuration Bundle Replication

- All members share the same configuration baseline
- Captain functions as a source control server
  - Members replicate changes from/to captain every 5 seconds (replication cycle)
  - Members generate a snapshot every minute and purge old sets every hour
    - var/run/splunk/snapshot/*.bundle
- All members SHOULD achieve eventual consistency at some point
  - In each replication cycle, members first pull (sync) outstanding changes from the captain, then push (submit) local changes to the captain
  - Members resolve conflicts on pull (sync)
  - All members keep a journal of changes in etc/system/replication/ops.json
    - Members check if the diverging point is at the end of captain’s log
    - Pull all changes after the diverging point and then insert the changes

### Splunk Deployer

- Use the deployer to distribute apps and non-replicable files into a SHC
  - Must run on a non-search head member   - Can be enabled on a dedicated deployment server or a master node
- Associate the deployer with a SHC by setting the pass4SymmKey in server.conf (no CLI support)
- Can share a deployer with multiple SHCs if:
  - The clusters have exactly the same apps
and configurations and use the same secret
Search Head Cluster 1

Search Head Cluster 2
Forwarders
Deployer server.conf [shclustering] pass4SymmKey = <secret>

Deployer
Master Node
Deployment Server
Indexer Cluster

### Splunk Deployer (cont.)

- Stage apps (configuration bundles) in the deployer's etc/shcluster folder
- Deploying bundles from deployer works in two ways:
  - Push bundles out from deployer to all search head members
ê Execute: splunk apply shcluster-bundle   - target <member:port> ê Can specify any member, but the deployer pushes the bundles to all
members
  - Configure search head members to fetch and sync after a restart ê Add the deployer address to the existing search head members: splunk edit shcluster-config -conf_deploy_fetch_url https://<deployer>:8089

### App and Configuration Distribution

- DO NOT add any Splunk default apps in the staging area (i.e. search, launcher, etc.)
- Deployer merges the files in each app's default and local directories into a bundle
- Merged (never local)
configurations are deployed to the corresponding app's default directory
- Each local user directory
bundle is first sent to the captain and the captain commits it to the user's
SPLUNK_HOME/etc/shcluster/
SPLUNK_HOME/etc/ apps/
apps/ <my-app>/
<my-app>/ default
default local
local users/ Deployer users/
SH Cluster members
SHC Captain
Note
Normal method for SHC configuration replication
The captain only adopts the new stanzas and ignores the existing stanzas.

### Managing SHC Bundle Deployment

- By default, apply shcluster-bundle automatically triggers a rolling-restart as necessary
  - 
To control the rolling-restart, you can apply in phases splunk apply shcluster-bundle -target <member:port> -action stage splunk apply shcluster-bundle -target <member:port> -action send splunk rolling-restart shcluster-members
- When you upgrade an app, by default the populated lookup tables get overwritten from the latest bundle update
  - 
To preserve the populated lookup tables, execute:
splunk apply shcluster-bundle -target <member:port> -preserve-lookups true

### Working with Deployment Server

- You CANNOT use deployment server to directly distribute apps to the peer nodes or SHC members
- You can use it to distribute apps to the master-apps and shcluster directories
[serverClass:idxc_x] stateOnClient = noop restartSplunkd = false
[serverClass:shc_y] stateOnClient = noop restartSplunkd = false
deploymentclient.conf on Master Node
[deployment-client]
serverclass.conf on Deployment Server
serverRepositoryLocationPolicy = rejectAlways repositoryLocation = SPLUNK_HOME/etc/master-apps
deploymentclient.conf on Deployer
[deployment-client] serverRepositoryLocationPolicy = rejectAlways repositoryLocation = SPLUNK_HOME/etc/shcluster/apps

 
### Bundle Deployment Summary

App bundle deployment
Deployer
splunk apply shcluster-bundle
- Sends bundles sequentially per app
- Sends to the captain first
2 1 3
Search bundle deployment
Bundle replication to search peers SHC Member 1 SHC Captain SHC Member 2
happens only from the captain
- Sends bundles in parallel per bundle 4
Peer 1 Peer 2 ... Peer n-1 Peer n

### SH Cluster Conf Replication Log Channels

- conf.log records recent configuration replication activity
index=_internal sourcetype=splunkd_conf | stats count by data.task   - 
SHC member-specific actions:
ê addCommit: changes made locally on this member ê pullFrom: changes pulled from the captain to this member ê acceptPush: changes pushed from a member to this instance ê computeCommon: initial negotiation with the captain ê purgeEligible: purge in-memory and on-disk data to reduce resource usage ê installSnapshot: captain
update to a snapshot of the latest configurations from the
ê downloadDeployableApps: install baseline apps from the deployer on startup   - 
Deployer-specific actions:
ê createDeployableApps: build bundles in var/run/splunk/deploy ê populateDeployableInfo: read bundles from var/run/splunk/deploy ê sendDeployableApps: push baseline apps to a search head cluster member

### Checking SHC Bundle Deployment Status

- Search from a search head member:
index=_internal component=ConfDeployment data.task=*Apps | table host data.source data.target_label data.task data.status
- Check the Search Head Clustering: App Deployment dashboard
http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/HowconfrepoworksinSHC

### Some Useful Debugging Searches

- Find missing baseline:
index=_internal sourcetype=splunkd_conf STOP_ON_MISSING_LOCAL_BASELINE | timechart count by host
- Overall configuration replication behavior
index=_internal sourcetype=splunkd_conf pullFrom data.to_repo!=*skipping* | timechart count by data.to_repo
- Evidence of captain switching
index=_internal sourcetype=splunkd_conf pullFrom data.from_repo!=*skipping* | timechart count by data.from_repo
- Find the destructive resync events:
index=_internal sourcetype=splunkd_conf installSnapshot | timechart count by host

### More Useful Conf Replication Log Channels

- splunkd.log components
  - 
ConfReplication   - 
ConfReplicationThread   - 
ConfReplicationHandler   - 
loader   - check events during startup
- Replication performance: metrics.log group=conf
  - 
wallclock_ms_total invocation
is for an action and wallclock_ms_max is for a single
- Network activity between the members:
  - 
sourcetype=splunkd_access uri_path="/services/replication/configuration*"
- Network activity between the members and the deployer:
  - 
sourcetype=splunkd_access uri_path="/services/apps/deploy*"
uri_path="/services/apps/local*" OR

### Types of SHC Captaincy

- SHC has two types of captaincy designed to handle specific cases
  - 
Dynamic captain (default)
ê If you want to reassign captaincy, you can manually transfer captaincy
to a preferred member
  - 
Static captain
ê When members are unable to elect a captain

### Controlling Captaincy

- Why?
  - SHC Captain consumes more CPU and memory resources   - Assign captaincy to members based upon geographic preference   - SHC search capacity = captains_scheduler_count x #_of_members
with scheduled search enabled (adhoc_searchhead = false)
ê If a system with less number of cores becomes the captain, the overall
scheduling capacity gets lowered
- How?
  - Set preferred_captain=false in server.conf of the excluded members   - SHC tries to elect a member with preferred_captain=true (default), but
not always possible
ê For example, if no preferred members are reachable by the majority   - When a non-preferred captain is chosen, the first available preferred
captain member will request a captaincy transfer

### Transferring Captaincy

- In dynamic election mode, run this command from the current captain to transfer captaincy to the target member:
splunk transfer shcluster-captain -mgmt_uri https://<target_member>:8089
  - 
splunk rolling-restart shcluster-members automatically invokes the captaincy transfer
- To check the transfer status, run this from any member:
splunk show shcluster-status

### Designating a Static Captain

Current Captain
Search Head Cluster SH3 10.1.1.x (Majority) 20.2.2.x (Minority)
- During a network partition failure, minority group members are unable to elect a captain
- Splunk admins can designate a captain for the minority group as a temporary workaround
  - On the new captain node (SH3):
splunk edit shcluster-config -election false -mode captain -captain_uri https://SH3:8089   - On the rest of the minority group members:
splunk edit shcluster-config -election false -mode member -captain_uri https://SH3:8089

### Restoring the Cluster to Dynamic Captaincy

Search Head Cluster
SH3 (Static Captain)
✔
10.1.1.x (Majority) 20.2.2.x (Minority)
Upon resolution of the issue, revert to the dynamic captain mode 1. On all majority members: Disable election and report to the static captain
splunk edit shcluster-config -election false -mode member -captain_uri https://SH3:8089 2. On all members: Re-enable election; change the static captain last
splunk edit shcluster-config -election true -mgmt_uri <THIS_MEMBER>:8089
3. On SH3: Bootstrap as the new dynamic captain for the entire SHC
splunk bootstrap shcluster-captain -servers_list <SEARCH_HEAD_MEMBER_LIST>

### Adding Search Head Members

- Why always use new Splunk instances?
  - SH cluster configuration consistency is derived from the ops.json journals
  - The ops.json file is fixed in size (configurable) and gets rolled
  - Non-members do not have this file
  - Members downed for a long time may not be able to find the diverging point
- Add a new member or a previously decommissioned member:
  - Details on the next slide
- Add an offline member to the search head cluster:

1.Restart the offline member (the cluster should recover automatically)
2.If the member is unable to fully join the cluster, run this command on the re- joining member to force the resynchronization: splunk resync shcluster-replicated-config http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/Handlememberfailure

### Adding a New or Decommissioned Member

1. Delete Splunk Enterprise, if exists 2. Install and initialize the instance 3. Join the search head cluster
  - To announce itself to an existing SH cluster, run on the new instance:
splunk add shcluster-member -current_member_uri https://<any_existing_member>:8089   - To introduce a new member to the cluster, run from any existing member:
splunk add shcluster-member -new_member_uri https://<new_member>:8089
- When a new member joins the cluster, it gets:
  - The deployed bundles from the deployer   - The replicated configurations and artifacts from the captain

### Search Peer Quarantine

- To troubleshoot without stopping a peer node, quarantine the peer node temporarily
  - Prevents the search head from dispatching
searches to the peer node   - The peer continues to index and replicate
- To quarantine a search peer:
  - Run this command on all SHC members: splunk edit search-server <peer_uri> -action [quarantine|unquarantine]   - Use the UI: Settings > Distributed search > Search peers   - Or, distribute the resulting distsearch.conf file with deployer
- You can override the quarantine on a search-by-search basis by specifying the peer with splunk_server=<quarantined_peer> in a search

### Decommissioning a SH Cluster Member

Is the member downed unannounced?
Temporarily decommissioning?
Yes
Run from any SHC member:
splunk remove shcluster-member -mgmt_uri <downedSH:m_port>
No
Yes
Run on the decommissioning member:
Important splunk remove shcluster-member splunk stop
DO NOT just run: splunk stop.
No
Run on the decommissioning member:
splunk remove shcluster-member splunk disable shcluster-config

### Upgrading a Search Head Cluster

- The process is the same for maintenance and major release upgrades
http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/UpgradeaSHC
- Upgrading from 6.4 or later
1. Upgrade one member and transfer the captaincy
Note a. Stop > Upgrade > Start the member
All cluster members must be on b. Wait while it again joins the search head cluster c. Transfer captaincy to the upgraded member
the same version down to the maintenance level.
2. Upgrade the rest of the members
It is not necessary to upgrade the indexers at the same time. 3. Upgrade the deployer
- Upgrading from 6.3 or earlier
  - You must stop all members and the deployer before you can upgrade

### Key SH Cluster Maintenance Commands

> splunk help shclustering
- Helpful CLI commands to run on the SHC captain
splunk rolling-restart shcluster-members splunk show shcluster-status
- Helpful CLI commands to run on the SHC members
splunk [edit|list] shcluster-config splunk add shcluster-member splunk resync shcluster-replicated-config splunk clean raft
- Helpful CLI commands to run on the SHC deployer
splunk apply shcluster-bundle

### Search Head Cluster Dashboards in MC

- SHC Status and Configuration provides high-level SHC health
  - The last heartbeats should be nearly the same
  - A member heartbeat drift and frequent captain election indicate possible problems
- members Configuration Replication shows how user changes propagate among its
  - Drill down to see members' common baseline and detect unpublished changes
- Growing backlogs in Artifact Replication Job Activity should be investigated
- In Scheduler Delegation, monitor Skip Ratio and Execution Latency

### Troubleshooting the SHC with Monitoring Console

By default, members report a heartbeat every five seconds and the captain marks a member as Down after missing for 60 seconds
If a member was down for a long period of time, it may not be able to find a baseline and a manual resync on the member is required
To restore consistency: splunk resync shcluster- replicated-config

### Further Reading:

- Search head forwarding http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Forwardmasterdata
- Distribute apps and configuration updates with deployer http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/PropagateSHCconfigurationchanges
- Search head cluster status and troubleshoot issues http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/ViewSHCstatusinDMC
- Use static captain to recover from loss of majority http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/Staticcaptain

### Lab Exercise 7   - Deploy a SHC App

- Time: 40 - 45 minutes
- Tasks:
  - Configure the SHC members and the deployer
  - Stage and distribute apps to search head cluster members
  - Verify the app deployment
  - Complete the Monitoring Console setup on dserver
  - Review the search head cluster dashboards

### Lab Exercise 7   - Deploy a SHC App (cont.)

x = Your student ID 8?89 = splunkd-port
Indexer Cluster (10.0.x.1)
cmaster 8089
SSH you@10.0.x.1
2
SSH you@Public_DNS ➜ http://{Public_DNS}/dserver ➜
dserver 3
(Deployer) 8189 4
8189 8289 8389 8489
Search Head Cluster (10.0.x.2)
fwdr 8289
SSH you@10.0.x.2
1
1 1 8189 8289 8389 8489
Jump Server (10.0.x.3)

### Lab Exercise 7   - Deploy a SHC App (cont.)

Your Browser
Indexer Cluster
http://{Public_DNS}/{splunk_server} For example:
idx1 idx2 idx3 idx4 http://{Public_DNS}/sh2 http://{Public_DNS}/sh3
Search Head Cluster http://{Public_DNS}/sh4 http://{Public_DNS}/sh5 (Optional)
sh1 sh2 sh3 sh4
Jump Server
Public_IP = Same as your jump server splunk_server = Splunk server name
cmaster dserver fwdr
5