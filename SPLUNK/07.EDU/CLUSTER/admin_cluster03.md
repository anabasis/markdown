# Splunk Cluster Administration 7.0

## Module 3: Multisite Indexer Cluster

### Module Objectives

- Describe how Splunk multisite indexer clusters work
- Identify multisite terms
- Implement a multisite indexer cluster
- Describe optional configuration settings

### Key Benefits of Multisite Indexer Cluster

- Allows for an extra layer of data partitioning
  - Indexers are grouped by “sites”
- Multisite clusters offer two key benefits:
1.
Disaster recovery
HQ (site1)
Replicate
ê Stores index copies at multiple sites
(i.e. geo-location or rack) ê Provides automatic site-failover capability ê In case of a disaster, indexing and
Replicate searching continue on the surviving sites
2.
Search affinity ê Preferentially searches assigned site ê Greatly reduces WAN network traffic
Multisite Cluster

AU (site2)
UK (site4) JP (site3)
 
Multisite Cluster Key Attributes
  - 
Property Name Description multisite Enables multisite clustering site - A logical group that shares clustering policies
- Also the site where the master node resides available_sites - Defines the sites in the cluster
- Supports up to 63 site_replication_factor Controls how to distribute raw copies of data among the sites site_search_factor Controls how to distribute searchable copies
site_replication_factor origin:2,total:3
Replication factor
  - 
site_search_factor origin:1,total:2
Search factor
Determines which sites get replicas and how many

Site Replication Factor Examples
Configuration Description origin:2, total:3 Default. Put the extra copy on a site
that doesn’t have a copy origin:1, total:4 (where there are 4 sites) Try to put a copy on any site that
doesn’t have one origin:2, site1:2, total:5 Both site1 and origin have a minimum
of 2 copies origin:2, site1:1, total:4 If origin happens to be site1, then the
higher value takes precedence origin:2, site1:2, site2:2, total:3 Invalid

### Multisite Clustering Use Cases

- Seamlessly route searches to a redundant site in case of a site failure
- Provides optimal search performance by using local site data in a geographically dispersed user environment
- An interesting edge case:
  - 
Configure a primary site equipped with high-performing systems
  - 
Indexing and searching only happen on this primary site
  - 
Replicate only rawdata copies to a failover site, perhaps equipped with older and less-capable systems
ê Site replication factor: origin:2, total:3 ê Site search factor: origin:2, total:2

### Multisite Indexer Cluster Deployment

1. Determine multisite cluster use cases and requirements 2. Install Splunk Enterprise and configure cluster instances
  - One master node
  - At least two peer nodes per site
  - At least one search head per site (optional) 3. Enable clustering on the instances in the order of
Master Node > Peer Nodes > Search Heads
  - Splunk CLI, or manually edit server.conf 4. Create and distribute the configuration bundle to the peer nodes 5. Configure forwarders to send data to the peer nodes

### Multisite Cluster Topology

SH1 Master
SH2 Node
8089 8089
8089
Site1 Site2
8089
8089
8089
8089 Peer1 Peer2 Peer3 Peer4
9100
9100
9100
9100
9997
9997
9997
9997
Management (splunkd port)
Replication (replication port)
Data (receiving port)
Forwarder with autoLB

### Configuring the Multisite Master Node

splunk edit cluster-config -mode master -multisite true -site site1 -available_sites site1,site2 -site_replication_factor origin:1,total:2 -site_search_factor origin:1,total:2 -secret mycluster
SPLUNK_HOME/etc/system/local/server.conf [general] site = site1
[clustering] multisite = true mode = master available_sites = site1,site2 site_replication_factor = origin:1,total:2 site_search_factor = origin:1,total:2 pass4SymmKey = Hashed_Secret

### Configuring Multisite Cluster Peer Nodes

