# Splunk Cluster Administration 7.0

## Module 8: KV Store Management in Splunk Clusters

### Module Objectives

- Enable a KV store collection under a search head cluster
- Manage KV store collections in a search head cluster
- Monitor and troubleshoot KV store issues in Splunk clusters
 
### Overview of Splunk Lookups

- A lookup is a Splunk data enrichment knowledge object
  - Used ONLY during search time
  - The lookup stanzas are defined in transforms.conf and props.conf
- File-based lookup is used for data sets that are small and/or change infrequently
  - Uses CSV files stored in the lookups directory
- KV store lookup is designed for large key-value collections that frequently change
  - Tracking workflow state changes (an incident-review system)
  - Keeping a list of environment assets assigned to users and their metadata
  - Controlling a job queue or application state as the user interacts with the app
  - Requires collections.conf to define fields
    - Can optionally specify data type enforcement and field accelerations

### CSV Lookup Challenges in Distributed Environment

- In a distributed environment, each SH1 SH2 SH3 search head manages and replicates its own CSV lookup files
  - Updates to lookup files propagate to app/lookups/csv app/lookups/csv app/lookups/csv search peers independently
- Searches can fail if a knowledge bundle is not replicated to search peers in time
  - By default, a bundle larger than 2GB is not replicated to the search peers
- In Splunk clusters, the timely replication of lookup files is more critical
  - Why? ☞ Slides 163 and 169 Indexer 1 Indexer 2 ... Indexer n-1 Indexer n
  - By default, SHC conf replication rejects a bundle size larger than 2GB 

### Benefits of KV Store Lookup

- Quick per-record updates
  - Performs Create-Read-Update-Delete (CRUD) operations on individual records using the Splunk REST API and SPL commands
- Standardized interface for app developers
  - REST API and data type validation
- Faster replication on SHC and search peers
  - Perform automatic lookups and distributed search-based lookups on the index tier
    - Replication must first be enabled per collection
  - Facilitate backup and restore of KV store data

### Enabling KV Store Collections

- Before users can use a KV store, an admin must create a collection
- A collection is defined in collections.conf
  - Must be placed in an app's default or local directory
    - Other attempts are ignored
    - Specify the name of the collection (stanza) and optional attributes
    - Matching lookup field, output fields, etc.
    - Enforcing data types is optional
    - If enforced, any input that does not match the type is silently dropped
    - We will discuss more options later

collections.conf Example: [collection_name]
[mykv] enforceTypes = [true|false]
enforceTypes = true field.<name1> = [number|string|bool|time]
field.x = number field.<name2> = [number|string|bool|time]
field.y = string accelerated_fields.<xl-name> = <json>
accelerated_fields.xl2 = {"x": 1, "y": 1}

### Populating KV Store Lookups

- Create a KV store collection stanza in collections.conf
  - Identify and include the options to enable
- Add lookup definition for KV store
  - 
Note
Working Click Settings > Lookups > Lookup definitions
with KV store is discussed in detail in Building ê The resulting configuration is saved in transforms.conf
Splunk Apps course.
- Write data to the KV store
  - 
Search: ... | fields id, location, type | outputlookup <lookup_name>
  - 
Or, use REST APIs
curl -k -u <user>:<pw> https://<url:mport>/servicesNS/<user>/<app>/storage/collections/data/<lookup_name> -H 'Content-Type: application/json' -d '{"id": 001, "location": "CA", "type": "basic"}'
 
### How KV Store Works in SH Cluster

SH1 SH2 SH3
- In cluster
a SHC, the KV store forms its own
  - 
RAFT Consensus
Member Captain Member
KV Store Consensus
KV SHC store members port must (8191 be by accessible default)
from all
  - 
