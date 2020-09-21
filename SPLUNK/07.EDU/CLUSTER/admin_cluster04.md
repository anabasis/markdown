# Splunk Cluster Administration 7.0

## Module 4: Indexer Cluster Management and Administration

### Module Objectives

- Enable replication for custom indexes
- Deploy common apps and configurations peer nodes
- Take a peer offline temporarily
- Decommission a peer permanently
- Clean up excess cluster buckets
- Optimize peer node storage utilization
- Configure Monitoring Console for indexer cluster environment

### Managing Indexes in an Indexer Cluster

- All of indexes.conf peer nodes must files use the same set
  - Do index not settings use the in Splunk a cluster UI to configure
  - Deploy with master-apps
- To edit enable indexes.conf replication on for master an index,
node   - 

```properties
[main]
repFactor = auto

[history]
repFactor = auto

[summary]
repFactor = auto

[_internal]
repFactor = auto

Explicitly in indexes.conf
set repFactor=auto per index
[_audit] repFactor = auto
- To set disable repFactor replication = 0
for an index,
[_thefishbucket] repFactor = auto
  - 
To possibility remove free up the of disk excess duplicate space copies
and events, eliminate manually the
[_telemetry] homePath = $SPLUNK_DB/_telemetry/db coldPath = $SPLUNK_DB/_telemetry/colddb thawedPath = $SPLUNK_DB/_telemetry/thaweddb repFactor = auto ... Generated for Cho Jun Seung (jscho@time-gate.com) (C) Splunk Inc, not for master-apps/_cluster/default/indexes.conf
distribution
```

### Splunk Deployment Tools

- The master node distributes configuration bundles (apps) to the peer nodes
  - Supports common configurations for inputs, parsing, and indexing
- Stage deployment bundles in the master's SPLUNK_HOME/etc/master-apps directory
  - For common apps, copy them to the <app-name> subdirectory   - For standalone files, copy them to the _cluster/local subdirectory

Master Node
Search Head Cluster

### Master-apps Configuration Bundles

- Validate the bundle (optional):
  - splunk validate cluster-bundle [--check-restart]
- Commit the bundle to the peers:
  - splunk apply cluster-bundle [--skip-validation]   - The master-apps folder is replicated to the peers' slave-apps folder   - Do not edit the slave-apps content directly   - If necessary, master node initiates an automatic rolling restart of all peers
SPLUNK_HOME/etc/master-apps
SPLUNK_HOME/etc/slave-apps /_cluster
/_cluster /default
/default /local
/local /<app_name>
/<app_name> /default
/default /local Master Node /local
Peer Nodes

### Index-time Configuration Precedence Revisited

Precedence order for indexer cluster peers:
1. Slave-app local directories (cluster peers only)
2. Highest priority System local directory
3. App local directories
4. Slave-app default directories (cluster peers only)
5. App default directories
6. System default directory

Lowest priority
Note
Within the slave-apps directories, the precedence is based on the ASCII sort order.

### Rollback the Configuration Bundle

- If the apply cluster-bundle action fails, you can restore to the previously running state
  - Rollback command restores the peer nodes to their previous state
  - After the rollback, fix the problem in the master-apps directory and re-apply
- To rollback, run from the master node:
splunk rollback cluster-bundle
  - Peers can only join the cluster if bundle
Latest validation succeeds during restart
Active Bundle
Bundle
  - 
rollback
Rollback is different than undo
ê Toggles only between the most recent configuration bundle and the
previous bundle

### Checking Deployment Status

- You can configure and check bundle actions using Splunk Web or the CLI
  - 
Settings > Indexer Clustering > Edit > Configuration Bundle Actions   - splunk show cluster-bundle-status
master
cluster_status=Rolling restart of the peers is in progress. active_bundle
checksum=2876FDCB7CEA1ED558268715613AD08F timestamp=1507674623 (in localtime=Tue Oct 10 22:30:23 2017) latest_bundle
checksum=2876FDCB7CEA1ED558268715613AD08F timestamp=1507674623 (in localtime=Tue Oct 10 22:30:23 2017) last_validated_bundle
checksum=2876FDCB7CEA1ED558268715613AD08F last_validation_succeeded=1 timestamp=1507674623 (in localtime=Tue Oct 10 22:30:23 2017) last_check_restart_bundle
last_check_restart_result=restart not required
idx4 BEED9FE9-8FCB-4152-85D6-DE6F668A39A1 site2
active_bundle=2876FDCB7CEA1ED558268715613AD08F latest_bundle=2876FDCB7CEA1ED558268715613AD08F last_validated_bundle=2876FDCB7CEA1ED558268715613AD08F last_bundle_validation_status=success restart_required_apply_bundle=0...

