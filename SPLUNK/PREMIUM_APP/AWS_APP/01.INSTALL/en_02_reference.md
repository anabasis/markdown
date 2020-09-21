# Reference

## Troubleshoot the Splunk App for AWS

Isolating the component with the problem
The Splunk App for AWS relies on the Splunk Add-on for Amazon Web Services for input collection and knowledge management. When troubleshooting, determine whether the issue you are experiencing is relevant to the app or to the add-on. In general, if your AWS data is successfully reaching your Splunk indexes, the issue is with the app. If data is not reaching your Splunk indexes, then you should check for configuration problems with the accounts and inputs handled by the Splunk Add-on for Amazon Web Services. See Troubleshoot the Splunk Add-on for AWS for troubleshooting specific to the add-on.

Some dashboards temporarily fail to display data after upgrade
After you upgrade the Splunk App for AWS to version 5.1.0, data is temporarily unavailable for these dashboards that were previously well-displayed: Topology, VPC Flow Log, and CloudFront Access Log. This is because these dashboards are powered by some saved searches that are introduced in the new release and are not scheduled to run by default. This problem will automatically go away when Splunk runs the scheduled saved search App Upgrader that comes with the Splunk App for AWS 5.1.0. You can also manually run the App Upgrader saved search to resolve the dashboard display problem right away. Running App Upgrader updates the Detailed Billing data model and schedules the execution of the saved searches powering these dashboards.

Custom dashboards fail to display data properly after upgrade
After you upgrade the Splunk App for AWS to a newer release, custom dashboards you modified and saved to local in the previous version override the dashboards that come with the new version. The dashboards may use out-of-date macros and not display data correctly, and you may see this error message: The search specifies a macro “<macro_name>” that cannot be found. To resolve this issue, delete local copies of the affected dashboards.

Dashboards not showing data from custom indexes
If you configure inputs using custom indexes, macros that support dashboard performance must be updated to include the custom indexes. By default, the Splunk App for AWS runs a saved search called Addon Synchronization every hour that automatically updates the macros to include custom indexes you specified when configuring inputs.

You can also manually run the Addon Synchronization saved search to immediately update the macros.

See Saved searches for the Splunk App for AWS for more information.

Alternatively, you can update your local/macros.conf file to specify which indexes the app dashboards should search.

See Macros for the Splunk App for AWS for more information.

Topology dashboard shows no data
If your Topology dashboard shows no data, first verify that you are using an account that has access to AWS Config service.

If you use a clustered distributed Splunk deployment, you need to perform some additional steps:

Configure the search head tier to directly forward data to the indexer tier.
Distribute the summary index configuration bundle across clustered indexers.
For detailed instructions, see Install in a clustered distributed environment.

If you have previous AWS Config data before upgrade, you need to manually run saved search "Config: Topology History Generator", which will migrate previous AWS Config data before update to summary index.

Then, check that the required saved searches are enabled. The topology dashboard requires data from a set of saved searches that you can find in the app under Search > Reports. These searches runs every hour and help populate your Topology dashboard. If you configure your inputs through the app, the saved search is automatically enabled and scheduled. If, however, you configure your inputs through the add-on instead, you need to manually enable and schedule the saved searches.

See Saved searches for the Splunk App for AWS for more information.

Accessing logs
You can access internal log data for help with troubleshooting by searching by source type. See Troubleshoot the Splunk Add-on for AWS for information about accessing add-on logs.

Billing metric not available for CloudWatch
If you do not see the Billing namespace listed on the input configuration page for CloudWatch, check that you have turned on Receive Billing Alerts in the Preferences section of the AWS Billing and Cost Management console.

S3 input performance issues
You can configure multiple S3 inputs for a single S3 bucket to improve performance. The Splunk platform dedicates one process for each data input, so provided that your system has sufficient processing power, performance will improve with multiple inputs.

Note: Be sure that multiple inputs do not collect the same S3 folder and file data, to prevent indexing duplicate data.

Unexpected termination of S3 dashboard saved searches
Some saved searches powering S3 dashboards (Data Events and Traffic Analysis) terminate unexpectedly due to insufficient memory caused by too many concurrent searches. To resolve this issue, consider the following:

Increase RAM on the indexer (better performance)
If the indexer runs Linux, increase the swap size on the indexer (more cost-efficient)
Billing dashboards fail to load
When billing data has been indexed but the Billing dashboards fail to load, check your billing data models (Detailed Billing or Instance Hour). If your billing data model keeps showing the Building status for a long time but never reaches 100%, try removing all the billing tags under the Configure menu. If this does not resolve the problem, try removing the data model definition from the local folder. Do not select more than eight billing tags. Too many billing tags may cause performance issues.

## Share data in the Splunk App for AWS

The Splunk App for AWS includes the opt-in ability to send anonymized usage data to Splunk to help improve the app in future releases. You opt in/out by enabling/disabling Anonymized Usage Data under Settings > Instrumentation on the Splunk Web UI.

For more information about how Splunk collects and uses data, please refer to the Splunk Privacy Policy.

How data is collected
If you opt in, the app enables an internal library to track basic usage and crash information. The library uses browser cookies to track app user visitor uniqueness and sessions and sends events to Splunk using XHR in JSON format.

What data is collected
If enabled, the Splunk App for AWS sends five different kinds of events to Splunk.

Event	Source Type	Description	Data sent includes common fields, plus
Field	Type	Description
Session start	mint:ping	Each ping event indicates that a new session has started.	fsEncrypted	N/A	Not used, always "NA"
rooted	N/A	Not used, always false
Session end	mint:gnip	Each gnip event indicates that a session has ended.	ses_duration	int	How long the session lasted
Page views	mint:view	Triggered once per page view in the app.	current	string	The URL of the current web page, without the hostname.
currentView	string	Not used. Hardcoded to 'examples'.
domProcessingTime	int	Time spent to process the domain.
domLookupTime	int	Time spent to look up the domain name.
elapsedTime	int	Time spent to render the page.
host	string	The hostname in the URL.
loadTime	int	Time spent to load the page.
previous	string	The referrer URL.
serverTime	int	Time spent to get a response from the server.
App performance 
and configuration	mint:log	Usage and performance logs for the Splunk App for AWS that track dashboard memory usage, dashboard loading times, the number of accounts, inputs, and regions configured in the app, and non-sensitive input configuration parameters (for example, SQS queue names and S3 bucket names are not collected.)	level	int	Log level. For example, 60 means 'error'.
log_name	any	Log content. See examples below.
API calls	mint:network	XMLHTTPRequest calls, usually HTTP API calls from client side (browser) to the Splunk server.	failed	boolean	Indicates if the request failed or not.
latency	int	Time spent before response received.
protocol	string	Network protocol: either http or https.
requestLength	string	N/A. Not used.
responseLength	int	The size of the response.
statusCode	string	HTTP response code.
url	string	The request URL, without the hostname.
Common fields
The data that the Splunk App for AWS sends to Splunk, if enabled, includes the following common fields. This set of fields includes several fields that are disabled or deliberately not used for the Splunk App for AWS for purposes of anonymization.

Field	Type	Description	Example value
apiKey	string	MINT API key for the Splunk App for AWS	4t2fk73n
appRunningState	Field is unused by the SDK. Shows a value of "NA" in all events.
appVersionCode	Field is unused by the SDK. Shows a value of "NA" in all events.
appVersionName	string	The version name of the app sending data.	4.1.0
browser	string	The browser name.	Chrome
browserVersion	string	The browser version.	47.0.2526.111
carrier	Field is unused by the SDK. Shows a value of "NA" in all events.
connection	Field is unused by the SDK. Shows a value of "NA" in all events.
device	string	The device making the request.	MacIntel
extraData	JSON object	This field stores custom information for the app. This app uses extraData.splunk_version to store the version number of the Splunk platform instance.	6.3.1511
locale	string	The user locale set in the browser.	en-US
osVersion	string	The version code of the underlying operating system.	OS X 10.11.2
packageName	string	The package name of the Splunk App for AWS.	splunk_app_aws
platform	Not used for the Splunk App for AWS. Shows a value of "web" in all events.
remoteIP	Not used for the Splunk App for AWS. Shows a value of "3.0.0.0" in all events.
sdkVersion	string	The version of the SDK.	4.3
screenOrientation	Field is unused by the SDK. Shows a value of "NA" in all events.
session_id	string	A unique string to identify a session.	a5026251
state	string	Indicator of whether the browser is online or not. Can be either CONNECTED or DISCONNECTED.	CONNECTED
uuid	UUID	A random identifier to track the user's uniqueness	837227ea-4569-4675-9a17-ccb39ca69505
Example app performance and configuration events
The Splunk App for AWS sends performance and configuration information using the log_name field in the mint:log source type. This log_name field contains two sub-fields, name, which indicates which type of logs are being transmitted, and data, the content of the tracking log.

