# Administering Splunk Enterprise Security

## Module 4: ES Deployment

### Objectives

- Identify deployment topologies
- Examine the deployment checklist
- Understand pre-deployment requirements

### Deployment Checklist

1. Determine size and scope of installation
2. Configure additional servers if needed
3. Obtain the ES software
4. Determine software installation requirements for search heads, indexers and forwarders
5. Install all ES apps on search head(s) 6. Deploy indexer configurations

### ES Impact on Resources

- ES generally requires a new, dedicated search head or search head cluster
  - ES is only compatible with other CIM-compatible apps –ES adds a large number of searches and search results
- Hardware must meet or exceed Splunk minimum requirements: <http://docs.splunk.com/Documentation/Splunk/latest/Capacity/Referencehardware>
- ES increases some hardware requirements : <http://docs.splunk.com/Documentation/ES/latest/Install/DeploymentPlanning#Splunk_Enterprise_system_requirements>

### Supported Architectures

- Single server (proof of concept, testing, dev)
- Distributed search (single search head, multiple indexers)
- Search head clustering
  - Not search head pooling
- Indexer clustering
  - Including multi-site <http://docs.splunk.com/Documentation/ES/latest/Install/DeploymentPlanning>

### Adding ES to an Existing Site

Before ES
Note Pre-ES site with a single search head and 3 indexers supporting ~500GB/day of indexed data.
Log on here for Splunk search
Log on here for ES
Note After ES install, ES increases search requirements, adds an extra search head and 2 more indexers.
After ES

### Search Head Requirements

- A dedicated server or cluster for the ES search head(s) with only CIM-compliant apps installed
- 64 bit OS, minimum 32 GB and 16 processor cores
  - Additional memory and CPU capacity may be needed depending on number of concurrent users and searches, etc
- Configure search head forwarding : <http://docs.splunk.com/Documentation/Splunk/latest/DistSearch/Forwardsearchheaddata>
- If enabling Monitoring Console, do not use distributed mode: docs.splunk.com/Documentation/ES/latest/Install/DeploymentPlanning# Monitoring_Console

### Indexer Requirements

- Increased search load in ES requires typically more indexers
  - Assume at most 100GB/day per indexer
  - Hardware minimum: 16 CPU cores, 32 GB RAM
- The exact number of indexers required depends on:
  - Types and amounts of data being used by ES
  - Number of active correlation searches –Number of real-time correlation searches

### Indexer Cluster Requirements

- You can only enable ES on one search head or search head cluster for each indexer cluster
- On a multisite indexer cluster:
  - Enable summary replication to improve performance docs.splunk.com/Documentation/Splunk/latest/Indexer/ Clustersandsummaryreplication
  - Disable search affinity <http://docs.splunk.com/Documentation/Splunk/latest/Indexer/Multisitesearchaffinity>
- Make sure you use the indexer cluster master to deploy any ES add-ons to the indexer tier

### HPAS Storage Requirements

- In addition to index storage requirements, ES requires space for accelerated data (High Performance Analytics Store/HPAS)
- HPAS will require approximately 3.4 x (daily input volume) of additional space per year
- Example: input volume of 500 GB per day with one year retention –500 GB * 3.4 = 1700 GB additional space for accelerated data model storage
- This space will be added across all indexers
  - Example: if there are 5 indexers, 1700 GB / 5 = ~ 340GB per indexer additional space is required

### More About HPAS

- Most ES searches are executed on accelerated data models that are stored in HPAS
- The storage volumes allocated for HPAS should be tuned for best performance
- By default, HPAS storage is allocated in the same location as the index containing the raw events being accelerated
- Use the tstatsHomePath setting in indexes.conf if needed to specify alternate locations for your HPAS storage <http://docs.splunk.com/Documentation/Splunk/latest/Admin/Indexesconf#PER_INDEX_OPTIONS>

### Indexed Real Time Search

- ES automatically configures Splunk to use indexed real time searching
  - <http://docs.splunk.com/Documentation/Splunk/latest/Search/Aboutrealtimesearches#Indexed_real-time_search>
- This improves concurrent real time search performance at the cost of a small delay in delivering real time results from searches
- Leave this turned on in ES for best performance

### Forwarder Requirements

- In general, forwarders are unaffected by ES installation
- However, some add-ons that ES depends on must be deployed to forwarders to collect data
- Examples:
  - Windows add-on
  - *NIX add-on
  - Splunk Stream add-on

### App/Add-on Deployment Options

- Depending on your requirements, you may need to distribute add- ons to other Splunk instances like search heads, indexers, and heavy universal forwarders
- Use the appropriate app and add-on deployment methodology:
  - Forwarders and non-clustered Indexers: use Forwarder Management (Deployment Server) –Indexer clusters: Use the master node to deploy apps to peer nodes
  - Search head clusters: Use the deployer to deploy apps to cluster members docs.splunk.com/Documentation/ES/latest/Install/ InstallTechnologyAdd-ons

### Splunk App for ES Health Check

- splunkbase.splunk.com/app/3133/
- Useful tool before deploying ES
- Use to measure data input rates and resource levels
- Identify CPU, storage or IO issues
- Helps plan resource upgrades before installing ES

Add-on Builder

- splunkbase.splunk.com/app/2962/
- Builds add-ons for custom ES data
- Normalizes custom data into the Common Information Model
- Built-in validation
- Should not be used on production servers

### Important Resources

- Splunk Data Administration, Splunk System Administration, Splunk Cluster Administration, and Architecting and Deploying Splunk courses from Splunk Education
- Distributed Splunk overview: docs.splunk.com/Documentation/Splunk/latest/Deploy/Distributedo verview
- Capacity planning: docs.splunk.com/Documentation/Splunk/latest/Capacity/Accommo datemanysimultaneoussearches

### Lab Exercise 4: Plan a Deployment

Time: 20 minutes Scenario: You are working with a client that has Splunk installed on a distributed site. There is one search head and 4 indexers indexing 800 GB/day, with a retention period of 1 year. The customer is marginally happy with current performance. All servers are at basic minimum Splunk hardware levels. No new inputs are planned after installing ES.

What alterations to their configuration do you suggest before installing ES?