### Log Channels for master-apps Activities

Master Node Peer Nodes component=CMBundleMgr
validate cluster-bundle or
Make a bundle apply cluster-bundle
Bundle Validate
Download the bundle
Validate the bundle
component=ClusterBundleValidator
Compute restart requirement
show cluster-bundle-status
Load the new conf
Report Rolling-restart
component=BundleJob
index=_internal sourcetype=splunkd component IN (CMBundleMgr, BundleJob, ClusterBundleValidator) | stats count by _time, host message

### Indexer Cluster Maintenance Mode

- Invoke maintenance-mode when performing any work on a peer node that may cause excessive bucket status changes
  - Examples: network reconfiguration or peer node upgrades > splunk [enable|disable|show] maintenance-mode
  - No notion of sites; works for both indexer cluster configurations   - Does not enforce replication factor or search factor policies while it is
enabled   - Indexing continues but forestalls bucket rolling during this period   - Still reassigns primaries to operate under a valid but incomplete state   - After disabling maintenance mode, master node catches up on replication
policies
- splunk apply cluster-bundle and splunk rolling-restart automatically invoke maintenance mode

### Temporarily Taking a Peer Offline

- Before taking a peer node offline, make sure the cluster has enough peer nodes to meet the replication policy
  - To minimize bucket fixup activities, take down only one node at a time
- To bring down a peer temporarily, run: splunk offline
  - The master node delays bucket-fixing and just maintains a valid state
  - The peer node must be back online within 60 seconds (by default)
ê If the peer node does not return, the master node initiates bucket fixup
activities
  - 
If you need more time, extend the wait time on the master node:
splunk edit cluster-config -restart_timeout <wait_seconds>

### Adding or Moving Peer Nodes

- To add a peer node to an existing cluster, use the standard peer node configuration procedures
- To move a peer node to a different site:
1. Take the node offline with splunk offline --enforce-counts
2. Move the server and have it join the new site's network
3. Delete or uninstall Splunk Enterprise from the peer ê Remove the index directories, if necessary
4. Reinstall Splunk Enterprise
5. Enable it as the peer node to the new site

### Permanently Decommissioning a Peer Node

- To decommission a peer node permanently, run:
splunk offline --enforce-counts   - 
Does not shut down until all search and remedial activities have completed   - 
Can take quite a while because the cluster must come to a complete state http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Takeapeeroffline
- The master keeps the peer node information even when it is decommissioned
  - If you want to remove it from the master node permanently, run:
splunk remove cluster-peers -peers <guid>,<guid>,...

### Indexer Cluster Peer Status

The master node dashboard reports several possible status conditions for peer nodes
http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Howtomonitoracluster
Not Recommended Temporary Permanent splunk stop
splunk offline splunk offline --enforce-counts
ShuttingDown
ReassigningPrimaries
Decommissioning
Stopped
ShuttingDown
GracefulShutdown
Down

### Decommissioning a Site

- After a site upgrade/migration, you can decommission a site that is no longer in use
- Prerequisites
  - The cluster must be in a complete state
  - The master node must not be a part of the decommissioning site
  - There must be at least one searchable copy of each bucket on other remaining sites
- NOTE:
  - Any prior standalone and single-site buckets will be lost
  - Decommissioning starts bucket fix-up activities and can take a considerable amount of time

### Steps for Decommissioning a Site

1. Reassign site search head(s) to a remaining site 2. If discovery, forwarders re-assign are associated them to with a remaining the decommissioned site
site using indexer
3. Run splunk enable maintenance-mode on the master node 4. Update the following attributes in the master's server.conf:
  - 
available_sites   - 
site_replication_factor   - 
site_search_factor   - 
site_mappings 5. Restart the master node 6. Run fixup splunk process
disable maintenance-mode on the master node to start the
7. Decommission each peer with splunk stop

site_mappings Example 1
Replace old peers in site2 with a new site
site1 site2 site3
available_sites = site1,site2 site_replication_factor = origin:1, total:2 site_search_factor = origin:1,total:2
available_sites = site1,site2,site3 site_replication_factor = origin:1,site1:1,site3:1,total:2 site_search_factor = origin:1,site1:1,site3:1,total:2 site_mappings = site2:site3

 
site_mappings Example 2
Replace all peers in site1 & site2
site1 site2 site3 site4
available_sites = site1,site2 site_replication_factor = origin:1, total:2 site_search_factor = origin:1,total:2
available_sites = site1,site2,site3,site4 site_replication_factor = origin:1,site3:1,site4:1,total:2 site_search_factor = origin:1,site3:1,site4:1,total:2 site_mappings = site1:site3,site2:site4

 
site_mappings Example 3
Migrate from multi-sites to a single site4
site1 site2 site3 site4
available_sites = site1,site2,site3 site_replication_factor = origin:1,site1:1,site3:1,total:2 site_search_factor = origin:1,site1:1,site3:1,total:2 site_mappings = site2:site3
available_sites = site1,site2,site3,site4 site_replication_factor = origin:2,site4:2,total:2 site_search_factor = origin:2,site4:2,total:2 site_mappings = default_mapping:site4

