# Support

LDAP 

Case #1568030
Simple Bind
SASL Bind(보안 강력 NTLM Kerberos,RAM-MD5)

authentication.conf(SSLEnabled=1)
$SPLUNK_HOME/etc/openldap

DistributedPeerManager

/opt/splunk/var/run
/opt/splunk/var/run/searchpeers

server.conf(max_content_length)
1GB미만

Role생성시에 index가 안보이는 경우(100이상)
$SPLUNK_HOME/share/splunk/search_mrsparkle/exposed/build/pages/enterprise/authorization_roles.js
findthius.indexListSearchJob.getResult().subscribe(function(results))
{count:1000}

_bump
cachehttps://<>/en-US/_bump

Splunk 8.0 Upgrade 방안

Duplicated alert in the Search Head Cluster
HEC token "Server is busy"

max_number_of_acked_requests_pending_query_per_ack_channel
고객까지 전달 되지 않음
inputs.conf(queueSize=200MB) 증가시킴
useACK=1

http_event_collector_metrics.log
num_of_ack_requests:0

limits.conf
max_number_of_acked_requests_pending_query_per_ack_channel (int 1000000)

Field Alias
EVAL-user 이용

[search]
search_keepalive_frequeny = 100(default) --> 5

parsing OR aggregation queue ratio 100%
parsing queue : LINE_BREAKER
aggregation queue : TIME_FORMAT, SHOULD_LINEMERGE = false

limits.conf
agg_cpu_profiling = true

KVStore failed

[kvstore]
enabled = true

server.conf
conf_deploy_fetch




