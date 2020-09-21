# Splunk Cluster Administration 7.0

## Module 2: Single-site Indexer Cluster

### Module Objectives

- Describe how Splunk single-site indexer clusters work
- Identify cluster components and terms
- Implement a single-site indexer cluster
- Search and review internal logs related to indexer clustering

### Single-site Indexer Cluster Overview

- Master node
  - There can only be one master
  - Controls and manages index replication
  - Distributes peer nodes apps and configurations to
- Peer nodes
  - Indexes data from inputs/forwarders
  - Replicates instructed by data the to master other peer nodes as
- Search head
  - Works head the same as any Splunk search
  - Required component of indexer cluster
- Forwarders
  - Send data to peer nodes

Index Replication cluster
Search Head
Master Node Distributed search
Data replication
Peer Nodes
Forwarder with useACK enabled

### Indexer Cluster Considerations

Benefits

- Data availability and fast recovery
- Easier overall administration
  - Coordinated indexer configuration management
  - Automatic distributed search setup
  - Elastic indexer discovery
  - Indexer peer node status dashboard on the master node
- Scale-out indexing capacity
- No additional cost for data replication

Trade Offs

- Increased storage requirements
- Increased processing load
  - Depending on the replication & search factors
- Requires additional Splunk instances 
  - Replication factor + CM + SH
- No support for heterogeneous indexers
  - Requires same OS and Splunk versions
- Requires cluster-specific deployment management

### Indexer Cluster System Requirements

- Each node must run on its own host
  - The master node must run the same or a later version than the peer nodes and search heads
  - The search heads must run the same or a later version than the peer nodes
  - All peer nodes must run EXACTLY the same version
- Peer node storage requirements should:
  - Be able to sustain 800 IOPS for each peer node   - Use fast attached storage or storage area networks (SAN) over fiber
    - The ratio of disks to disk controllers should mimic a database system requirement
- Cluster recovery time depends upon system resources available on master node

<http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Systemrequirements>

### Indexer Cluster Deployment Overview

1. Identify your clustering requirements
  - Replication policy, disk space, number of peer nodes, etc.
2. Install Splunk Enterprise and configure cluster instances
  - One master, at least two indexers, and one search head
3. Enable clustering on each cluster instance
  - Use Splunk Web UI, CLI, or manually edit server.conf
4. Create and distribute configuration bundles to the peer nodes
5. Forward data to the peer nodes

### Best Practices and Guidelines

- Plan, plan, and plan
  - A single cluster or segregated clusters (by sourcetype, department, or use case)
- Cluster instances should not share hardware
  - Dedicate hardware to the master node, search head, and peer nodes
  - All members share the same license pool
  - Each peer node must have its own storage
- Number of peer nodes is determined by:
  - Expected availability requirements of your organization
  - Level of replication required, daily data rate, retention policy, and concurrent users
    -Index replication does not increase your licensing usage
- Cannot use a deployment server to distribute configuration bundles directly to peer nodes

### Where to Install the Master Node

- On a dedicated host
  - Cannot be shared with a peer node or search head instance
  - Search capabilities are for debugging purposes
- Under certain limited circumstances, it can fulfill additional server roles
  - Built-in search head for debugging purposes
  - Monitoring Console
  - Deployer
  - License master

Note  Master node failover is discussed later in this module.

Note When hosting additional roles, the cluster should be under the following limits:

- 30 indexers
  - 10 indexes
  - 100,000 buckets
- 10 search heads

### Single-site Cluster: Key Specifications

- Peer nodes copy buckets to other peer nodes (index replication)
  - The copied buckets may be searchable buckets or contain only rawdata
- Replication factor
  - Specifies how many total copies of rawdata the cluster should maintain
  - Sets the total failure tolerance level
- Search factor
  - Specifies how many copies are searchable
    - Searchable buckets have both rawdata and index files
  - Cannot be larger than the replication factor
  - Determines how quickly you can recover the search capability
    - A trade-off between disk usage and search availability
- Security key (pass4SymmKey)
  - Authenticates communication between the cluster nodes
  - The key must be the same across all cluster instances

### Estimating Disk Usage

- Total rawdata disk usage = Rawdata total * replication factor
  - Assuming rawdata on disk = ~ 15% of indexed data
- Total index disk usage = Index data total * search factor
  - Assuming index files on disk = ~35% of indexed data

|Daily Index data = 100GB|RF=3 & SF=2 on 3 peer nodes|RF=3 & SF=2 on 6 peer nodes|RF=3 & SF=3 on 6 peer nodes|
|:--:|:--:|:--:|:--:|
|Rawdata (15% of 100GB)|15 * 3 = 45 GB|15 * 3 = 45 GB|15 * 3 = 45 GB|
|Index files (35% of 100GB)|35 * 2 = 70 GB|35 * 2 = 70 GB|35 * 3 = 105 GB|
|Total size across cluster|115 GB|115 GB|150 GB|
|Per Peer storage per day|115 / 3 = 38.3 GB|115 / 6 = 19 GB|150 / 6 = 25 GB|