splunk edit cluster-config -master_uri https://10.0.1.3:8089
Peer1&2 -mode slave -site site1 -replication_port 9100 -secret mycluster
splunk edit cluster-config -master_uri https://10.0.1.3:8089 -mode slave -site site2 -replication_port 9100 -secret mycluster
Peer1&2 server.conf Peer3&4 server.conf [general]
[general] site = site1
site = site2
[clustering]
[clustering] mode = slave
mode = slave master_uri https://10.0.1.3:8089
master_uri https://10.0.1.3:8089 pass4SymmKey = Hashed_Secret
pass4SymmKey = Hashed_Secret
[replication_port://9100]
[replication_port://9100]

Peer3&4
 
Configuring Multisite Cluster Search Heads
- Search heads can:
  - 
Join the indexer cluster at any time
  - 
Participate in multiple indexer clusters
  - 
Combine searches across clustered and non-clustered search peers
- Steps to configure a SH to search multiple indexer clusters:
  - 
If not already a cluster search head, enable it first:
splunk edit cluster-config -mode searchhead ...   - 
To add the search head to another indexer cluster, run: splunk add cluster-master <master_uri:port> ...   - 
If you need to change the clustering configuration or attributes:
splunk edit cluster-master <master_uri:port> ...

### Configuring a New Multisite Search Head

Enable a new instance (SH2) as a cluster search head
splunk edit cluster-config -mode searchhead -master_uri https://10.0.1.3:8089 -site site2 -secret mycluster
SH2 server.conf [general] ... site = site2
...
[clustering] master_uri = https://10.0.1.3:8089 mode = searchhead multisite = true pass4SymmKey = Hashed_Secret

### Configuring an Existing SH to Multisite

- Convert existing single-site cluster search head (SH1) to multisite mode
splunk edit cluster-master https://10.0.1.3:8089 -multisite true   - site site1 -secret mycluster
- Enable the converted SH1 to search an additional single-site cluster splunk add cluster-master https://20.0.2.6:8089 -multisite false -secret 2ndCluster
[clustering] master_uri = clustermaster:10.0.1.3:8089,clustermaster:20.0.2.6:8089 mode = searchhead
[clustermaster:10.0.1.3:8089] master_uri = https://10.0.1.3:8089 multisite = true pass4SymmKey = Hashed_Secret
SH1 server.conf site = site1
[clustermaster:20.0.2.6:8089] master_uri = https://20.0.2.6:8089 multisite = false pass4SymmKey = Hashed_Secret2

### Master Node View   - Multisite Cluster

Search Affinity
- In single-site mode, there is only one set of “primary” searchable buckets that respond to searches
- With multisite, each site can have searchable replicas that respond to searches
- Search affinity (enabled by default)
  - 
Search heads have a site association
  - 
Searches get as many events as they can from the same site
ê If a searchable bucket exists on the site, it will be the primary bucket for
that site ê Searches will extend across sites only when they are needed
  - 
Limit the access of each user to only their local search heads

### Multisite Factors in Action

- 2 sites
- 4 peer nodes
- 2 search heads
- 1 master node
- site_replication_factor:
origin: 2 total: 3
- site_search_factor:
origin: 1 total: 2
1O
2O
3O
O
S
R
Cluster A
Search Head A Master Node Search Head B
Complete & Valid
Peer A Peer B Peer C Peer D
1R
2R
3R
Origin
Searchable backup
Rawdata-only
Site 1 Site 2 
1S
2S
3S

### Multisite Factors in Action   - Primary Loss

- 2 sites
- 4 peer nodes
- 2 search heads
- 1 master node
- site_replication_factor:
origin: 2 total: 3
- site_search_factor:
origin: 1 total: 2
1O
2O
3O
O
S
R
Search Head A Master Node Search Head B
Peer A Peer B Peer C Peer D Valid but
1S Not complete
2S origin:2 is not possible
3S
Origin
Searchable backup
Rawdata-only

Cluster A
Site 1
1S
2S
3S
Site 2

### Multisite Factors in Action   - Site Loss

- 2 sites
- 4 peer nodes
- 2 search heads
- 1 master node
- site_replication_factor:
origin: 2 total: 3
- site_search_factor:
origin: 1 total: 2
1O
2O
3O
O
S
R
Search Head A Master Node Search Head B
Peer A Peer B Peer C Peer D Valid but
1S
1S Not complete
2S
2S
3S
3S
Origin
Searchable backup
Rawdata-only

Cluster A
Site 2 Site 1

 
Example: site1 Down
- Pending
  - 
A replication failed   - Will transition to
the next flag based on the subsequent heartbeat check
- Down
  - 
The peer went offline for some unknown reason

### Disabling Search Affinity

- You can disable search affinity for overall search performance
  - Spread the search request across indexers on all sites
  - Will increase WAN traffic
- To disable search affinity, edit the search head configuration
  - IMPORTANT: all sites must be in close proximity with very low network
latency
splunk edit cluster-master https://10.0.55.3:8089 -site site0
[clustermaster:10.0.55.3:8089] master_uri = https://10.0.55.3:8089 multisite = true pass4SymmKey = Hashed_Secret site = site0 SH2 server.conf (Search affinity disabled)

### Master Node Failover

- Preparing for master node failover is the same for both single site and multisite clustering
- Remember that splunk set indexing-ready unblocks master node services immediately
  - A cluster can be in a state where it CANNOT fulfill the
site_replication_factor
origin:1, site1:1, site2:1, site3:1, total:4
  - Or, the site where the master goes down and a stand-by master starts up on another site
- In this circumstance, run the command every time you restart the master

### Restarting a Multisite Indexer Cluster

- Again, ordinarily you do not restart the entire cluster
  - Search heads can be restarted at any time
- If you must restart the entire cluster:

1. Restart the master node with splunk restart
2. Wait and check the master dashboard for cluster status
3. For site-aware restarts, run: splunk rolling-restart cluster-peers -site-by-site true -site-order site2,site1,site3 Or for non site-aware restarts, run: splunk rolling-restart cluster-peers

### Migrating from Single-site to Multisite

- Make sure all cluster nodes are running the same Splunk Enterprise version 1. Change the master node to multisite mode and restart
  - DO NOT remove the existing single-site replication factor and search factor   - New multisite factors must be at least as large as the single-site factors 2. Enable maintenance mode on the master:
splunk enable maintenance-mode 3. Change peer nodes to multisite mode with a site association and restart
  - DO NOT restart a peer that hasn’t been converted 4. Change search heads to multisite mode and restart
splunk edit cluster-master https://CMaster:8089 -multisite true -site site1   - secret mycluster 5. Disable maintenance mode on the master:
splunk disable maintenance-mode
 
### Migration Notes

- Multisite policies apply to new data only
- Existing non-clustered buckets will not replicate; they just age out
- Existing single-site buckets follow the existing policies until they age out
  - Do not remove the existing single-site replication attributes
  - Multisite total values must be larger than the single-site factors
ê Must reduce the single-site factors to match the least number of peers
on any site

### Upgrading and Applying Maintenance Releases

- With multisite indexer cluster, you can upgrade one site at a time
  - No service interruption during the upgrade   - Only minor version upgrades (ie., from 7.0 to 7.0.x) 1. Upgrade the master node and run splunk enable maintenance-mode 2. Stop all the peers and search heads on site1 and upgrade 3. Run splunk disable maintenance-mode and wait until the cluster
comes to the complete state 4. Run splunk enable maintenance-mode 5. Stop all the peers and search heads on site2 and upgrade 6. Run splunk disable maintenance-mode
- Maintenance releases should be applied in the same order

### Further Reading: Clustering

- Basic clustering concepts for advanced users
  - http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Basicconcepts
- Master site failover
  - http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Mastersitefailure
- Configure the search head
  - http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Configurethesearchhead

### Lab Exercise 3   - Migrate to Multisite Cluster

- Time: 45 - 50 minutes
- Tasks:
  - Migrate the single-site master node to the multisite mode
  - Migrate the existing peer nodes and add a new peer
  - To configure site2, convert the 3rd peer (IDX3) and bring up a new peer (IDX4)
  - Convert SH1 to site1 search head and add SH2 to site2
  - Test the indexer site failover scenario
    - Stop site1 processes ê Check the cluster status and verify the search affinity

### Lab Exercise 3   - Migrate to Multisite Cluster (cont.)

Your Computer
x = Your student ID 8?89 = splunkd-port
Indexer Cluster (10.0.x.1)
cmaster 8089
dserver 8189
SSH you@10.0.x.1 1
2 3 8189 8289 8389 8489
SSH you@Public_DNS ➜
Search Heads (10.0.x.2)
fwdr 8289
SSH you@10.0.x.2
4 5 8189 8289 8389 8489
Jump Server (10.0.x.3)

### Lab Exercise 3   - Migrate to Multisite Cluster (cont.)

Your Browser 6
7 Indexer X
Cluster X idx1 idx2 idx3 idx4 http://{Public_DNS}/{splunk_server} For example:
Search Heads
http://{Public_DNS}/cmaster http://{Public_DNS}/sh1
sh1 sh2 sh3 sh4
Jump Server
Public_IP = Same as your jump server splunk_server = Splunk server name
cmaster dserver fwdr
