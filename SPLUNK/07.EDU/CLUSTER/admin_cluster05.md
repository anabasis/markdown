# Splunk Cluster Administration 7.0

## Module 5: Forwarder Configuration

### Module Objectives

- Use indexer discovery to configure forwarders in a clustered environment
- Describe optional indexer discovery settings
  - Polling rate
  - Weighted load balancing
- Optimize indexing loads with volume-based load balancing

### Best Practices for Configuring Forwarders

- Forwarders are not required to be configured for clustering
- Enable load balancing and indexer acknowledgement in outputs.conf

[tcpout:indexers]
server = <peer_node_list>
useACK = true

Note
autoLB is set to true by default. outputs.conf   - The receiving peer (origin) tracks the write state of indexing data when
useACK is enabled   - Acknowledges when indexing data is written to a bucket and sent to
replicating peers   - Any node or network failure causes a bucket to roll and triggers a fixup

### Forwarder Management Challenges

- In a non-clustered environment, Splunk administrators must track target server changes and update the forwarder outputs.conf settings manually
  - A static list of indexers has to be deployed to each forwarder
  - Target indexer changes require restarting the forwarders
IDX1 IDX2 IDX3 New [tcpout:indexers] server = IDX1:9997,IDX2:9997,IDX3:9997 useACK = true
9997 9997 ????
Forwarder

9997

### Using Indexer Discovery

- Designed for dynamic environments where capacity is added and removed on demand
  - Reduces the load on DNS servers   - Minimizes the forwarder restarts   - Can use weighted load balancing   - Scales well and easier to manage   - Can be site-aware
Site 1 Site 2
Master
Peer1 Peer2 Peer3 Peer4 Peer5 Node
1
Peers report their receiving ports to master node
8089
1
9997 9997 9997
9997
9997
2
Forwarders poll master node to get the latest list of peer nodes
2 3 4
3
Forwarders send data to the peers in the list
4
A peer can be added or removed without affecting the forwarder configurations
Forwarder with Indexer Discovery 

### Configuring Indexer Discovery   - Master Node

- No changes to the peer nodes to receive data from forwarders
splunk enable listen 9997
- No CLI support for enabling indexer discovery
- Enable indexer discovery on the master node
[indexer_discovery] pass4SymmKey = AnotherSecret Master node server.conf
  - The between pass4SymmKey the master specifies node and the the security forwarders
key used to communicate
ê Must use the same value on all forwarders and the master node   - The pass4SymmKey here is NOT the same cluster secret
ê For master better node security, and the use peer a different nodes
value than the one used between the

### Configuring Indexer Discovery   - Forwarders

Each forwarder's outputs.conf
Each forwarder's server.conf [tcpout:idxc-forwarders] indexerDiscovery = cluster1 useACK=true
- If both manual and indexer discovery attributes are set, indexer discovery takes precedence
  - Forwarders poll the master at set
intervals to receive the most recent list of peers
[general] site = <site-id>
[indexer_discovery:cluster1]
- For multisite configuration, you master_uri = https://<master>:8089 pass4SymmKey = AnotherSecret
must also assign a site association
- site = site0 configures the forwarder to transmit data to all peer nodes at all sites

### Indexer Discovery Option   - Polling Rate

- The master determines a polling interval dynamically based on the number of connected forwarders and the polling_rate
  - poll_interval (seconds) = #_of_forwarders / polling_rate + 30
  - polling_rate is a fixed factor that the master uses to calculate the polling interval
    - Set a factor between 1 and 10 (the default is 10)
# of forwarders polling_rate polling_int_sec polling_interval
Master Node server.conf 100 1 100/1+30=130 2 min. 10 sec.
[indexer_discovery] 100 1,000 10 100/10+30= 40 40 sec. 10 1000/10+30=130 2 min. 10 sec.
pass4SymmKey = <string> polling_rate = <1-10>

### Balancing Indexing Loads

- Evenly distributed index data greatly improves the search performance
- The default load balancing on the forwarder is based on time
  - Time-based forwarding alone cannot distribute data evenly across peers