The minimum burden on surviving nodes

### Configuring Splunk Cluster

- There are three ways to configure
  - Splunk Web, CLI, and server.conf
  - In this course, you will use CLI for configurations and Splunk Web for monitoring
- Enable clustering on the instances in the order of Master node > Peer nodes > Search heads
- Get help on Splunk cluster commands:
  - splunk help cluster
  - splunk help [list|edit] cluster-config

### Ports for Indexer Clustering

9997
Master Node
Forwarder with autoLB
Search Head 8089
8089
8089
8089
8089 Peer1 Peer2 Peer3
9100
9100
9100
9997
9997
Note Management (splunkd port)
To participate in this indexer
Replication (replication port)
cluster, all nodes -- including the search head -- must use the same Data (receiving port)
pass4SymmKey.

### Configuring Splunk Master Node

Enter the single command
- Splunk defaults to: > splunk edit cluster-config -mode master -replication_factor 2
  - replication_factor = 3
  - search_factor = 2 -search_factor 2 -secret mycluster
- The secret parameter is encrypted and saved as
Results in: SPLUNK_HOME/etc/system/local/server.conf
[clustering] mode = master replication_factor = 2 pass4SymmKey = Hashed_Secret
pass4SymmKey
  - Starting in 6.6, a non-default security key is required
  - mycluster is set as the password for this cluster example

### Configuring the Peer Nodes

> splunk enable listen 9997 > splunk edit cluster-config -mode slave
- Ports required on each peer:
  - Receiving port to listen to forwarders -master_uri https://10.0.1.3:8089 -secret mycluster -replication_port 9100
  - Replication port to communicate with
other peer nodes
- In this example:
SPLUNK_HOME/etc/system/local/server.conf
[clustering]
  - 9997 = the forwarder listening port   - 10.0.1.3 = master node address
mode = slave master_uri = https://10.0.1.3:8089 pass4SymmKey = Hashed_Secret
  - 8089 = master node's splunkd-port   - mycluster = same cluster password