Can use up to 50 nodes   - 
If you you must want disable to disable it on KV all store members
in a SHC,
SHC Replication .
Secondary Secondary
KV Store Replication
Primary - SHC synchronize there or restart)
captain is a status their and change member KV store (add, list primary
every remove,
time
  - 
Search Head Cluster READY
[kvstore] disabled = true
server.conf
T
S
Y
N
C
e im
By [shclustering] connection default, the and value replication
is used of mgmt_uri for KV store
in

 
### KV Store Collection Replication   - Write

SH1 SH2 SH3
- Writing majority to journaling
a collection uses write
WRITE
Member Captain
Secondary ✔
replicate w{x=1}
Primary
  - Similar replication, are majority are operation their acknowledged done respective to of writing SHC has voting except been configuration
journals and KV write when logged store the
requests
the
nodes
to
Secondary
- Collection data the journal
consistency checkpoints and get provide
used in
- Journal information
provides recovery
Collection READY
w{x=1}
w{x=1}
j{ok}
✘ j{ok}
Primary
Captain success

### KV Store Collection Replication   - Read

SH1 SH2 SH3
- Read collection request is routed to the nearest member
READ
READ
  - 
Read from the lowest-latency
Member Captain
member
Secondary ✔ Secondary
✘
Primary ✔ {x=1} {x=1}

### KV Store CLI Commands

- The similar conditions that cause SHC members to end up in an inconsistent replication state can cause the KV store collections to be out of sync
  - Frequent status changes to the SHC member instances   - Changing the GUID or hostname of a member   - Depending on the condition of SHC, KV store cluster can also be in the
state where it is impossible to sync   - One workaround is to bootstrap SHC from scratch
ê WARNING: Proceed ONLY when it is absolutely necessary
- Useful commands
  - splunk show kvstore-status   - splunk clean kvstore   - splunk resync kvstore -source <GUID>

 
splunk show kvstore-status
Example ran on SH2 splunk show shcluster-status splunk show kvstore-status Captain:
This member: ... dynamic_captain : 1
replicationStatus : Non-captain KV store member elected_captain : Wed Apr 12 00:14:18 2017
standalone : 0 id : E3D8A0AD-307B-4F95-BF70-60A7688F3109
status : ready initialized_flag : 1
label : sh2
Enabled KV store members: mgmt_uri : https://10.0.1.2:8289
10.0.1.2:8291 guid : A0FCB2B8-CC0C-4E98-A77A-83010779A649 min_peers_joined_flag : 1
hostAndPort : 10.0.1.2:8291 rolling_restart_flag : 0
... service_ready_flag : 1
KV store members: 10.0.1.2:8391 configVersion : 3 Members:
electionDate : Wed Apr 12 00:13:38 2017 sh3 label : sh3
electionDateSec : 1491956018 last_conf_replication : Thu Apr 13 17:42:00 2017
hostAndPort : 10.0.1.2:8391 mgmt_uri : https://10.0.1.2:8389
lastHeartbeat : Thu Apr 13 17:42:10 2017 mgmt_uri_alias : https://10.0.1.2:8389
lastHeartbeatRecv : Thu Apr 13 17:42:10 2017 status : Up
... optimeDate : Thu Apr 13 17:42:08 2017 sh2 label : sh2
optimeDateSec : 1492105328 mgmt_uri : https://10.0.1.2:8289
pingMs : 0 mgmt_uri_alias : https://10.0.1.2:8289
replicationStatus : KV store captain status : Up
uptime : 149278 ... sh4 label : sh4
10.0.1.2:8291 configVersion : 3 last_conf_replication : Thu Apr 13 17:42:00 2017
hostAndPort : 10.0.1.2:8291 mgmt_uri : https://10.0.1.2:8489
optimeDate : Thu Apr 13 17:42:08 2017 mgmt_uri_alias : https://10.0.1.2:8489
optimeDateSec : 1492105328 status : Generated Up
for Cho Jun Seung (jscho@time-gate.com) (C) Splunk replicationStatus Inc, not for distribution
: Non-captain KV store member
 
### Monitoring KV Store Status with MC

- MC provides more details on KV store status
  - In the MC General Setup page, add the KV store server role to the instance   - 
Go to MC > Search > KV Store: Deployment

 
### Backing Up and Restoring KV Store

- To back up KV store data in a SHC:
1. Confirm the KV store status with CLI or MC 2. Stop a SHC member that is in ready state and has the KV store data 3. Use your organization's backup tool to copy the kvstore folder and all
the collections.conf files ê The location of the kvstore folder is specified in server.conf under
[kvstore] dbPath ê Default: SPLUNK_HOME/var/lib/splunk/kvstore ê splunk btool collections list --debug | grep "\["
- To restore when the majority SHC members are stale:
1. Stop all SHC members 2. Restore collections.conf and the kvstore folder to each member 3. Start each SHC member

### Fixing Stale KV Store Members

- When KV store members fail on the write operations, they might become stale
  - Run splunk show kvstore-status to determine the stale members
- If majority members are stale, then the SHC requires a new bootstrap
1. Pick the best member to be the KV
store data authority and back up 2. On all SHC members,
a. Remove the KV store cluster info b. Clean KV store local data c. Reset SHC raft data 3. Restore the backed-up KV store
data and bootstrap a new SHC 4. Start members and re-join SHC 5. From the captain, sync KV store
cp   - r <sh>/<dbPath> <kv-backup>
splunk stop splunk clean kvstore   - cluster splunk clean kvstore -local splunk clean raft
cp   - r <kv-backup> <sh>/<dbPath> splunk start splunk bootstrap shcluster-captain -servers_list <sh>
splunk add shcluster-member -current_member_uri <captain>
splunk show shcluster-status splunk resync kvstore

### Fixing Stale KV Store Members (cont.)

- If only a few members are stale, then resync it from one of the members
  - Stale member:
splunk show kvstore-status splunk stop splunk clean kvstore -local splunk start
  - SHC captain:
splunk resync kvstore [-source <KVstore_source_GUID>]
Note
Specify optional -source, if you want to use a member other than the captain.

 
### SHC KV Store Bundle Replication to Indexers

- To enable an automatic lookup with KV store data, you must enable replication in collections.conf
  - Each automatic lookup configuration is limited to a specific host, source, or source type
collections.conf on all members
WRITE [mykv] enforceTypes = true field.x = number
Captain
field.y = string accelerated_fields.xl2 = {"x": 1, "y": 1} replicate = true replication_dump_strategy = one_file|auto
Secondary Primary
Secondary replication_dump_maximum_file_size = <KB>

1
2 5
3 4 4

### Notable KV Store Log Channels

- KV kvstore.log
store activities are logged in metrics.log, splunkd.log, and
- Metrics.log activities
has two sub groups under group=kvstore to track different
  - 
name=dump OR name=sync   - 
Example: Show the sync operation duration over time
index=_internal name
metrics group=kvstore | timechart max(msSyncTotal) by
- splunkd.log sync, and replication contains activities
various components for KV store status, start/stop,
  - 
To get the list of KV store components, search:
index=_internal | stats count by sourcetype=splunkd component
component=*kvstore*
- kvstore.log index
contains the introspection data feeding the _introspection

### Monitoring KV Store with Monitoring Console

- index=_introspection Monitoring Console
derives the historical KV store performance views in
- Start with the deployment-wide view and drill down to an instance level   - Median Page Faults per Operation shows the read activity involving disk I/O   - 
Excessive hit (1.3+) indicates a need for more RAM   - 
Replication nodes during latency writes
shows the operation lags between the primary and secondary
ê Long lags (30+) can indicate replication (write) issues ê You may need to increase the operation log size   - 
Together infer that there with High are heavy lock percentage write operations (50%+) or and the system Flushing is rate sluggish
(50~100%), you can
  - 
Operations last operations Log in Window the journal
of KV Store Captain shows the time between the first and
- Operations number of calls per Minute made to in the the KV KV store Store: operations Instance dashboard in detail
shows the

 
### KV Store Operations Log Size

- A larger operations log can give a KV store cluster a greater tolerance for lag and even make the set more resilient
- The KV store allocates the full log size the first time Splunk is started, regardless of its utilization
  - 
The operations log is 1 GB by default
  - 
To adjust, edit [kvstore] oplogSize in server.conf
ê Must edit all nodes and run splunk clean kvstore   - local
- So, what is consuming all that journal space?
  - 
A large | outputlookup operation can generate a lot of records
  - 
outputlookup append=true vs outputlookup append=false

### Further Reading: KV Store

- About KV Store
  - docs.splunk.com/Documentation/Splunk/latest/Admin/AboutKVstore
- Tutorial: Use KV Store with a simple app
  - dev.splunk.com/view/SP-CAAAEZT
- KV Store backup and restore
  - docs.splunk.com/Documentation/Splunk/latest/Admin/BackupKVstore

### Lab Exercise 8   - Add a KV Store Collection

- Time: 25 - 30 minutes
- Tasks:
  - Identify the current SHC captain and KV store captain
  - Verify the state of the KV store service from Monitoring Console
  - Enable the KV store collection replication

Course Wrap-up

What’s Next?
Power User Certification
Advanced Dashboards and Visualizations
Administrator Certification
Splunk
Splunk
Certified
Advanced Fundamentals
Fundamentals
Power User
Searching and 1
2
Online Test
Reporting
Analytics and Data Science
You are here
Splunk
Splunk Fundamentals
Fundamentals 1
2
Certified Power User Online Test
Splunk System
Splunk Data Administration
Administration
Certified Administrator Online Test
Splunk Cluster Administration
Architecting Architect Certification
Splunk Deployments
Architect Certification Lab
Splunk for App Developers
 Splunk
Splunk Fundamentals
Fundamentals 1
2
Splunk System
Splunk Data Administration
Administration
Required
Splunk
Splunk Fundamentals
Fundamentals 1
2
Certified Power User Online Test
Searching Advanced Reporting
and
Visualizations Dashboards Advanced
and
Developing
Required E-learning Building
with Splunk Splunk Apps
Java and
Exam Python SDKs
Recommended
For detailed course and certification information go to: http://splk.it/g8q If you have further questions, send an email to: certification@splunk.com
Troubleshooting Splunk
 
YouTube: The Splunk How-To Channel
- In addition to our roster of training courses, check out the Splunk Education How-To channel: http://www.youtube.com/c/SplunkHowTo
- This site provides useful, short videos on a variety of Splunk topics