Note If a site used in a mapping is later decommissioned, its previous mappings must be remapped to an available site.

### Bucket Status and Fixup

- Bucket-fixing is the process of master node trying to meet the search and replication policies
  - Copies both rawdata and index data to meet the replication factor and
search factor
- The Bucket Status page displays fixup tasks and their states

 
### Cleaning-up Excess Bucket Replicas

- There might be extra copies of buckets in a cluster when a peer node rejoins a cluster after fixup has occurred
  - This does not affect searching, but consumes storage space
- To determine and manage these excess buckets, use Splunk Web or CLI on the master node
splunk list excess-buckets [index]
splunk remove excess-buckets [index]

### Monitoring Indexer Cluster with Metrics

- Internal logs under clustering may get rotated too fast and a diag may not provide sufficient information
  - For more insight, search:
index=_internal sourcetype=splunkd {bucket_id}
- metrics.log can provide service loads and job activities
index=_internal sourcetype=splunkd metrics name=cm* | stats values(group) AS groups by name
Cluster service loads based on the number of endpoint access and its response time
Scheduled tasks, finished tasks, tasks still pending
Peers have their own corresponding metrics

 
MC > Indexing > Indexer Clustering > Indexer Clustering: Service Activity
index=_internal sourcetype=splunkd host=cmaster metrics name=cmmaster_service | timechart avg(to_fix_gen) avg(to_fix_total) avg(to_fix_rep_factor) avg(to_fix_search_factor) span=30s
Examples: Indexer Cluster Health
1 2 3

### Primary Rebalancing

- To optimize the distribution of primary buckets, primary rebalancing occurs automatically when:
  - 
A peer node joins or rejoins the cluster
  - 
Master node rejoins the cluster
  - 
Rolling restart completes
- The master identifies duplicate searchable buckets and reassigns primaries to other peers that have the same buckets
  - 
This is to balance the search load across all peer nodes
  - 
This does not move searchable copies to different peer nodes
- Primary rebalancing occurs independently for each site in a multisite cluster
- Can also manually trigger the rebalancing process with the REST endpoint on the master
services/cluster/master/control/control/rebalance_primaries

### Data Rebalancing

- Uneven higher load bucket on certain distribution peer doesn't nodes
utilize storage optimally and can cause
- Uneven bucket distribution can occur:
  - After adding new peer nodes   - 
When forwarding data is skewed   - 
After frequent bucket fixups due to outages
- Data rebalancing redistributes the number of bucket copies per index
  - Operates on only warm and cold buckets, NOT hot buckets   - 
Rebalances all non-searchable, searchable and primary buckets   - 
In multisite cluster, data is rebalanced within a site as well as across sites

### Data Rebalancing CLI

- You run the command manually to initiate the data rebalancing
splunk rebalance cluster-data -action start [-index <index>]   - 
This process impacts search performance   - 
Perform during a maintenance window
ê Append the command parameter [-max_runtime <in_min>] splunk rebalance cluster-data -action status splunk rebalance cluster-data -action stop
- The goal is to achieve a practical balance, not a perfect balance
splunk edit cluster-config -rebalance_threshold 0.90   - 
The value of 1.00 means fully balanced
- Can master also node configure dashboard
the threshold and start the rebalancing process from the
  - Settings > Indexer clustering > Edit > Data Rebalance http://docs.splunk.com/Documentation/Splunk/latest/Indexer/rebalancethecluster

### Report and DM Acceleration Replication

- By model default, acceleration indexer clusters summaries
do not replicate report acceleration and data
  - Only primary buckets have associated summaries
- To enable summary replication, run this on the master node:
splunk edit cluster-config -summary_replication true Or, set summary_replication = true in server.conf
- All summaries searchable for that copies bucket
contain all the replicated
  - For each hot searchable buckets, the copy
cluster creates a summary for
  - For any warm/cold missing or buckets, out-of-date the summaries
cluster replicates to fill in
[clustering] summary_replication = true
Note
With summary replication enabled, summary-generating searches use more resources across the cluster. The searches may take longer to complete.

