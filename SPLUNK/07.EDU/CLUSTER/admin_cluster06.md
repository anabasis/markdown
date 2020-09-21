# Splunk Cluster Administration 7.0

## Module 6: Search Head Cluster

## Module Objectives

- Describe search head cluster architecture
- Configure a search head cluster
- Identify the captain and monitor cluster status with CLI
- Describe optional configuration settings

### Search Head Challenges

- Individual search heads are single points of failure
- Maintaining consistent configurations across multiple search heads
- Increasing concurrent users and searches
- Managing large numbers of scheduled searches and alerts

### Addressing Search Scaling

- As the number of simultaneous searches increases, you have several actions to mitigate performance issues
- Best practices:
  - Increase the number of search peers (indexers)
  - Optimize scheduled searches to run on non-overlapping time slots
  - Isolate scheduled searches, real-time searches, and ad-hoc searches
  - Limit the time range of end-user searches
  - Add more search heads
  - Configure user roles to limit the number of concurrent real-time searches
  - Implement search head clustering

### Benefits of Search Head Clusters

- Highly available and scalable search service by grouping search heads into a cluster
  - Always-on search services
  - Simple horizontal scaling
    - Add more members any time ê Commodity hardware
    - No need for NFS
- Seamless user experience
  - Easy to on-board users and apps
- Reliable alerts
  - Search job failure aware and reschedule
- Dedicated configuration bundle management

Load Balancer
Search Head Cluster
Deployer

### Search Head Cluster Terminology

- Raft distributed consensus
  - Captain/Member (instead of master/peers)
- Replication factor
  - Applies only for search artifacts
- Deployer
- How does it work?
  - Members of search head cluster elect

the captain dynamically   - Replicates configuration changes to
all cluster members   - Captain schedules and manages searches
Deployer

### Load Balancer

Captain
Member Member
Search Head Cluster
Raft Distributed Consensus

### How Does SH Cluster Scale Search Capacity?

- Load-based scheduling heuristic
  - Captain is the only job scheduler
    - Captain establishes authority and is also a member ê The normal scheduler on all members is suppressed ê Captain schedules and delegates jobs to its members
  - Captain maintains global knowledge of all search jobs ê Members regularly report their job loads to the captain ê Ad-hoc and real-time search results (artifacts) are not replicated ê Saved and scheduled search artifacts are replicated per search head cluster replication factor
  - Captain directs which member to contact to access search results

### Report Scheduler

- Without SHC, a search head can defer and skip search jobs if:
  - Search head restarts (could cause a gap in summary index)
  - 
Search head encounters resource constraints (max. concurrent search limit) ê A user performing prolific ad-hoc searches can overwhelm other searches ê Infrequent long-running searches can starve out frequent short-running
searches ê A deferred job is implicitly retried (repeated for the duration of its window)
- With SHC, the captain mitigates starvation and recovers missed jobs
  - Implements a heuristic approach based on job load, priority scoring, search
history introspection, and schedule window
http://docs.splunk.com/Documentation/Splunk/latest/Report/Configurethepriorityofscheduledreports

### How Does Cluster Provide Always-On Services?

- Auto SH captain failover
  - Elect new captain via Raft
  - Artifacts Running jobs Persists its records in
Alerts Load stats var/run/splunk/_raft/<server>/log
  - Members register their list of artifacts,
New Captain running jobs, alerts, and search load statistics to a new captain
  - New captain enables its scheduler
  - New captain executes fix-ups if needed
- Uses DNS names when initializing
Old Captain
members

Fix-up
Scheduler
Members
 
### Search Head Cluster Key Considerations

- Always use new Splunk instances
  - Must have at least three members
  - You cannot upgrade from an existing SH or a member of SH Pool
    - Migrate the configurations after the SH Cluster is up
- Same hardware requirements as the dedicated search head
  - Use identical specifications for all members (bare metal or VM) ê Works on all operating systems supported for Splunk Enterprise ê Same version of Splunk Enterprise