- Other options:
  - Weighted load balancing
  - Volume based data forwarding
  - Event breaker (discussed in Splunk Enterprise Data Administration course)

### Using Weighted Load Balancing

- With indexer discovery, the master can adjust the autoLB server list based on the peers' relative storage capacities
  - More frequently selects peers with higher advertised capacity
selection_ratio = peer_capacity / cluster_wide_capacity
- Peers with more storage capacity, typically new peers, can become preferred search peers because a larger percentage of recent data will be indexed on these peers
Enable it on the master's server.conf Specify the advertised capacity peer nodes (optional)
[indexer_discovery]
[clustering] indexerWeightByDiskCapacity = true
advertised_disk_capacity = <10~100%>

### Using Volume-based Data Forwarding

- Data rebalancing is NOT bucket age aware
  - Recent data can still concentrate on particular peers, while the overall
buckets are evenly distributed   - Therefore, it is preferred to balance the data coming from the forwarders
- With volume-based forwarding, a forwarder can distribute more evenly
  - Sends a predefined amount of data before switching to another peer
- To enable:
  - Set autoLBVolume=<size_in_bytes> in outputs.conf
ê Set the autoLBVolume size in multiples of 64KB (65536)   - Set EVENT_BREAKER_ENABLE=true and its associated attributes in
props.conf
- autoLBVolume and autoLBFrequency settings work in conjunction
  - Whichever threshold is met first, switch to the next peer node

### Forwarder Site Failover

- You can configure site-awareness for forwarders in a multisite cluster
  - What happens to forwarding in case of a site failure?
- Solution: enable the forwarder site failover
  - Forwarders must use indexer discovery   - Send data to a secondary site when all nodes on the primary site are down   - When a node from the primary site is back online, forwarding resumes to the primary site
- To configure, run from the master node:
splunk edit cluster-config -forwarder_site_failover <primary>:<failover>
[clustering]
[general] mode = master
site = site1 multisite = true available_sites = site1,site2, site3
Each forwarder's server.conf forwarder_site_failover = site1:site2,site2:site3 ... [indexer_discovery] pass4SymmKey = whatever
Master node's server.conf

### Indexer Discovery Log Channels

- Search the master node's indexer discovery log channel for any errors:
  - index=_internal component=CMIndexerDiscovery
- Search the forwarder's splunkd.log for any indexer discovery events:
  - index=_internal component IN (IndexerDiscoveryHeartbeatThread,
HttpPubSubConnection, TcpOutputProc)
- Search the forwarder's metrics.log for data distribution and failover:
  - index=_internal host=uf component=Metrics group=tcpout_connections
| timechart span=1h sum(kb) by destIp

### Lab Exercise 5   - Configure a Forwarder

- Time: 30 - 35 minutes
- Tasks:
  - On the master node, enable the indexer discovery option with forwarder site failover
  - Configure the deployment server
  - Enable the deployment client setting on the forwarder
  - Update the instance server role in Monitoring Console
  - Verify the forwarder app deployment
  - Test the forwarder site failover scenario

### Lab Exercise 5   - Configure a Forwarder (cont.)

x = Your student ID 8?89 = splunkd-port
Indexer Cluster (10.0.x.1)
cmaster 8089
SSH you@10.0.x.1
1
SSH you@Public_DNS ➜ http://{Public_DNS}/dserver ➜
dserver (Deployer) 8189
8189 8289 8389 8489
Search Heads (10.0.x.2) 2
fwdr 8289
SSH you@10.0.x.2
3
8189 8289 8389 8489
Jump Server (10.0.x.3)

### Lab Exercise 5   - Configure a Forwarder (cont.)

Your Browser
Indexer Cluster
idx1 idx2 idx3 idx4 http://{Public_DNS}/{splunk_server} For example:
Search Heads http://{Public_DNS}/sh1 http://{Public_DNS}/sh2
4 4 sh1 sh2 sh3 sh4
Jump Server
Public_IP = Same as your jump server splunk_server = Splunk server name
cmaster dserver fwdr