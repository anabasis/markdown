# Managing Indexers and Clusters of Indexers

## Deploy and Configure a multisite indexer cluster

### Multisite indexer cluster deployment overview

Before reading this topic, see:

* "[Indexer cluster deployment overview](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Clusterdeploymentoverview)". That topic provides a general overview of deployment for both single-site and multisite indexer clusters. The topic you are reading now describes only the multisite differences.

Important: This chapter assumes that you are deploying independent search heads in the multisite indexer cluster. For information on how to incorporate search heads that are members of a search head cluster, see "[Integrate the search head cluster with an indexer cluster](http://docs.splunk.com/Documentation/Splunk/7.1.0/DistSearch/SHCandindexercluster)" in the Distributed Search manual.

#### Migrating from a single-site cluster

To migrate from a single-site to a multisite indexer cluster, read "[Migrate an indexer cluster from single-site to multisite](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Migratetomultisite)".

#### Deploy a multisite indexer cluster

To deploy a multisite cluster, you configure the set of nodes for each site:

* A single master resides on one of the sites and controls the entire multisite cluster.
* A set of peer nodes resides on each site.
* A search head resides on each site that searches cluster data. If you want all searches to be local, you must install a search head on each site. This is known as search affinity.

For example, to set up a two-site cluster with three peers and one search head on each site, you install and configure these instances:

* One master node on one of the sites, either site 1 or site 2
* Three peer nodes on site 1
* Three peer nodes on site 2
* One search head on site 1
* One search head on site 2

Note: The master itself is not actually a member of any site, aside from its physical location. However, each master has a built-in search head, and that search head requires that you set a site attribute in the master's configuration. You must specify a site for the master, even if you never use its built-in search head. Note that the search head is for testing only. Do not use it for production purposes.

#### Configure multisite nodes

To deploy and configure multisite cluster nodes, you must directly edit server.conf or use the CLI. You cannot use Splunk Web.

##### Multisite-specific configuration settings

When you deploy a multisite cluster, you configure the same settings as for single-site, along with some additional settings to specify the set of sites and the location of replicated and searchable copies across the sites.

On the master, you:

* Enable the cluster for multisite.
* Enumerate the set of sites for the cluster.
* Set a multisite replication factor.
* Set a multisite search factor.

* Adjust the single-site replication and search factors as necessary. See "[Multisite cluster does not meet its replication or search factors](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Bucketreplicationissues#Multisite_cluster_does_not_meet_its_replication_or_search_factors)."

On each cluster node, you:

* Identify the site that the node resides on.

##### Configure with server.conf

To configure a multisite master node with server.conf, see "[Configure multisite indexer clusters with server.conf](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisiteconffile)".

##### Configure with the CLI

To configure a multisite master node with the CLI, see "[Configure multisite indexer clusters with the CLI](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/MultisiteCLI)"

#### Use indexer discovery with a multisite cluster

If you are using indexer discovery to connect forwarders to the peer nodes, you must assign a site to each forwarder. See "[Use indexer discovery in a multisite cluster](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/indexerdiscovery#Use_indexer_discovery_in_a_multisite_cluster)."

### Implement search affinity in a multisite indexer cluster

One of the key benefits of multisite indexer clustering is that it allows you to configure a cluster so that search heads get search results only from data stored on their local sites. This reduces network traffic while still providing access to the entire set of data, because each site contains a full copy of the data. This benefit is known as search affinity.

For example, say you have two data centers in California, one in San Francisco and the other in Los Angeles. You set up a two-site cluster, with each site corresponding to a data center. Search affinity allows you to reduce long-distance network traffic. Search heads at the San Francisco data center get results only from the peers in San Francisco, while search heads in Los Angeles get results only from their local peers.

#### How search affinity works

For those sites that you want to support search affinity, you must configure multisite clustering so that the site has a full set of searchable data and a local search head. The search head on any particular site then gets data only from its local site, as long as that site is valid.

When search affinity is functioning, each search head sends its searches to all peers, across all sites, but only the local peers search their data and return results to the search head.

If a local peer holding some of the searchable data goes down and the site temporarily loses its valid state, the search head will, if necessary, get data from peers on remote sites while the local site is undergoing bucket fixing. During this time, the search head will still get as much of the data as possible from the local site.

Once the site regains its valid state, new searches again occur across only the local site.

For more details on how the cluster handles search affinity, see "[Multisite indexer cluster architecture](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisitearchitecture)" and "[Search locally in a multisite cluster](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Howclusteredsearchworks#Search_locally_in_a_multisite_cluster)".

#### Implement search affinity

With multisite clusters, search affinity is enabled by default. However, you must perform a few steps to take advantage of it. Specifically, you must ensure that both the searchable data and the search heads are available locally.

To implement search affinity:

1. Configure the site search factor so that you have at least one searchable copy on each site where you require search affinity.

    One way to do this is to explicitly specify a search factor for each site that requires search affinity. For example, a four-site cluster with site_search_factor = origin:1, site1:1, site2:1, total:3 ensures that both site1 and site2 have searchable copies of every bucket. The third set of searchable copies will be spread across the two non-explicit sites, with no guarantee that either site will have a full set of searchable copies. Thus, search affinity is enabled for only site1 and site2. Site1 and site2 will each hold primary copies of all buckets.

    There are also ways to configure the site search factor to ensure that all sites have searchable copies, even without explicitly specifying some or all of them. For example, a three-site cluster with site_search_factor = origin:1, total:3 guarantees one searchable copy per site, and thus enables search affinity for each site. Each site will have primary copies of all buckets.

    For more information on how replication and search factors distribute copies across sites, see "[Configure the site replication factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitereplicationfactor)" and "[Configure the site search factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitesearchfactor)".