- Synchronize the system clock on all members including the indexing layer
- Search head cluster can search 6.x or 7.0 search peers

### Sharing Splunk Server Roles

BEST PRACTICE:
- Disable local indexing and forward everything, including all internal indexes, to the peer nodes (discussed later)
- A search head cluster member should not have any other server roles WARNING:
- A member cannot be a search peer to another search head
  - 
Exception: when it is configured to be a monitored instance of Monitoring Console running on other instance

### Search Head Cluster Ports

Site1 Site2
Cluster Master pass4SymmKey
for Search Head Cluster
Peer1 Peer2 Peer3 Peer4
9100
9100
9200 9200
SH1
SH2 SH3 SH4 8089 8089
8089
8089 8089
8089
8089
8089
9100
pass4SymmKey
9100
for Indexer Cluster
Management (splunkd port) Replication (index replication port) Replication (search artifact replication port) Search request/results (distributed search)

8089
 
### Deploy a Search Head Cluster

1. Install Splunk Enterprise and set admin password
  - Recommend LDAP/SAML 2. Bring up and initialize all SH cluster members:
splunk init shcluster-config -mgmt_uri https://SH2:8089 -replication_port 9200   - secret shcluster 3. Assign one of the members as the captain and set a member list:
splunk bootstrap shcluster-captain   - servers_list https://SH2:8089,https://SH3:8089,https://SH4:8089 4. Check search head cluster status: splunk show shcluster-status splunk list shcluster-members
Note
Search head cluster configuration is in SPLUNK_HOME/etc/system/local/server.conf.

### Connecting a SHC to Non-Clustered Indexers

- You have two ways to search non-clustered indexers
  - Individually add the search peers from each member, or
  - Enable search peer replication
    - Add the search peers to one SHC member and let the SHC replicate the peer configurations to all cluster members ê All members gain access to the same set of search peers
  - 
[raft_statemachine] disabled = false replicate_search_peers = true
server.conf of each SHC member
Once enabled, you can add new peers with CLI, Web, or REST API
splunk add search-server https://<peer>:8089 -remoteUsername <user> -remotePassword <pw>

### Connecting a SHC to Indexer Clusters

- SHC members do not have site awareness
  - No site-by-site artifact replications   - Using site0 can provide a seamless search experience
- Configure each SHC member as a search head on an indexer cluster   - The search head members get their list of search peers from the master
node of the indexer cluster   - Connecting to a single-site indexer cluster:
splunk edit cluster-config -mode searchhead -master_uri https://10.0.1.3:8089 -secret idxcluster
  - Connecting to a multisite indexer cluster:
splunk edit cluster-config -mode searchhead -master_uri https://10.0.1.3:8089 -site site2 -secret idxcluster

### Search Head Cluster Member server.conf