There are three possible options for name:

track_performance. When a user accesses a dashboard in the app, the Splunk App for AWS sends performance logs for dashboard memory usage and loading times.
track_configuration. When a Splunk admin visits the Configure page, the Splunk App for AWS sends a log of the number of accounts, inputs, and regions configured in the app, and non-sensitive input configuration parameters. (For example, SQS queue names and S3 bucket names are not collected.)
track_usage. When a Splunk admin visits the Configure page, the Splunk App for AWS sends a log of the data volume that each input is responsible for.
The following examples demonstrate what data the Splunk App for AWS sends for each type of event.

log_name.name	Example JSON object
track_performance	
{ 
 "memory":{ 
   "totalJSHeapSize":72200000,
   "usedJSHeapSize":39600000,
   "jsHeapSizeLimit":1620000000
 },
 "timing":{ 
   "navigationStart":1453273923766,
   "unloadEventStart":1453273923929,
   "unloadEventEnd":1453273923930,
   "redirectStart":0,
   "redirectEnd":0,
   "fetchStart":1453273923766,
   "domainLookupStart":1453273923766,
   "domainLookupEnd":1453273923766,
   "connectStart":1453273923766,
   "connectEnd":1453273923766,
   "secureConnectionStart":0,
   "requestStart":1453273923773,
   "responseStart":1453273923927,
   "responseEnd":1453273923929,
   "domLoading":1453273923939,
   "domInteractive":1453273923975,
   "domContentLoadedEventStart":1453273923975,
   "domContentLoadedEventEnd":1453273923975,
   "domComplete":1453273926985,
   "loadEventStart":1453273926985,
   "loadEventEnd":1453273926987
 }
}
track_configuration	
{  
   "addon":{  
      "isLocal":true,
      "version":"4.0.0"
   },
   "accounts":{  
      "count":3,
      "details":[  
         {  
            "name":"testaccount4",
            "category":"4"
         },
         {  
            "name":"testaccount1",
            "category":"1"
         },
         {  
            "name":"Peter",
            "category":"1"
         }
      ]
   },
   "inputs":{  
      "config":{  
         "count":1,
         "details":[  
            {  
               "account":"Peter",
               "regions":"ap-southeast-1",
               "index":"main",
               "interval":"30"
            }
         ]
      },
      "billing":{  
         "count":1,
         "details":[  
            {  
               "account":"Peter",
               "index":"main",
               "interval":"86400",
               "billing_daily_type":"2",
               "billing_montly_type":"2"
            }
         ]
      },
      "cloudwatch-logs":{  
         "count":2,
         "details":[  
            {  
               "account":"Peter",
               "regions":"ap-southeast-1",
               "index":"history",
               "interval":"600"
            },
            {  
               "account":"Peter",
               "regions":"ap-southeast-1,ap-southeast-2",
               "index":"history",
               "interval":"600"
            }
         ]
      },
      "cloudwatch":{  
         "count":2,
         "details":[  
            {  
               "account":"testaccount4",
               "regions":"cn-north-1",
               "index":"default",
               "interval":"3600",
               "metric_namespaces":"[\"AWS/Billing\", \"AWS/EBS\", \"AWS/EC2\", \"AWS/ELB\", \"AWS/S3\", \"AWS/SNS\", \"AWS/SQS\"]",
               "metric_details":"[{\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"ServiceName\": [\".*\"], \"Currency\": \".*\"}], \"metrics\": [\"EstimatedCharges\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"VolumeId\": [\".*\"]}], \"metrics\": [\"VolumeWriteOps\", \"VolumeTotalReadTime\", \"VolumeQueueLength\", \"VolumeTotalWriteTime\", \"VolumeWriteBytes\", \"VolumeIdleTime\", \"VolumeReadOps\", \"VolumeReadBytes\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"InstanceId\": [\".*\"]}], \"metrics\": [\"NetworkOut\", \"NetworkIn\", \"CPUCreditBalance\", \"StatusCheckFailed_Instance\", \"CPUCreditUsage\", \"StatusCheckFailed_System\", \"DiskReadOps\", \"DiskWriteBytes\", \"StatusCheckFailed\", \"CPUUtilization\", \"DiskReadBytes\", \"DiskWriteOps\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"LoadBalancerName\": [\".*\"]}], \"metrics\": [\"UnHealthyHostCount\", \"HealthyHostCount\", \"BackendConnectionErrors\", \"HTTPCode_ELB_5XX\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"BucketName\": [\".*\"], \"StorageType\": [\".*\"]}], \"metrics\": [\"NumberOfObjects\", \"BucketSizeBytes\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"TopicName\": [\".*\"]}], \"metrics\": [\"NumberOfNotificationsFailed\", \"NumberOfMessagesPublished\", \"PublishSize\", \"NumberOfNotificationsDelivered\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"QueueName\": [\".*\"]}], \"metrics\": [\"ApproximateNumberOfMessagesVisible\", \"NumberOfMessagesSent\", \"NumberOfMessagesDeleted\", \"ApproximateNumberOfMessagesNotVisible\", \"SentMessageSize\", \"ApproximateNumberOfMessagesDelayed\", \"NumberOfMessagesReceived\", \"NumberOfEmptyReceives\"]}]"
            },
            {  
               "account":"Peter",
               "regions":"eu-central-1,ap-northeast-1,eu-west-1,us-east-1,ap-southeast-1,ap-southeast-2,us-west-2,us-west-1,sa-east-1",
               "index":"default",
               "interval":"3600",
               "metric_namespaces":"[\"AWS/Billing\", \"AWS/EBS\", \"AWS/EC2\", \"AWS/ELB\", \"AWS/S3\", \"AWS/SNS\", \"AWS/SQS\"]",
               "metric_details":"[{\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"ServiceName\": [\".*\"], \"Currency\": \".*\"}], \"metrics\": [\"EstimatedCharges\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"VolumeId\": [\".*\"]}], \"metrics\": [\"VolumeIdleTime\", \"VolumeWriteBytes\", \"VolumeReadOps\", \"VolumeQueueLength\", \"VolumeReadBytes\", \"VolumeTotalWriteTime\", \"VolumeWriteOps\", \"VolumeTotalReadTime\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"InstanceId\": [\".*\"]}], \"metrics\": [\"DiskReadBytes\", \"NetworkOut\", \"StatusCheckFailed_Instance\", \"NetworkIn\", \"StatusCheckFailed\", \"StatusCheckFailed_System\", \"CPUUtilization\", \"CPUCreditBalance\", \"DiskWriteOps\", \"DiskWriteBytes\", \"DiskReadOps\", \"CPUCreditUsage\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"LoadBalancerName\": [\".*\"]}], \"metrics\": [\"UnHealthyHostCount\", \"HTTPCode_ELB_5XX\", \"HealthyHostCount\", \"BackendConnectionErrors\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"BucketName\": [\".*\"], \"StorageType\": [\".*\"]}], \"metrics\": [\"NumberOfObjects\", \"BucketSizeBytes\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"TopicName\": [\".*\"]}], \"metrics\": [\"NumberOfNotificationsFailed\", \"NumberOfMessagesPublished\", \"PublishSize\", \"NumberOfNotificationsDelivered\"]}, {\"statistics\": [\"Minimum\", \"Maximum\", \"Sum\", \"Average\"], \"dimensions\": [{\"QueueName\": [\".*\"]}], \"metrics\": [\"SentMessageSize\", \"ApproximateNumberOfMessagesNotVisible\", \"ApproximateNumberOfMessagesDelayed\", \"NumberOfMessagesDeleted\", \"NumberOfMessagesSent\", \"NumberOfMessagesReceived\", \"ApproximateNumberOfMessagesVisible\", \"NumberOfEmptyReceives\"]}]"
            }
         ]
      },
      "cloudtrail":{  
         "count":1,
         "details":[  
            {  
               "account":"Peter",
               "regions":"ap-southeast-1",
               "index":"main",
               "interval":"30"
            }
         ]
      },
      "description":{  
         "count":2,
         "details":[  
            {  
               "account":"testaccount4",
               "regions":"cn-north-1",
               "index":"default"
            },
            {  
               "account":"Peter",
               "regions":"eu-west-1,ap-southeast-1,ap-southeast-2,eu-central-1,ap-northeast-2,ap-northeast-1,us-east-1,sa-east-1,us-west-1,us-west-2",
               "index":"main"
            }
         ]
      },
      "s3":{  
         "count":1,
         "details":[  
            {  
               "account":"Peter",
               "index":"default",
               "interval":null
            }
         ]
      }
   }
}
track_usage	
{  
    "usage":[  
        {  
            "time":"2016-04-30",

            "volumes": {
                "aws:cloudtrail": 0.035876275,

                "aws:cloudwatch": 2918.7095499213,

                "aws:config": 0.288619041,

                "aws:s3": 0.288619041

            }
        }
    ]
}
What data is not collected
The following kinds of data are not collected:

Sensitive data such as usernames or passwords
Identifying information such as addresses, phone numbers, IP addresses, hostnames.
Indexed data that you ingest into your Splunk platform instance

No data is collected that is not explicitly described in the What data is collected section above.

## Saved searches for the Splunk App for AWS

The Splunk App for AWS includes the following saved searches.

To enable or disable a saved search:

From the Settings menu, choose Searches, reports, and alerts.
Locate the saved search by filtering the list or entering the name of the saved search in the filter field to search for it.
Under the Action column of the saved search, choose Edit > Enable/Disable to enable or disable it.
The "Addon Metadata - Summarize AWS Inputs" saved search is included in the Splunk Add-on for AWS and is disabled by default, but you MUST enable this saved search on the add-on side for the Splunk App for AWS to work properly. The saved search is used to aggregate inputs and accounts data in the "summary" index.

Name	Purpose	Action required
Amazon Inspector: Topology Amazon Inspector Recommendation Generator	Generates Amazon Inspector data for the Amazon Inspector & Config Rules layer on the Topology dashboard.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
App Upgrader	Migrates topology data, refreshes the detailed billing data model, and enables newly introduced saved searches to complete the app upgrade.	Automatically enabled. No action required.
Anomaly Detection (billing_d, cloudtrail_d, cloudtrail_h)	Used for anomaly detection.	Automatically enabled when you configure an anomaly detection rule. No action required. Scheduled to run once on a daily basis.
AWS Billing - Account Name	Populates an Account name lookup file, account_name.csv, so that the app dashboards can display friendly names for the account IDs in your billing reports.	This saved search runs automatically the first time that a user access one of the four dashboards that contain billing data. If you have a large amount of data, this search may take up to a minute to fully populate the lookup file with the friendly names that correspond to the account IDs in the reports. After the lookup generation is complete, the dashboard prompts you to reload the page to display your friendly account names. This search is not scheduled, so after it runs the first time the lookup is not updated again. If, in future months, your billing reports include additional accounts, you may want to rerun the saved search manually to capture the new friendly names for those accounts.
AWS: calculate data volume indexed	Calculates how much data volume the app and add-on have ingested daily.	Automatically enabled. No action required. Scheduled to run once daily at twenty minutes past midnight.
AWS Config - Tags	Extract user tags from config data.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run once daily at midnight. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
AWS Description - CloudFront Edges	Generates metadata of Cloudfront Edges.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. No action required. Scheduled to run once on a daily basis. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
AWS Description - Tags	Extract user tags from description data.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run once daily at midnight. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
Billing Alert: Account Total Cost	Billing alert template used for alerting user when the cost of a specific account reaches a threshold.	To use this alert, first modify the search to include your billing account ID, then enable this alert on the Alerts page in the app.
Billing Alert: Service Total Cost	Billing alert templates used for alerting user when the cost of a specific service reaches a threshold.	To use this alert, first modify the search to include a service name, then enable the alert on the Alerts page in the app.
Billing Alert: Subaccount Service Total Cost	Billing alert templates used for alerting user when the cost of a specific service for a subaccount reaches a threshold.	To use this alert, first modify the search to include your billing account ID and a service name, then enable this alert on the Alerts page in the app.
Billing Alert: Subaccount Total Cost	Billing alert templates used for alerting user when the cost of a specific subaccount reaches a threshold.	To use this alert, first modify the search to include your billing account ID, then enable this alert on the Alerts page in the app.
Billing: Detailed Reports List	Used to reduce the loading time of the "Select Billing Tags" window on the Configure dashboard.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run once daily at 10pm. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
Billing: Topology Billing Metric Generator	Generates billing data for Billing layer on the Topology dashboard.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
CloudTrail Base Search	Used for report acceleration.	Accelerated search. No action required.
CloudTrail Timechart Search	Used for report acceleration.	Accelerated search. No action required.
CloudTrail S3 Data Event Search	Used for report acceleration.	Accelerated search. No action required.
CloudTrail Alert: IAM: Create/Delete Roles	CloudTrail alert triggered by creation or deletion of roles in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: IAM: Create/Delete/Update Access Keys	CloudTrail alert triggered by creation, deletion, or update of access keys in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: IAM: Create/Delete/Update Groups	CloudTrail alert triggered by creation, deletion, or update of groups in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: IAM: Create/Delete/Update Users	CloudTrail alert triggered by creation, deletion, or update of users in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: IAM: Group Membership Updates	CloudTrail alert triggered by group membership changes in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: Instances: Reboot/Stop/Terminate Actions	CloudTrail alert triggered by reboot, stop, or termination actions in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: Instances: Run/Start Actions	CloudTrail alert triggered by run or start actions in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: Key Pairs: Create/Delete/Import Key Pairs	CloudTrail alert triggered by creation, deletion, or importation of Key Pairs in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: Security Groups: Create/Delete Groups	CloudTrail alert triggered by creation or deletion of security groups in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: Unauthorized Actions	CloudTrail alert triggered by any unauthorized actions in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: VPC: Create/Delete VPC	CloudTrail alert triggered by the creation or deletion of VPCs in AWS.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: VPC: Create/Delete/Attach Network Interfaces	CloudTrail alert triggered by creation, deletion, or attachment of network interfaces in VPCs.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail Alert: VPC: Create/Delete/Replace Network ACLs	CloudTrail alert triggered by creation, deletion, or replacement of network ACLs in VPCs.	To use this alert, enable this alert on the Alerts page in the app.
CloudTrail EventName Generator	Extracts the eventnames from CloudTrail.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every twenty minutes on the hour, twenty minutes past the hour, and forty minutes past the hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
CloudWatch: Topology CPU Metric Generator	Gets past day's average value for CPU Percentage from CloudWatch every hour. It is used on topology dashboard in the KPI tooltip and CPU Utilization layer.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
CloudWatch: Topology Disk IO Metric Generator	Gets past day's average value for Disk IO Operation Count from CloudWatch every hour. It is used on topology dashboard in the KPI tooltip.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour, and forty minutes past the hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
CloudWatch: Topology Network Traffic Metric Generator	Gets past day's average value for Network IO Size from CloudWatch every hour. It is used on topology dashboard in the KPI tooltip and the Network Traffic layer.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
CloudWatch: Topology Volume IO Metric Generator	Gets past day's average value for Volume IO Operation Count from CloudWatch every hour. It is used on topology dashboard in the KPI tooltip.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
CloudWatch: Topology Volume Traffic Metric Generator	Gets past day's average value for Volume IO Size from CloudWatch every hour. It is used on topology dashboard in the KPI tooltip and the Network Traffic layer.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
Config: Topology Daily Snapshot Generator	Generates daily snapshot of AWS topology.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every day. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
Config: Topology Monthly Snapshot Generator	Generates monthly snapshot of AWS topology.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every month. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
Config: Topology History Appender	Appends new AWS Config data collected through the Splunk Add-on for AWS to summary index, which is used to generate the AWS topology snapshot.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
Config: Topology Playback Appender	Converts AWS Config data into topology summary index used by the topology playback feature.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every day. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
Config: Topology History Generator	Migrates previous AWS Config data before update to summary index, which is used to generate the AWS topology snapshot.	Automatically scheduled to run once shortly after upgrade. No action required. You can also manually run this saved search after upgrading the app from an earlier version.
Config Rules: Topology Config Rules Generator	Generates Config Rules data for the Amazon Inspector & Config Rules layer on the Topology dashboard.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
Config Rules Alert: New Non-Compliant Resource	Sends an alert when a new non-compliant resource is found by Config Rules during the previous day.	To use this alert, enable this alert on the Alerts page in the app.
Insights Alert: Billing Anomaly Detection	Used for alerting the user when anomalies have been detected in Billing data.	To use this alert, enable this alert on the Alerts page in the app.
Insights Alert: Security Anomaly Detection	Used for alerting the user when anomalies have been detected in CloudTrail data.	To use this alert, enable this alert on the Alerts page in the app.
Insights: ELB, Insights: EIP, Insights: EBS	Used to generate insights.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.
ELB Alert: latency	Sets off an alert when ELB latency is greater than 100 seconds.	To use this alert, enable this alert on the Alerts page in the app.
Machine Learning Recommendation	This saved search runs every day to generate Recommendations on the Topology dashboard.	Automatically enabled. No action required. Scheduled to run every night at 9pm. Splunk recommends that you not run this search manually.
RI Expiration Alert - RI Plans expired within one month	Sets off an alert when an RI plan is about to expire within one month.	To use this alert, enable this alert on the Alerts page in the app.
Addon Synchronization	Synchronizes macro searches between the Splunk Add-on for AWS and the Splunk App for AWS.	If you use any indexes other than main, run this macro to update the app's index macro.
VPC Flow Logs Summary Generator (Dest Port, Dest IP, Src IP)	Generates VPC Flow Logs data in summary index.	Automatically enabled when you configure any input through the Splunk App for AWS Configure tab. Scheduled to run every hour. If you configure all your inputs through the Splunk Add-on for AWS instead, you should manually enable and schedule this saved search.