### Tsidx Reduction in Clustering

- You can trade some search performance for significant index size reduction with the Splunk storage optimization feature
  - Only non-hot buckets old enough qualify   - Certain metadata files get removed from the optimized buckets   - Produces minified buckets ending with a .mini.tsidx extension
- Minified buckets are searchable buckets
  - If the cluster needs a new searchable bucket, it first attempts to replicate
from the existing searchable copy   - If the cluster has no searchable copy, it rebuilds a full bucket and then
minifies it during the next minification schedule   - NOTE:
ê Commands such as tstats and typeahead do not work on minified buckets

### Peer Detention

- A peer enters the automatic detention state when the minFreeSpace threshold in server.conf is crossed
  - When a peer is in automatic detention, all indexing and replication is stopped, but the peer still responds to search requests
- You can also manually put a peer into a detention state
- Use cases for manual detention:
  - Before decommissioning a peer, make it available only for searches
  - Preempt automatic detention
  - Diagnose a suspect peer by blocking indexing and replication
  - Force new data to go to other peers
  - Slow the disk writes by only allowing indexing, but not replication

### Enabling Manual Detention

- From the master node:
splunk edit cluster-config -manual_detention <option>   - peers <guid1>, <guid2>,...
- From the peer:
splunk edit cluster-config -manual_detention <option>   - on closes the TCP, UDP, and HEC ports
ê Disables indexing and replication for all network-based or remote (forwarder) data inputs ê Still indexes local monitor and scripted inputs   - on_ports_enabled blocks incoming replication, but continues to index   - off disables the manual detention
- Checking the detention status
  - On the peer node: splunk list cluster-config   - On the master node: splunk show cluster-status
Or, Settings >Indexer Clustering >Peers   - On monitoring console: Indexing > Indexer Clustering > Indexer Clustering: Status

### Key Indexer Cluster Maintenance Commands

- Helpful CLI commands to run on the Master Node
splunk help clustering
splunk rolling-restart cluster-peers splunk [enable|disable|show] maintenance-mode splunk set indexing-ready splunk validate cluster-bundle splunk show cluster-bundle-status splunk apply cluster-bundle

### Monitoring Clusters with Monitoring Console

- Enable Monitoring Console (MC) on the system that has the best vantage point of the distributed deployment
  - MC is the search head of all search heads
- Enable only one MC instance in the entire deployment
  - A dedicated search head that only
administrators can access within the indexer cluster   - Deployer or dedicated license master   - Master Node   - DO NOT enable on a search head cluster
member or on a peer node
...
SH1 SHn LM
DPLYR MC Master
Peer1 Peer2 Peer3

### Monitoring Console Setup Prerequisites

- A user must have the admin_all_objects capability to configure the MC
- Each instance must use a unique servername and default-hostname
- Platform instrumentation is enabled for every instance (UF optional)
- Optionally on the master: splunk edit cluster-config -cluster_label idxc1
- Forward all indexes (including internals and summaries) from search heads and the master node to the indexing tier
[indexAndForward] index = false
[tcpout] defaultGroup = default-autolb-group forwardedindex.filter.disable = true indexAndForward = false
[tcpout:default-autolb-group] server=idx1:9997,idx2:9997,idx3:9997,idx4:9997 useACK = true outputs.conf 

### Post MC Configuration Checklist

- Add all instances, except peer nodes, as search peers to the MC
- After enabling the MC to run in distributed mode, verify that all instances were discovered and the server roles are correct
  - Make sure only peer nodes (indexers) are marked as indexers   - A search head that is also a license master should have both roles marked
  - If the correct roles are not selected, click Edit and update
- Use custom groups to organize related components
  - The custom groups are used for view selection in the MC dashboards
- For any changes in the environment, return to Setup,

Note check the server roles, and update if necessary
To propagate the changes, always click Apply Changes on the MC Setup page.

### Indexer Cluster Dashboards in MC

- Indexer Clustering: Status
  - Provides the same information as the master's indexer clustering page
- Indexer Clustering: Service Activity
  - For an ideal healthy cluster, most of the panels should be blank   - The trending down of fixup tasks and service jobs count is normal   - Pay attention to an increasing trend on pending tasks and jobs

### Lab Exercise 4   - Monitor CM Service Activities

- Time: 25 - 30 minutes
- Tasks:
  - Stage an app and deploy it to the peer nodes
  - Disable indexing on cmaster and dserver
  - Enable the Monitoring Console to run in distributed mode on dserver
  - Monitor the indexer clustering service activities from Monitoring Console