[general] License master password
pass4SymmKey = $1$ttbJh5nUk5AM serverName = sh2 Indexer cluster site association
site = site2
If connected to an indexer cluster
[clustering] master_uri = https://10.0.1.3:8089 mode = searchhead multisite = true Indexer cluster password
pass4SymmKey = $1$uMfLhYvCipA/Eg==
Search head cluster artifact replication port
[replication_port://9200]
[shclustering] disabled = 0 Member's self identifying address to the SHCluster
mgmt_uri = https://sh2:8089 Search head cluster password
pass4SymmKey = $2$ptbLhYvCipA/Eg== Generated search head cluster ID
id = 571B9C60-66EA-4B9F-8562-27B62E93E31F

 
### Checking SH Cluster Status

> splunk show shcluster-status
Captain:
Members: sh4 label : sh4 last_conf_replication : Fri Aug 26 20:29:30 2016 dynamic_captain : 1
mgmt_uri : https://10.0.1.2:8489 elected_captain : Fri Aug 26 20:22:04 2016
mgmt_uri_alias : https://10.0.1.2:8489 id : 75DBEA2A-0204-4C15-983C-116F1
status : Up initialized_flag : 1
label : sh2
sh2 label : sh2 mgmt_uri : https://10.0.1.2:8289
mgmt_uri : https://10.0.1.2:8289 mgmt_uri_alias : https://10.0.1.2:8289 min_peers_joined_flag : 1
status : Up rolling_restart_flag : 0 service_ready_flag : 1
sh3 label : sh3 last_conf_replication : Pending
mgmt_uri : https://10.0.1.2:8389 mgmt_uri_alias : https://10.0.1.2:8389
status : Up > splunk clean raft
  - When SHC can't elect a captain, run on all members before bootstrap   - When SHC has an active captain but a member can't join, run on the failing members

### Splunk Web Settings Menu Changes

- When search heads become members of a search head cluster, the Settings menu in Splunk Web changes
  - Hides all non-replicable options
ê You can unhide them, if necessary ê If you make changes to the settings that are hidden, you must use the deployer to push the underlying changes   - Enables the search head clustering
UI on all SHC members
ê Able to perform rolling restart and
captaincy transfer

### Configuration and Artifact Replication

- Captain orchestrates both configuration and artifact replication
- Knowledge object configurations (replicated to all members):
  - Changes made via Splunk Web, CLI, or API are replicated   - Direct .conf file edits must be implemented using the deployer   - More details are discussed in the next module
- Artifacts (based on the search head cluster replication factor):
  - Only the artifacts resulting from scheduled reports are replicated   - Real-time and ad-hoc search artifacts are not replicated, instead they are
proxied (discussed later)   - Captain enforces artifact fix-ups according to its replication policy
http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/HowconfrepoworksinSHC

### Roles and User Account Replication

- Use any of the available authentication methods
  - Splunk native authentication
    - Automatically replicates the underlying .conf files and
SPLUNK_HOME/etc/passwd
  - SAML authentication
    - Only replicates authentication.conf ê Must use the deployer to push the certificates
  - Scripted authentication
    - Must use the deployer to push both the script and authentication.conf
- User configurations are automatically synchronized across all members

### Ad-hoc Search Management

- Ad-hoc search results are not replicated
  - Artifact proxying is used to access the ad-hoc search results from any member
  - If the search is accessed from a different member, artifact proxying calls the owner member to get the results
- To reduce captain's work load, disable running scheduled searches on the captain with captain_is_adhoc_searchhead = true (on all members)
- To configure a member to run only ad-hoc searches, set adhoc_searchhead = true

### Alerts

- When results of search meet alerting criteria:
  - The alerts are checked and fired locally on the member that ran the search job
  - The local alert information is reported to the captain
- Captain merges and maintains global view of alerts
  - Centralizes suppression information
    - Remember, captain is the only scheduler and it delegates jobs
  - Merged alerts and suppression information are sent to all members

### Handling Summarization

- The captain coordinates the report and data model acceleration searches
  - The resulting summaries are stored on indexers
- The summary indexes are stored only on the search head that generates them
  - If you want to share them with other members, forward your summary indexes to the indexing layer BEST PRACTICE:
- To consolidate index data, forward all indexes (including summary and internal indexes) to the indexing layer

[indexAndForward] index = false
[tcpout] defaultGroup = default-autolb-group forwardedindex.filter.disable = true indexAndForward = false
[tcpout:default-autolb-group] server=idx1:9997,idx2:9997,idx3:9997, idx4:9997 autoLB = true useACK = true
outputs.conf

### Artifact Reaping

- Reaping (deletion of search results) happens when artifact TTL expires
- Original member reaps its search artifacts and notifies captain
- Captain orchestrates reaping of the replicas
- If the original member is out of commission, captain waits beyond TTL and replicas are reaped thereafter

### Restarting a Search Head Cluster

- To restart SH cluster, splunk rolling-restart shcluster-members
  - Members restart in phases so the cluster can continue to operate   - The transfer, captain thus is preventing the final member captaincy to restart from changing and automatically during the invokes restart captaincy
process   - 
Deployer automatically initiates a rolling restart, when necessary
Rolling restart Success : 1
Message : Rolling Restart of all the search head cluster members has been kicked off. It might take some time for completion. After restart the information will be logged at audit log, Meanwhile you can check the progress of this transaction...
- To splunk check rolling-restart the progress,
shcluster-members -status 1
Message : Rolling Restart Peer Status :
Peer | Status | Start Time | End Time | GUID 1. sh3 | RESTARTING | Wed Jul 15 00:40:47 2015 | N/A | 6BA626AA-CDFD-4599-9BC5-6C4FB87DF70C 2. sh4 | NOT STARTED | N/A | N/A | FEC2B0AD-0244-4A36-8CBD-2D2A7A3D325D

### Search Head Cluster Log Channels

- Heartbeat and election
  - splunkd_access.log: uri_path="/services/shcluster/member/consensus*"   - Corresponding events in sourcetype=splunkd
ê component=SHCRaftConsensus OR component=SHClusterMgr ê component=Metrics group=captainstability upgrades_to_captain=1
- Job scheduling
  - scheduler.log: sourcetype=scheduler component=SavedSplunker status=*   - Corresponding events in sourcetype=splunkd
ê component=SHCMaster delegate ê component=Metrics group=search_concurrency
- Artifact proxy and reaping
  - sourcetype=splunkd_access uri_path="/services/search/jobs*"
ê status!=20* indicates an issue ê method=GET isProxyRequest=true indicates a proxied request   - sourcetype=splunkd_access uri_path="/services/shcluster/captain/artifacts*"

### Useful SHC Debugging Searches

- Election history:
  - 
index=_internal component=SHCRaftConsensus by _time host
host IN (sh2, "All sh3, hail sh4) leader" sourcetype=splunkd
| stats values(message)
- Job scheduling status:
  - 
index=_internal status=* count by | status_host eval host status_host=status."-".host IN limit=0 (sh2, usenull=f
sh3, sh4) sourcetype=scheduler
| timechart span=5m
- Skipping jobs:
  - 
index=_internal status=continued ".host | timechart host OR status=skipped span=5m IN (sh2, count sh3, by sh4) | eval status_host sourcetype=scheduler
status_host=status."-
limit=0 usenull=f
- Artifact proxy:
  - 
index=_internal uri_path="/services/search/jobs*" by method host file
host IN (sh2, sh3, isProxyRequest=true sh4) sourcetype=splunkd_access
| stats count

### Further Reading: Search Head Cluster

- Basic search head clustering concepts
http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/SHCarchitecture
- Migrating a search head pooling environment
http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/Migratefromsearchheadpooling
- Splunk Answers on search head clustering
http://answers.splunk.com/topics/shc.html

### Lab Exercise 6   - Deploy a Search Head Cluster

- Time: 35 - 40 minutes
- Tasks:
  - Add two more search heads to site2
  - Enable a search head cluster with site2 search heads
  - Verify that your search head cluster is functioning

### Lab Exercise 6   - Deploy a Search Head Cluster (cont.)

Your Computer
x = Your student ID 8?89 = splunkd-port
Indexer Cluster (10.0.x.1)
cmaster 8089
dserver 8189
SSH you@10.0.x.1
8189 8289 8389 8489
SSH you@Public_DNS ➜
Search Head Cluster (10.0.x.2)
fwdr 8289
SSH you@10.0.x.2
1
1 1 8189 8289 8389 8489
Jump Server (10.0.x.3)

### Lab Exercise 6   - Deploy a Search Head Cluster (cont.2)

Your Browser
Indexer Cluster
idx1 idx2 idx3 idx4 http://{Public_DNS}/{splunk_server} For example:
Search Head Cluster
http://{Public_DNS}/sh3 http://{Public_DNS}/sh4
2 3 sh1 sh2 sh3 sh4
Jump Server
Public_IP = Same as your jump server splunk_server = Splunk server name
cmaster dserver fwdr
