# Splunk Cluster Administration 7.0

## Module 1: Large-scale Splunk Deployment Overview

### Module Objectives

- Review Splunk deployment options
- Understand factors that affect large-scale deployment design
- Describe how Splunk can scale
- Configure a Splunk License Master

### Review: Splunk Deployment Options

- Key Splunk functions:
  - Consumes data, stores indexed data, and searches indexed data
- Splunk scales by splitting its functionality across multiple dedicated instances

Indexer & Search Head
Standalone
Forwarders
Forwarders

Search Head
Deployment Server
Indexers

### Reference Servers for Distributed Deployment

Indexer Search Head
OS Linux or Windows 64-bit distribution
Network 1Gb Ethernet NIC (optional second NIC for a management network)
Memory 12 GB RAM
CPU Intel 64-bit chip architecture
12 CPU cores Running at 2+ GHz

- Ratio of indexers to search heads depends on the number of concurrent users and the indexing volume per node
- Hardware sizing is discussed in detail in Architecting Splunk course

Intel 64-bit chip architecture 16 CPU cores Running at 2+ GHz
Disk Disk subsystem capable of 800 IOPS 2 x 10K RPM 300GB SAS drives - RAID 1
<http://docs.splunk.com/Documentation/Splunk/latest/Capacity/Referencehardware>
<http://docs.splunk.com/Documentation/Splunk/latest/Capacity/Summaryofperformancerecommendations>

### Introducing Splunk Clustering

- Using commodity hardware, configure indexers to replicate indexes or group search heads to coordinate their search activities and loads
- Allows you to balance growth, speed of recovery, and overall disk usage

High Availability (HA) Disaster Recovery (DR)
Indexing Tier
Single-site cluster (index replication)

- Flexible replication policies

Multisite cluster

- Can withstand entire site failure
- Supports active-passive and active-active configurations

Search Tier

- Search head or
- Search head cluster

Search affinity (site-aware)

- Search head or
- Search head cluster

### Splunk Server Roles in Splunk Clusters

Load-balanced Forwarders
Monitoring Search Heads
Console
Clustered
Site 1 Clustered
Site 2
Deployer
Indexers
Master Node
Deployment Server
License Master

### Splunk Server Roles in Splunk Clusters (cont.)

|License Master|Allocates license capacity and manages license usage of all cluster members|
|:--:|:--|
|Master Node|Regulates the functioning of an indexer cluster|
|Peer Node|Indexer (search peer) that participates in an indexer cluster|
|Search Head|Participates in clusters as stand-alone or search head cluster member|
|Deployment Server|Centralized configuration manager for forwarders|
|Deployer|Distributes configurations to search head cluster members|
|Monitoring Console|Allows admins to monitor performance details regarding your Splunk environment|

- Splunk recommends that you dedicate a host for each role
  - You can enable multiple Splunk server roles on a server with caveats
    - You will learn more about caveats of each server role throughout this course

### License Master Configuration

- By default, every instance is a License Master
- All cluster members must share:
  - The same licensing pool   - The same licensing configuration
- Only incoming data counts against the license
  - Replicated data does not count
- Cannot use a free license for clustering
- You should forward license master's internal logs to the indexing layer
  - Do not count against the license   - Able to run searches against the license logs   - Any unusual condition can be visible on all search heads

### License Master Configuration (cont.)

- To add a license to a License Master, run:

```bash
splunk add licenses {path_to_license_file}
```

- To switch to a License Slave, run this command on each slave:

```bash
splunk edit licenser-localslave -master_uri https://{Lic_Master:Mport}
```

- To check the slave configuration of a particular node, run:

```bash
splunk list licenser-localslave
```

- To check the list of license slaves, run this command on the license master:

```bash
splunk list licenser-slaves
```

### Lab Exercise 1   - Configure Splunk License Master

- Time: 15 - 20 minutes
- Tasks:
  - Access your designated Splunk environment
  - Set up password-less SSH connection
  - Configure your License Master instance
  - Log into Splunk Web and verify the license information

### Lab Exercise 1   - Configure Splunk License Master (cont.)

x = Your student ID 8?89 = splunkd-port
Indexers (10.0.x.1)
cmaster 8089
dserver 8189
ssh you@10.0.x.1
3
8189 8289 8389 8489 Your Computer
1 ssh you@Public_DNS➜
2
Search Heads (10.0.x.2)
fwdr 8289
ssh you@10.0.x.2
8189 8289 8389 8489
Jump Server (10.0.x.3)

### Lab Exercise 1   - Configure Splunk License Master (cont.2)

4
Indexers
idx1 idx2 idx3 idx4
Search Heads <http://{Public_DNS}/{splunk_server}>
For example, <http://{Public_DNS}/dserver>
sh1 sh2 sh3 sh4
Jump Server
Public_DNS = Same as your jump server splunk_server = Splunk server name
cmaster dserver fwdr
Your Browser