[replication_port://9100]
  - 9100 = index replication port

### Configuring the Search Head

> splunk edit cluster-config
- To configure as a cluster search head: -mode searchhead -master_uri https://10.0.1.3:8089
  - 
-secret mycluster
edit cluster-config
- Functions as a regular search head
- For more help: SPLUNK_HOME/etc/system/local/server.conf
splunk help [list|add|edit|remove] [clustering] mode = searchhead
cluster-master
master_uri = https://10.0.1.3:8089 pass4SymmKey = Hashed_Secret1

### Adding SH to an Additional Indexer Cluster

> splunk add cluster-master
- SHs can belong to multiple clusters -master_uri https://20.0.2.6:8089 -secret yourCluster
- To allow SH to search additional clusters:
splunk add cluster-master
SPLUNK_HOME/etc/system/local/server.conf
[clustering] mode = searchhead master_uri = clustermaster:10.0.1.3:8089, clustermaster:20.0.2.6:8089
[clustermaster:10.0.1.3:8089] master_uri = https://10.0.1.3:8089 pass4SymmKey = Hashed_Secret1
[clustermaster:20.0.2.6:8089] master_uri = https://20.0.2.6:8089 pass4SymmKey = Hashed_Secret2

Master Dashboard   - Single-site Cluster
Master node: Settings > Indexer clustering

### Index Replication Health

Cluster status is When current operational RF is When current operational SF is Complete Met as specified Met as specified Valid One or greater
- With replication factor = 3 and search factor = 2:
  - A complete cluster has 3 copies of each bucket, 2 of which are searchable   - A valid cluster has at least one searchable copy of all buckets
- With replication factor = 2, search factor = 2, and 2 peers:
  - A complete cluster has 2 searchable buckets, each having its copy of
rawdata   - A valid cluster has at least one searchable copy of all buckets

### Factors in Action   - Normal

Complete
4 peer nodes, replication factor = 3, search factor = 2 & Valid
P Primary (Origin) B Searchable backup R
Rawdata-only
Peer node A Peer node B Peer node C Peer node D
1P
1B
2P
2R
3P
4P
4B
5P
5R

1R
3B
4R
Data replication
2B
3R
5B
 
Factors in Action   - Primary Loss
Complete & Valid
1P
2P
3P
4P
5P after
1P
1B
recovery
2B is complete
3P
4P
4B
5B

4 peer nodes, replication factor = 3, search factor = 2
P Primary (Origin) B Searchable backup R
Rawdata-only
Peer node A Peer node B Peer node C Peer node D
1R
2R
3R
4R
5R

Data replication
2P
3B
5P
 
### Factors in Action   - Second Peer Loss

Valid but
4 peer nodes, replication factor = 3, search factor = 2 Not complete
P B R
Peer node A Peer node B Peer node C Peer node D
will never be
1P
1P
1P
complete
2P
2B
3P
3P
4P
4P
4P
5P
5B

2P
3B
5P
Primary (Origin) Searchable backup Rawdata-only

1R
2R
3R
4R
5R
Data replication
 
### Factors in Action   - Peer Restored

Complete
4 peer nodes, replication factor = 3, search factor = 2 & Valid
P Primary (Origin) B Searchable backup R
Rawdata-only
Peer node A Peer node B Peer node C Peer node D
after
1B
1P
1P
1R rebalancing
2P
2B
2R Excess Buckets
3P
3R
3P
4B
4P
4P
4R
5P
5B
5R
6P 6B 6R

Data replication
2P
3B
5P
 
Excess Buckets
Lists excess buckets

### Notable Indexer Cluster Log Channels

- Cluster peers communicate via the /service/cluster endpoints
  - Peer to master communication: /services/cluster/master   - Master to peer communication: /services/cluster/slave
- splunkd_access.log   - Indexer cluster communication logs
  - Example:
index=_internal sourcetype=splunkd_access (uri="/services/cluster/slave/buckets*" OR uri="/services/cluster/master/buckets*") | convert ctime(_time) | table _time uri_path | sort _time
  - Higher response time indicates service overloading   - Response status 200 is good, anything else is not good
- splunkd.log   - indexer clustering activity logs   - component=CM* OR component=Cluster*   - Look for WARN/ERROR on the host=<master>
- metrics.log
  - component=Metrics group=clusterout_connections

### Master Node Failover

- If the master node is lost, the cluster continues to operate
  - New data arriving at peer nodes is indexed, but might not replicate
  - The search heads continue to send the queries to last known list of peer nodes and peer nodes will respond if they can
  - After the master node comes back online, buckets are re-balanced
- A stand-by master node can be configured
  - http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Handlemasternodefailure
- You can employ DNS-based failover, a load balancer, or some other technique to switch the same master_uri to the stand-by master
  - Hot-standby is NOT recommended

### Master Node Failover (cont.)

- A standby master only needs the primary master's static state information
  - SPLUNK_HOME/etc/system/local/server.conf
  - SPLUNK_HOME/etc/master-apps
- When the standby master node starts, its services are blocked until it can fulfill the replication factor
  - To have the standby master unblock immediately, run: splunk set indexing-ready

Note
Extra provisioning is required if the Monitoring Console is also enabled on the Master Node.

### Restarting Indexer Cluster

- Ordinarily you do not restart the entire cluster
  - Search heads can be restarted at any time
- If you do need to restart the entire cluster:

1. Restart the master node with splunk restart
2. Wait and check the master node dashboard for cluster status
3. Run splunk rolling-restart cluster-peers ê Performs a phased restart of all the peer nodes ê Restarts 10% of the peers at a time in random order (configurable) ê To cause all peers to restart immediately, run from the master:

> splunk edit cluster-config -percent_peers_to_restart 100 > splunk rolling-restart cluster-peers

### Migrating Non-clustered Indexers to a Cluster

- You can add a non-clustered indexer to a cluster as a peer node at any time splunk edit cluster-config -mode slave ...
- Apps must be re-distributed via master-apps
  - 
Note
You cannot convert a peer node to a non-clustered indexer.
You will learn about distributing apps in indexer clusters in Module 4
- Only new data coming into this peer is replicated
  - New data follows the cluster's replication factor
- Existing buckets are not replicated
  - Contact Splunk Professional Services if you must replicate legacy buckets

### Upgrading and Applying Maintenance Releases

- To upgrade a single-site indexer cluster, you must take down the entire cluster at once and upgrade all node tiers in order:
  - Master node > search heads > peer nodes   - Within each tier, upgrade all nodes as a single operation
- Maintenance updates can be applied in a rolling, online upgrade
  - No need to bring down the entire cluster at once   - Upgrade the tiers in the same prescribed order
ê Put the master into maintenance mode while upgrading the peer nodes ê To take down peer nodes, use the splunk stop command ê We will discuss maintenance mode command later http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Upgradeacluster

### Lab Exercise 2   - Enable Single-site Cluster

- Time: 35 - 40 minutes Tasks:
  - Switch all cluster members to license slaves
  - Configure the master node for a single-site indexer cluster
  - Configure three indexers to form the replication peers
  - Configure a search head to join the cluster
  - Monitor the cluster status with Splunk Web
  - Test a peer node failover scenario
  - Investigate the peer outage with Splunk internal logs

### Lab Exercise 2   - Enable Single-site Cluster (cont.)

Your Computer
x = Your student ID 8?89 = splunkd-port
Indexer Cluster (10.0.x.1)
cmaster 8089
1
dserver 8189
SSH you@10.0.x.1
2
8189 8289 8389 8489
SSH you@Public_DNS ➜
Search Heads (10.0.x.2)
fwdr 8289
SSH you@10.0.x.2
3
8189 8289 8389 8489
Jump Server (10.0.x.3)