## Lookups for the Splunk App for AWS

The Splunk App for AWS includes lookups that map data from AWS to support dashboard displays. The lookup files are located in $SPLUNK_HOME/etc/apps/splunk_app_aws/lookups.

Filename	Description
all_eventName.csv	Maps IAM event names to an alert level and boolean for notable event status.
cn_price.csv	Maps instance_type to region, instance_type, region, on_demand_hourly, reserved_one_all_yearly, reserved_one_partial_yearly, reserved_one_partial_hourly
price.csv	Maps instance_type to region, instance_type, region, on_demand_hourly, reserved_one_all_yearly, reserved_one_partial_yearly, reserved_one_partial_hourly
regions.csv	Maps AWS region strings to latitude and longitude calculations and friendly names.
resource_timeline_services.csv	Maps serviceID to serviceName
unauthorized_errorCode.csv	Maps four variations on unauthorized error strings to a boolean value.
well_known_ports.csv	Maps name to port, name

## Data models for the Splunk App for AWS

The Splunk App for AWS includes five data models to support dashboard performance.

Name	Purpose	Accelerated	Action required
CloudFront Access Log	Supports the Overview and CloudFront - Traffic Analysis dashboards.	Yes	None
S3 Access Log	Supports the S3 - Traffic Analysis dashboard.	Yes	None
Detailed Billing	Supports the Historical Detailed Bills dashboard.	Yes	None
Instance Hour	Supports the Capacity Planner dashboard.	Yes	None