2. Deploy a search head on each site where you require search affinity.

#### Disable search affinity

You can disable search affinity for any search head. When search affinity is disabled, the search head does not attempt to obtain search results from a single site only. Rather, it can obtain results from multiple sites. This can be useful, for example, if you have two data centers in close proximity with low latency, and you want to improve overall performance by spreading the processing across indexers on both sites.

##### What happens when search affinity is disabled

When search affinity is disabled on a search head, search results can come from indexers on any or all sites. If the site search factor stipulates searchable bucket copies on multiple sites, the search head uses undefined criteria to choose which of the searchable copies to search. It is likely to choose some bucket copies from one site and other bucket copies from other sites, so the results will come from multiple sites.

Search heads always select from primary bucket copies. For example, say you have a two site cluster with this search factor:

```properties
site_search_factor = origin:2, total:3
```

The origin site will store two searchable copies and the second site will store one searchable copy of each bucket. So, for some buckets (those originating on site1), site1 will have two searchable copies and for other buckets (those originating on site2), site2 will have two searchable copies. Each site, however, has only a single primary copy.

A search head with search affinity enabled limits its searches to the primary copies on its own site, when possible.

In contrast, a search head with search affinity disabled distributes its search across primary copies on both sites. For a given bucket, you cannot know whether it will select the primary on site1 or the primary on site2. It does tend to use the same primaries from one search to the next.

##### How to disable search affinity

To disable search affinity for a search head, set the search head's site value to "site0" in server.conf:

```properties
[general]
site = site0

[clustering]
multisite = true
...
```

By setting site=site0, you cause searches to behave like they would on a single-site cluster, with no preference for any particular site.

For more information on configuring multisite search heads, see "[Configure the search heads](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisiteconffile#Configure_the_search_heads)."

### Configure multisite indexer clusters with server.conf

#### Read this first

Before reading this topic, see:

* "[Multisite indexer cluster deployment overview](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisitedeploymentoverview)". This topic provides important background information about configuring a multisite cluster.
* "[Configure the indexer cluster with server.conf](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Enableclustersindetail)". This topic explains the basics of cluster configuration. It focuses on single-site clusters, but most of the information is relevant to multisite clusters as well.

#### How multisite configuration differs from single-site configuration

You configure multisite indexer clusters in a similar way to how you configure clusters for a single site, with the exception of a few new attributes:

On all multisite cluster nodes (master/peers/search heads):

* The site attribute specifies the site that a node resides on.

On the master node and search head:

* The multisite attribute indicates that the master or search head is participating in a multisite cluster.

On the master node only:

* The available_sites attribute names the sites that the master is managing.
* The site_replication_factor replaces the standard replication_factor used with single-site clusters. For details, see "[Configure the site replication factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitereplicationfactor)".
* The site_search_factor replaces the standard search_factor used with single-site clusters. For details, see "[Configure the site search factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitesearchfactor)".

Important: Although the site_replication_factor effectively replaces the single-site replication_factor, and the site_search_factor replaces single-site search_factor, those single-site attributes continue to exist in the master's configuration, with their default values of 3 and 2, respectively. This can cause problems on start-up if any site has fewer peer nodes than either of those values; that is, if any site has only one or two peer nodes. The symptom will be a message that the multisite cluster does not meet its replication or search factor. For example, if one of your sites has only two peers, the default single-site replication factor of 3 will cause the error to occur. To avoid or fix this problem, you must set the single-site replication and search factors to values that are less than or equal to the smallest number of peers on any site. In the case where one site has only two peers, you must therefore explicitly set the replication_factor attribute to a value of 2. See "[Multisite cluster does not meet its replication or search factors](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Bucketreplicationissues#Multisite_cluster_does_not_meet_its_replication_or_search_factors)."

If you are migrating a cluster from single-site to multisite, you must keep the existing replication_factor and search_factor attributes for the existing single-site buckets, while also adding the new multisite site_replication_factor and site_search_factor attributes for the new multisite buckets. See "[Migrate an indexer cluster from single-site to multisite](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Migratetomultisite)".

#### Configure multisite cluster nodes

To configure a multisite cluster, you configure the nodes for each site, editing each node's server.conf file. For details on the clustering attributes, read the server.conf specification.

##### Site values

Site values identify the site on which a node resides. You assign a site value to each node in a multisite cluster. To do this, you set the site attribute in the node's [general] stanza.

Site values have the syntax:
site\<n\>

where \<n\> is an integer in the range of 0 to 63: site1, site2, site3, ....

For example:

site=site1

The special value "site0" can be set only on search heads or on forwarders that are participating in indexer discovery:

* For a search head, "site0" disables search affinity for the search head. See [Disable search affinity](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisitesearchaffinity#Disable_search_affinity).
* For a forwarder participating in indexer discovery, "site0" causes the forwarder to send data to all peer nodes across all sites. See [Use indexer discovery in a multisite cluster](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/indexerdiscovery#Use_indexer_discovery_in_a_multisite_cluster).

##### Configure the master node

You configure the key attributes for the entire cluster on the master node. Here is an example of a multisite configuration for a master node:

```properties
[general]
site = site1

[clustering]
mode = master
multisite = true
available_sites = site1,site2
site_replication_factor = origin:2,total:3
site_search_factor = origin:1,total:2
pass4SymmKey = whatever
cluster_label = cluster1
```

This example specifies that:

* the instance is located on site1.
* the instance is a cluster master node.
* the cluster is multisite.
* the cluster consists of two sites: site1 and site2.
* the cluster's replication factor is the default "origin:2,total:3".
* the cluster's search factor is "origin:1,total:2".
* the cluster's security key is "whatever".
* the cluster label is "cluster1."

Note the following:

* You specify the site attribute in the [general] stanza. You specify all other multisite attributes in the [clustering] stanza.
* You can locate the master on any site in the cluster, but each cluster has only one master.
* You must set multisite=true.
* You must list all cluster sites in the available_sites attribute.
* You must set a site_replication_factor and a site_search_factor. For details, see Configure the site replication factor and Configure the site search factor.
* The pass4SymmKey attribute, which sets the security key, must be the same across all cluster nodes. See Configure the indexer cluster with server.conf for details.
* The cluster label is optional. It is useful for identifying the cluster in the monitoring console. See Set cluster labels in Monitoring Splunk Enterprise.

Important: When the master starts up for the first time, it blocks indexing on the peers until you enable and restart the full replication factor number of peers. For example, given a three-site cluster with "site_replication_factor = origin:2, site1:1, site2:2, site3:3, total:8", the master blocks indexing until there are at least eight peers in total across all sites, including at least one in site1, two in site2, and three in site3.

Do not restart the master while it is waiting for the peers to join the cluster. If you do, you will need to restart the peers a second time.

##### Configure the peer nodes

To configure a peer node in a multisite cluster, you set a site attribute in the [general] stanza. All other configuration settings are identical to a peer in a single-site cluster.

Here is an example configuration for a multisite peer node:

```properties
[general]
site = site1

[replication_port://9887]

[clustering]
master_uri = https://10.152.31.202:8089
mode = slave
pass4SymmKey = whatever
```

This example specifies that:

* the instance is located in site1. A peer can belong to only a single site.
* the peer will use port 9887 to listen for replicated data streamed from the other peers. You can specify any available, unused port as the replication port. Do not re-use the management or receiving ports.
* the peer's cluster master is located at 10.152.31.202:8089.
* the instance is a cluster peer ("slave") node.
* the security key is "whatever".

##### Configure the search heads

Multisite search heads can provide search affinity. For information, see "[Implement search affinity in a multisite indexer cluster](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisitesearchaffinity)".

To configure a search head in a multisite cluster, you set a site attribute in the [general] stanza and a multisite attribute in the [clustering] stanza. All other configuration settings are identical to a search head in a single-site cluster. Here is an example configuration for a multisite search head node:

```properties
[general]
site = site1

[clustering]
multisite = true
master_uri = https://10.152.31.202:8089
mode = searchhead
pass4SymmKey = whatever
```

This example specifies that:

* the instance is located in site1. A search head can belong to only a single site per master.
* the search head is a member of a multisite cluster.
* the search head's cluster master is located at 10.152.31.202:8089.
* the instance is a cluster search head.
* the security key is "whatever".

Note: You can also use server.conf to enable multi-cluster search, in which the search head searches across multiple clusters, multisite or single-site. For searching across multiple multisite clusters, you can configure the search head to be a member of a different site on each cluster. For details, see "Configure multi-cluster search for multisite clusters".

When reconfiguring a search head that is up-and-running, Splunk recommends that you use the CLI command described in "Configure multisite indexer clusters with the CLI", rather than editing server.conf directly. If you use the CLI, you do not need to restart the search head.

#### Restart the cluster nodes

##### After initial configuration

After configuring instances as multisite cluster nodes, you need to restart all of them (master, peers, and search head) for the changes to take effect. You can do this by invoking the CLI restart command on each node:

```bash
$SPLUNK_HOME/bin/splunk restart
```

Important: When the master starts up for the first time, it blocks indexing on the peers until you enable and restart the full replication factor number of peers. For example, given a three-site cluster with "site_replication_factor = origin:2, site1:1, site2:2, site3:3, total:8", the master blocks indexing until there are at least eight peers in total across all sites, including at least one in site1, two in site2, and three in site3.

Do not restart the master while it is waiting for the peers to join the cluster. If you do, you will need to restart the peers a second time.

##### After changing the configuration

On the master

You must restart the master after you change any of the following attributes:

* multisite
* available_sites
* site_replication_factor
* site_search_factor

After you restart the master, you must also initiate a rolling-restart of the cluster peers. If you don't, the cluster will be in an indeterminate state. For information on the splunk rolling-restart command, see "[Use rolling restart](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Userollingrestart)".

You do not need to restart if you change the site value on a master.

On the peers

If you change the site value on a peer, you must restart it for the change to take effect.

Important: Although you can use the CLI restart command when you initially enable an instance as a cluster peer node, you should not use it for subsequent restarts. The restart command is not compatible with index replication once replication has begun. For more information, including a discussion of safe restart methods, see "[Restart a single peer](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Restartthecluster#Restart_a_single_peer)".

On the search head

You do not need to restart if you change the site value on a search head.

### Configure multisite indexer clusters with the CLI

#### Read this first(CLI)

Before reading this topic, see:

* "[Multisite indexer cluster deployment overview](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisitedeploymentoverview)". This topic provides important background information about configuring a multisite cluster.
* "[Configure the indexer cluster with the CLI](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/UsetheCLI)". This topic explains the basics of using the CLI to configure a cluster. It focuses on single-site clusters, but most of its information is relevant to multisite clusters as well.
* "[Configure multisite indexer clusters with server.conf](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisiteconffile)". This topic provides useful information on configuring a multisite cluster, including details on the attributes corresponding to the command-line options described in the current topic.

#### Configure multisite cluster nodes(CLI)

You configure instances as multisite cluster nodes with the splunk edit cluster-config command. After enabling an instance, you must restart it.

##### Site values(CLI)

Site values identify the site on which a node resides. You assign a site value to each node in a multisite cluster.

Site values have the syntax:

site\<n\>

where \<n\> is an integer in the range of 1 to 63: site1, site2, site3, ....

Note: In the case of a search head only, you can also set the site value to "site0". This setting disables search affinity for the search head.

##### Configure the master node(CLI)

Here is an example of a multisite configuration for a master mode:

```bash
splunk edit cluster-config -mode master -multisite true -available_sites site1,site2 -site site1 -site_replication_factor origin:2,total:3 -site_search_factor origin:1,total:2

splunk restart
```

This example specifies that:

* the instance is a cluster master node.
* the cluster is multisite.
* the cluster consists of two sites: site1 and site2.
* the master is located on site1.
* the cluster's replication factor is the default "origin:2,total:3".
* the cluster's search factor is "origin:1,total:2".

Note the following:

* Each cluster has only one master.
* You must set multisite to true for multisite cluster masters.
* You must list all cluster sites with the available_sites attribute.
* You must set a site_replication_factor and a site_search_factor. For details, see "[Configure the site replication factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitereplicationfactor)" and "[Configure the site search factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitesearchfactor)".

You might also need to adjust the single-site replication and search factors. See "[How multisite configuration differs from single-site configuration](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisiteconffile#How_multisite_configuration_differs_from_single-site_configuration)."

Important: When the master starts up for the first time, it blocks indexing on the peers until you enable and restart the full replication factor number of peers. For example, given a three-site cluster with "site_replication_factor = origin:2, site1:1, site2:2, site3:3, total:8", the master blocks indexing until there are at least eight peers in total across all sites, including at least one in site1, two in site2, and three in site3.

Do not restart the master while it is waiting for the peers to join the cluster. If you do, you will need to restart the peers a second time.

Note: You do not need to restart the master if you later change its site value.

##### Configure the peer nodes(CLI)

To configure a peer node in a multisite cluster, you set a site attribute. All other configuration settings are identical to a peer in a single-site cluster.

Here is an example configuration for a multisite peer node:

```bash
splunk edit cluster-config -mode slave -site site1 -master_uri https://10.160.31.200:8089 -replication_port 9887

splunk restart
```

This example specifies that:

* the instance is a cluster peer ("slave") node.
* the instance is located in site1. A peer can belong to only a single site.
* the peer's cluster master is located at 10.160.31.200:8089.
* the peer will use port 9887 to listen for replicated data streamed from the other peers. You can specify any available, unused port as the replication port. Do not re-use the management or receiving ports.

Note: You do not need to restart the peer if you later change its site value.

##### Configure the search heads(CLI)

To configure a search head for a multisite cluster, set the site parameter. All other settings are the same as for a search head in a single-site cluster.

You use different commands to configure a search head initially and to change its configuration later.

To initially configure a search head:

Use the splunk edit cluster-config command. Here is an example configuration for a multisite search head:

```bash
splunk edit cluster-config -mode searchhead -site site1 -master_uri https://10.160.31.200:8089

splunk restart
```

This example specifies that:

* the instance is a cluster search head.
* the search head is located in site1. A search head can belong to only one site in each cluster.
* the search head's cluster master is located at 10.160.31.200:8089.

To disable search affinity for a search head, so that it gets its data randomly from all sites in the cluster, set the site attribute to "site0".

Note: When you specify the site parameter, the command automatically sets multisite=true in the search head's server.conf file. You do not need to explicitly pass a multisite parameter.

To edit the search head configuration later:

Use the splunk edit cluster-master command, not the splunk edit cluster-config command.

For example, assume that you initially configured a single-site search head using the splunk edit cluster-config command:

```bash
splunk edit cluster-config -mode searchhead -master_uri https://10.160.31.200:8089

splunk restart
```

To later reconfigure the search head for a multisite cluster, use the splunk edit cluster-master command:

splunk edit cluster-master <https://10.160.31.200:8089>  -site site1

Important: The splunk edit cluster-master command always takes the current master URI:port value as its initial parameter. For more examples, see "[Configure the indexer cluster search head with the CLI](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/ConfiguresearchheadwithCLI)".

For information on configuring a multisite search head for multi-cluster search, see "[Configure multi-cluster search for multisite clusters](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Configuremulti-clustersearch#Configure_multi-cluster_search_for_multisite_clusters)".

Note: You do not need to restart the search head if you later change its site value.

### Configure the site replication factor

#### Read this first(replication)

Before attempting to configure the site replication factor, you must be familiar with:

* The basic, single-site replication factor. See "[The basics of indexer cluster architecture](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Basicclusterarchitecture)" and "[Replication factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Thereplicationfactor)".
* Multisite cluster configurations. See "[Configure multisite indexer clusters with server.conf](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisiteconffile)".

#### What is a site replication factor

To implement multisite indexer clustering, you must configure a site replication factor. This replaces the standard replication factor, which is specific to single-site deployments. You specify the site replication factor on the master node, as part of the basic configuration of the cluster.

The site replication factor provides site-level control over the location of bucket copies, in addition to providing control over the total number of copies across the entire cluster. For example, you can specify that a two-site cluster maintain a total of three copies of all buckets, with one site maintaining two copies and the second site maintaining one copy.

You can also specify a replication policy based on which site originates the bucket. That is, you can configure the replication factor so that a site receiving external data maintains a greater number of copies of buckets for that data than for data that it does not originate. For example, you can specify that each site maintains two copies of all data that it originates but only one copy of data originating on another site.

#### Syntax

You configure the site replication factor with the site_replication_factor attribute in the master's server.conf file. The attribute resides in the [clustering] stanza, in place of the single-site replication_factor attribute. For example:

```properties
[clustering]
mode = master
multisite=true
available_sites=site1,site2
site_replication_factor = origin:2,total:3
site_search_factor = origin:1,total:2
```

You can also use the CLI to configure the site replication factor. See "[Configure multisite indexer clusters with the CLI](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/MultisiteCLI)".

Warning: You must configure the site_replication_factor attribute correctly. Otherwise, the master will not start.

Here is the formal syntax:

```properties
site_replication_factor = origin:<n>, [site1:<n>,] [site2:<n>,] ..., total:<n>
```

where:

* \<n\> is a positive integer indicating the number of copies of a bucket.
* origin:\<n\> specifies the minimum number of copies of a bucket that will be held on the site originating the data in that bucket (that is, the site where the data first entered the cluster). When a site is originating the data, it is known as the "origin" site.
* site1:\<n\>, site2:\<n\>, ..., indicates the minimum number of copies that will be held at each specified site. The identifiers "site1", "site2", and so on, are the same as the site attribute values specified on the peer nodes.
* total:\<n\> specifies the total number of copies of each bucket, across all sites in the cluster.

Note the following:

* This attribute specifies the per-site replication policy. It is specified globally and applies to all buckets in all indexes.
* This attribute is valid only if mode=master and multisite=true. Under those conditions, it supersedes any replication_factor attribute.
* The origin and total values are required.
* Site values (site1:\<n\>, site2:\<n\>, ...) are optional. A site that is specified here is known as an "explicit" site. A site that is not specified is known as a "non-explicit" site.
* Here is how the cluster determines the minimum number of copies a site gets:
* When a site is functioning as origin, the minimum number of copies the site gets is the greater of either its site value, if any, or origin.
* When a site is not functioning as origin, the site value, if any, determines the minimum number of copies the site gets.
* A non-explicit site is not guaranteed any copies except when it is functioning as the origin site.

For example, in a four-site cluster with "site_replication_factor = origin:2, site1:1, site2:2, site3:3, total:8", site1 gets two copies of any data that it originates and one copy of data that any other site originates. Site2 gets two copies of data, whether or not it originates it. Site3 gets three copies of data, whether or not it originates it. The non-explicit site4 gets two copies of data that it originates, two copies of data that site2 or site3 originates, and one copy of data that site1 originates. (Site4 gets the number of copies necessary to ensure that the number of copies of each bucket meets the total value of 8.) Here is how you calculate site4's number of copies, according to where the data originates:

originate at site1 -> 2 copies in site1, 2 copies in site2, 3 copies in site3, 1 copy in site4 => total=8
originate at site2 -> 1 copy in site1, 2 copies in site2, 3 copies in site3, 2 copies in site4 => total=8
originate at site3 -> 1 copy in site1, 2 copies in site2, 3 copies in site3, 2 copies in site4 => total=8
originate at site4 -> 1 copy in site1, 2 copies in site2, 3 copies in site3, 2 copies in site4 => total=8

* When specifying the site_replication_factor, here is how you determine the minimum required value for total, based on the site and origin values:
* If there are some non-explicit sites, then the total value must be at least the sum of all explicit site and origin values.

For example, with a three-site cluster and "site_replication_factor = origin:2, site1:1, site2:2", the total must be at least 5: 2+1+2=5. For another three-site cluster with "site_replication_factor = origin:2, site1:1, site3:3", the total must be at least 6: 2+1+3=6.

* If all sites are explicit, then the total must be at least the minimum value necessary to fulfill the dictates of the site and origin values.

For example, with a three-site cluster and "site_replication_factor = origin:1, site1:1, site2:1, site3:1", the total must be at least 3, because that configuration never requires more than three copies. For a three-site cluster and "site_replication_factor = origin:2, site1:1, site2:1, site3:1", the total must be at least 4, because one of the sites will always be the origin and thus require two copies, while the other sites will each require only one. For a three-site cluster and "site_replication_factor = origin:3, site1:1, site2:2", site3:3", the total must be at least 8, to cover the case where site1 is the origin.

The easiest way to determine this is to substitute the origin value for the smallest site value and then sum the site values (including the substituted origin value). So, in the last example ("site_replication_factor = origin:3, site1:1, site2:2", site3:3"), site1 has the smallest value, which is 1. You substitute the origin's 3 for that 1 and then add the site2 and site3 values: 3+2+3=8.

• Because the total value can be greater than the total set of explicit values, the cluster needs a strategy to handle any "remainder" bucket copies. Here is the strategy:
• If copies remain to be assigned after all site and origin values have been satisfied, those remainder copies are distributed across all sites, with preference given to sites with less or no copies, so that the distribution is as even as possible. Assuming that there are enough remainder copies available, each site will have at least one copy of the bucket.

For example, given a four-site cluster with "site_replication_factor = origin:1, site1:1, site4:2, total:4", if site1 is the origin, there will be one remainder copy. This copy gets distributed randomly to either site2 or site3. However, if site2 is the origin, it gets one copy, leaving no remainder copies to distribute to site3.

In another example, given a four-site cluster where "site_replication_factor = origin:2, site1:2, site4:2, total:7", if site1 is the origin, there are three remainder copies to distribute. Because site2 and site3 have no explicitly assigned copies, the three copies are distributed between them, with each site getting at least one copy. If site2 is the origin, however, it gets two copies and site3 gets the one remainder copy.

This entire process depends on the availability of a sufficient number of peers on each site. If a site does not have enough peers available to accept additional copies, the copies go to sites with available peers. In any case, at least one copy will be distributed or reserved for each site, assuming enough copies are available.

Here are a few more examples:

• A three-site cluster with "origin:1, total:3": The distribution guarantees one copy per site.
• A three-site cluster with "origin:1, total:4": The distribution guarantees one copy per site, with one additional copy going to a site that has at least two peers.
• A three-site cluster with "origin:1, total:9", where site1 has only one peer and site2 and site3 have 10 peers each: The distribution guarantees one copy per site, with the six remaining copies distributed evenly between site2 and site3.
• If all peers on one non-explicit site are down and there are still remainder copies after all other non-explicit sites have received a copy, the cluster will reserve one of the remainder copies for that site, pending the return of its peers. During that time, the site_replication_factor cannot be met, because the total number of copies distributed will be one less than the specified total value, due to the copy that is being held in reserve for the site with the downed peers.

For example, given a four-site cluster with "site_replication_factor = origin:1, site1:1, site4:2, total:5", if site1 is the origin, there will be two remainder copies to distribute between site2 and site3. If all the peers on site2 are down, one remainder copy goes to site3 and the other copy will be held in reserve until one of the site2 peers rejoins the cluster. During that time, the site_replication_factor is not met. However, given a four-site cluster with "site_replication_factor = origin:1, site1:1, site4:2, total:4" (the only difference from the last example being that the total value is 4 instead of 5), if site1 is the origin, there will be only one remainder copy, which will go to either site2 or site3. If all the peers on site2 are down, the copy will go to site3 and no copy will be held in reserve for site2. The site_replication_factor is met in this example, because no copies are being held in reserve for site2.

• Each site must deploy a set of peers at least as large as the greater of the origin value or its site value.

For example, given a three-site cluster with "site_replication_factor = origin:2, site1:1, site2:2, site3:3, total:8", the sites must have at least the following number of peers: site1: 2 peers; site2: 2 peers; site3: 3 peers.

• The total number of peers deployed across all sites must be greater than or equal to the total value.

• The total value must be at least as large as the replication_factor attribute, which has a default value of 3. Therefore, if the total value is 2, you should explicitly set the value of replication_factor to 2.

• If you are migrating from a single-site cluster, the total value must be at least as large as the replication_factor for the single-site cluster. See "[Migrate an indexer cluster from single-site to multisite](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Migratetomultisite)".

• The attribute defaults to: "origin:2, total:3."

#### Examples

• **A cluster of two sites (site1, site2), with the default "site_replication_factor = origin:2, total:3"**: For any given bucket, the site originating the data stores two copies. The remaining site stores one copy.

• **A cluster of three sites (site1, site2, site3), with the default "site_replication_factor = origin:2, total:3"** : For any given bucket, the site originating the data stores two copies. One of the two non-originating sites, selected at random, stores one copy, and the other doesn't store any copies.

• **A cluster of three sites (site1, site2, site3), with "site_replication_factor = origin:1, site1:1, site2:1, site3:2, total:5"** : For all buckets, site1 and site2 store a minimum of one copy each and site3 stores two copies. The fifth copy gets distributed to either site1 or site2, because those sites have fewer assigned copies than site3.

• **A cluster of three sites (site1, site2, site3), with "site_replication_factor = origin:2, site1:1, site2:1, total:4"** : Site1 stores two copies of any bucket it is originating and one copy of any other bucket. Site2 follows the same pattern. Site3, whose site value is not explicitly defined, follows the same pattern.

• **A cluster of three sites (site1, site2, site3), with "site_replication_factor = origin:2, site1:1, site2:2, total:5"** : Site1 stores two copies of any bucket it originates, one or two copies of any bucket site2 originates, and one copy of any bucket that site3 originates. Site2 stores two copies of any bucket, whether or not it originates it. Site3, whose site value is not explicitly defined, stores two copies of any bucket it originates, one copy of any bucket site1 originates, and one or two copies of any bucket site2 originates. (When site2 originates a bucket, one copy remains after initial assignments. The master assigns this randomly to site1 or site3.)

• **A cluster of three sites with "site_replication_factor = origin:1, total:4"** : Four copies of each bucket are distributed randomly across all sites, with each site getting at least one copy.

#### Handle the persistence of the single-site replication_factor

Important: Although the site_replication_factor effectively replaces the single-site replication_factor, the single-site attribute continues to exist in the master's configuration, with its default value of 3. This can cause problems on start-up if any site has fewer than three peer nodes. The symptom will be a message that the multisite cluster does not meet its replication factor. For example, if one of your sites has only two peers, the default single-site replication factor of 3 will cause the error to occur. To avoid or fix this problem, you must set the single-site replication factor to a value that is less than or equal to the smallest number of peers on any site. In the case where one site has only two peers, you must therefore explicitly set the replication_factor attribute to a value of 2. See "Multisite cluster does not meet its replication or search factors."

### Configure the site search factor

#### Read this first(search factor)

Before attempting to configure the site search factor, you must be familiar with:

• The basic, single-site search factor. See "[The basics of cluster architecture](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Basicclusterarchitecture)" and "[Search factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Thesearchfactor)".
• The site replication factor. See "[Configure the site replication factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitereplicationfactor)".
• Multisite cluster configurations. See "[Configure multisite indexer clusters with server.conf](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisiteconffile)".

#### What is a site search factor

To implement multisite indexer clustering, you must configure a site search factor. This replaces the standard search factor, which is specific to single-site deployments. You specify the site search factor on the master node, as part of the basic configuration of the cluster.

The site search factor provides site-level control over the location of searchable bucket copies, in addition to providing control over the total number of searchable copies across the entire cluster. For example, you can specify that a two-site cluster maintain a total of three searchable copies of all buckets, with one site maintaining two copies and the second site maintaining one copy.

You can also specify a search policy based on which site originates the bucket. That is, you can configure the search factor so that a site receiving external data maintains a greater number of searchable copies of buckets for that data than for data that it does not originate. For example, you can specify that each site maintains two searchable copies of all data that it originates but only one copy of data originating on another site.

The site search factor helps determine whether the cluster has search affinity. See "[Implement search affinity in a multisite indexer cluster](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisitesearchaffinity)".

#### Syntax(search factor)

The syntax for site_search_factor and site_replication_factor are identical, except for the additional requirement that the values and explicit sites in site_search_factor be a subset of those in site_replication_factor. This section describes the syntax in full detail.

You configure the site search factor with the site_search_factor attribute in the master's server.conf file. The attribute resides in the [clustering] stanza, in place of the single-site search_factor attribute. For example:

```properties
[clustering]
mode = master
multisite=true
available_sites=site1,site2
site_replication_factor = origin:2,total:3
site_search_factor = origin:1,total:2
```

You can also use the CLI to configure the site search factor. See "[Configure multisite indexer clusters with the CLI](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/MultisiteCLI)".

Warning: You must configure the site_search_factor attribute correctly. Otherwise, the master will not start.

Here is the formal syntax:

```properties
site_search_factor = origin:<n>, [site1:<n>,] [site2:<n>,] ..., total:<n>
```

where:

• \<n\> is a positive integer indicating the number of searchable copies of a bucket.
• origin:\<n\> specifies the minimum number of searchable copies of a bucket that will be held on the site originating the data in that bucket (that is, the site where the data first entered the cluster). When a site is originating the data, it is known as the "origin" site.
• site1:\<n\>, site2:\<n\>, ..., indicates the minimum number of searchable copies that will be held at each specified site. The identifiers "site1", "site2", and so on, are the same as the site attribute values specified on the peer nodes.
• total:\<n\> specifies the total number of searchable copies of each bucket, across all sites in the cluster.

Note the following:

• This attribute specifies the per-site searchable copy policy. It is specified globally and applies to all buckets in all indexes.
• This attribute is valid only if mode=master and multisite=true. Under those conditions, it supersedes any search_factor attribute.
• The origin and total values are required.
• Site values (site1:\<n\>, site2:\<n\>, ...) are optional. A site that is specified here is known as an "explicit" site. A site that is not specified is known as a "non-explicit" site.
• To determine the minimum number of searchable copies a site gets, use the same rules as for determining the minimum number of replicated copies a site gets through site_replication_factor. See "[Configure the site replication factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitereplicationfactor)".
• To determine the minimum required value for total, use the same rules as for determining the minimum total value for the site_replication_factor. See "[Configure the site replication factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitereplicationfactor)".
• Because the total value can be greater than the total set of explicit values, the cluster needs a strategy to handle any "remainder" searchable bucket copies. The strategy follows the strategy for remainder replicated copies, described in "[Configure the site replication factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitereplicationfactor)".
• All values must be less than or equal to their corresponding values in the site_replication_factor.

For example, if you have a three-site cluster with "site_replication_factor = origin:2, site1:1, site2:2, total:5", then, in site_search_factor, the origin value cannot exceed 2, the site1 value cannot exceed 1, the site2 value cannot exceed 2, and the total value cannot exceed 5.

• If a site value is explicit in site_search_factor, it must also be explicit in site_replication_factor. However, an explicit site value in site_replication_factor does not need be explicit in site_search_factor.

For example, if you have a three-site cluster with "site_replication_factor = origin:2, site1:1, site2:2, total:5" (with a non-explicit site3), you can specify "site_search_factor = origin:1, site2:2, total:4" (removing the explicit site1), but you cannot specify "site_search_factor = origin:1, site1:1, site2:2, site3:1, total:4" (making the non-explicit site3 explicit).

• For search affinity, you must configure the site_search_factor so that you have at least one searchable copy on each site where you require search affinity. Only explicit sites adhere to search affinity.

• If you are migrating from a single-site cluster, the total value must be at least as large as the search_factor for the single-site cluster. See "[Migrate an indexer cluster from single-site to multisite](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Migratetomultisite)".

• The attribute defaults to: "origin:1, total:2."

#### Examples(search factor)

For examples of site search factor syntax, refer to the examples in "[Configure the site replication factor](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Sitereplicationfactor#Examples)". The syntax for specifying origin/site/total values in site_search_factor is identical to site_replication_factor.

#### Handle the persistence of the single-site search_factor

Important: Although the site_search_factor effectively replaces the single-site search_factor, the single-site attribute continues to exist in the master's configuration, with its default value of 2. This can cause problems on start-up if any site has only one peer node. The symptom will be a message that the multisite cluster does not meet its search factor. To avoid or fix this problem, you must set the single-site search factor to a value that is less than or equal to the smallest number of peers on any site. In the case where one site has only one peer, you must therefore explicitly set the search_factor attribute to a value of 1. See "[Multisite cluster does not meet its replication or search factors](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Bucketreplicationissues#Multisite_cluster_does_not_meet_its_replication_or_search_factors)."

### Migrate an indexer cluster from single-site to multisite

You can migrate an indexer cluster from single-site to multisite. After the migration, the cluster holds both single-site and multi-site buckets. It maintains them separately, following these rules:

• Single-site buckets (those existing at the time of migration) continue to respect their single-site replication and search factors. You cannot convert them to multisite.
• Multisite buckets (those created after migration) follow the multisite replication and search factor policies.

#### Perform the multisite migration

Important: The migration process does not alter the version of Splunk Enterprise that the instances are running on. To migrate to a multisite cluster, the instances must be running version 6.1 or higher. Therefore, before migrating to multisite, you might need to upgrade your single-site cluster. Follow the appropriate procedure in [Upgrade an indexer cluster](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Upgradeacluster).

To migrate a single-site cluster to multisite, configure each node for multisite:

1. Configure the master node for multisite and restart it, following the instructions in [Configure multisite indexer clusters with the CLI](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/MultisiteCLI). For example:

    ```bash
    splunk edit cluster-config -mode master -multisite true -available_sites site1,site2 -site site1 -site_replication_factor origin:2,total:3 -site_search_factor origin:1,total:2

    splunk restart
    ```

    Note the following:

    • Do not remove the existing single-site attributes for replication factor and search factor, replication_factor and search_factor. The master needs them to handle the migrated buckets.
    • The total values for site_replication_factor and site_search_factor must be at least as large as replication_factor and search_factor, respectively.
    • If the number of peers on any site is less than the single-site replication_factor or search_factor, you must reduce the values of those attributes to match the least number of peers on any site. For example, if replication_factor is 3 and search_factor is 2, and one of the sites has only 2 peers, you must change replication_factor to 2. Otherwise, the migrated buckets might not meet the replication and search factors, due to the way the cluster replicates migrated buckets. See [Multisite cluster does not meet its replication or search factors](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Bucketreplicationissues#Multisite_cluster_does_not_meet_its_replication_or_search_factors).

2. Set maintenance mode on the master:

    ```bash
    splunk enable maintenance-mode
    ```

    This step prevents unnecessary bucket fix-ups. See [Use maintenance mode](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Usemaintenancemode).

    To confirm that the master has entered maintenance mode, run splunk show maintenance-mode.

3. Configure the existing peer nodes for multisite. For each peer, specify its master node and site. For example:

    ```bash
    splunk edit cluster-config -site site1
    ```

    You will be prompted to restart the peer.

    Do this for each peer, specifying the site for that peer.

4. If you want to add new peers to the cluster, follow the instructions in [Configure multisite indexer clusters with the CLI](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/MultisiteCLI). For example:

    ```bash
    splunk edit cluster-config -mode slave -site site1 -master_uri https://10.160.31.200:8089 -replication_port 9887

    splunk restart
    ```

    Do this for each new peer that you want to add to the cluster.

5. Configure the search heads for multisite. For each search head, specify its master node and site. For example:

    ```bash
    splunk edit cluster-master https://10.160.31.200:8089 -site site1
    ```

    Do this for each search head, specifying the site for that search head.

    Note: The configuration is essentially the same if the search heads are members of a search head cluster. See Integrate with a multisite indexer cluster in Distributed Search.

6. If you want to add new search heads to the cluster, follow the instructions in Configure multisite indexer clusters with the CLI. For example:

    ```bash
    splunk edit cluster-config -mode searchhead -site site1 -master_uri https://10.160.31.200:8089

    splunk restart
    ```

    Do this for each new search head that you want to add to the cluster.

7. Disable maintenance mode on the master:

    ```bash
    splunk disable maintenance-mode
    ```

    To confirm that the master has left maintenance mode, run splunk show maintenance-mode.

    You can view the master dashboard to verify that all cluster nodes are up and running.

    During the migration, the cluster tags each single-site bucket with a site value.

    Note: You can also configure a multisite cluster by directly editing server.conf. See [Configure multisite indexer clusters with server.conf](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Multisiteconffile)

8. If you are using indexer discovery to connect forwarders to the peer nodes, you must assign a site to each forwarder. See [Use indexer discovery in a multisite cluster](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/indexerdiscovery#Use_indexer_discovery_in_a_multisite_cluster).

#### How the cluster migrates and maintains single-site buckets

Buckets in multisite clusters include a property that identifies the origin site. Buckets in single-site clusters do not include that property. So, when a cluster migrates from single-site to multisite, it must tag each single-site bucket with an origin site value. Since the bucket name includes the GUID of the originating peer, the cluster always knows the originating peer. With that information, it infers an origin site for the bucket:

• If the originating peer still exists in the cluster, the cluster assumes that the bucket originated on the site that the originating peer has been assigned to. It sets the bucket's origin to that site.
• If the originating peer is no longer in the cluster, the cluster assumes that the site with the most copies of the bucket is the origin site. It sets the bucket's origin to that site.

Here is how the cluster uses the inferred origin site to maintain the single-site bucket going forward, to handle any necessary fix-up so that the bucket continues to meet the single-site replication and search factors:

• If the cluster needs to replicate additional copies of the bucket to fulfill the replication factor, it only replicates within the bucket's inferred origin site.
• If the cluster needs to make a non-searchable copy of the bucket searchable to fulfill the search factor, it might do so on a non-origin site, if a non-searchable copy of that bucket already exists on some other site.

The cluster will never create a new copy of the bucket on a non-origin site.

#### Handle problems with the new site

If you configure the master node for multisite clustering, but the new site is not yet fully operational, the master blocks indexing while it waits for enough peers to become available to fulfill the multisite replication factor. To unblock indexing, you can run the splunk set indexing-ready command on the master. See [Restart indexing in multisite cluster after master restart or site failure](http://docs.splunk.com/Documentation/Splunk/7.1.0/Indexer/Restartindexing).