## Macros for the Splunk App for AWS

The Splunk App for AWS includes a set of macros that support dashboard performance. In most circumstances, you do not need to edit these macros.

Name	Default macro definition	Update required if you manage inputs from the add-on rather than the app
aws-cloudtrail-index	(index="main" OR index="aws-cloudtrail")	If you are using any index for your CloudTrail data other than main, aws-cloudtrail, or another default index you have set for your environment, add it to this definition.
aws-config-index	(index="main" OR index="aws-config")	If you are using any index for your Config data other than main, aws-config, or another default index you have set for your environment, add it to this definition.
aws-billing-index	(index="main" OR index="default")	If you are using any index for your Billing data other than main or another default index you have set for your environment, add it to this definition.
aws-cloudwatch-index	(index="main" OR index="default")	If you are using any index for your CloudWatch data other than main or another default index you have set for your environment, add it to this definition.
aws-description-index	(index="main" OR index="default")	If you are using any index for your Description data other than main, add it to this definition.
aws-config-rule-index	(index="main" OR index="default")	If you are using any index for your Config Rule data other than main, add it to this definition.
aws-inspector-index	(index="main" OR index="default")	If you are using any index for your Amazon Inspector data other than main, add it to this definition.
aws-s3-index	(index="main")	If you are using any indexes for your S3 access logs, ELB access logs, and CloudFront access logs other than main, add them to this definition.
aws-health-index	(index="main")	If you are using any index for your AWS Personal Health data other than main, add it to this definition.
aws-cloudwatch-logs-index	(index="main" OR index="default")	If you are using any indexes other than main for your CloudWatch Logs data, including any data that you collect through the add-on's Kinesis input, add it to this definition.