# INSTALL

## About the Splunk App for AWS

The Splunk App for AWS gives you critical operational and security insight into your Amazon Web Services account.

The app includes:

- A pre-built knowledge base of dashboards, reports, and alerts that deliver real-time visibility into your environment.
- A logical topology dashboard that displays your entire AWS infrastructure to help you optimize resources and detect problems.
- Insights dashboards that display detected problems in your AWS environment and provide best practice recommendations to help you optimize AWS resources, including EC2, EIP, ELB, and EBS.

If you are a Splunk software administrator, install this app and all required dependencies to your Splunk platform deployment. See [Hardware and software requirements for the Splunk App for AWS](http://docs.splunk.com/Documentation/AWS/5.1.1/Installation/Hardwareandsoftwarerequirements) to plan your deployment.

If you are a Splunk software user, check out the User Manual to get familiar with the dashboards.

Access the Release Notes for a list of new features, fixed issues, and known issues in the current release.

## Hardware and software requirements for the Splunk App for AWS

### Splunk platform requirements

The Splunk App for AWS runs on the following Splunk platforms:

- Splunk Cloud 6.3.1511 and later
- Splunk Enterprise 6.5.0 and later
- Splunk Light 6.5.1 and later

Because this app runs on the Splunk platform, all of the system requirements apply for the Splunk software that you use to run this app.

- If you plan to run this app in Splunk Cloud only, there are no additional requirements.
- If you plan to manage on-premises heavy forwarders to get data in to Splunk Cloud, see System Requirements in the Installation Manual in the Splunk Enterprise documentation, which includes information about forwarders.
- If you plan to run this app in an on-premises deployment of the Splunk platform, see System Requirements in the Installation Manual in the Splunk Enterprise documentation.
- If you plan to run this app in a self-managed AWS instance, there are no additional requirements. Refer to the Virtual hardware information for sizing considerations specific to AWS.

### Splunk Add-on for Amazon Web Services

The Splunk App for AWS relies on the Splunk Add-on for Amazon Web Services version 4.4.0 or later. Both the add-on and the app need to be installed for the app to function. For information about installing the Splunk Add-on for AWS, see Installation and configuration overview for the Splunk Add-on for AWS in the Splunk Add-on for AWS manual. Use the add-on setup and configuration user interface to link to your AWS account and configure data collection.

> The "Addon Metadata - Summarize AWS Inputs" saved search is included in the Splunk Add-on for AWS and is disabled by default, but it is recommended that you enable this saved search on the add-on side. The saved search is used to aggregate inputs data into the summary index.

### Python for Scientific Computing

If you are running this app on Splunk Enterprise or Splunk Cloud, the Recommendations Service feature depends on the Python for Scientific Computing app version 1.1 or later, available on Splunkbase or in your in-product app browser. Install the app appropriate for your environment on all Splunk search heads running the Splunk App for AWS.

- Python for Scientific Computing for Linux 64-bit version 1.1 or later
- Python for Scientific Computing for Windows 64-bit version 1.1 or later
- Python for Scientific Computing for Linux 32-bit version 1.1 or later
- Python for Scientific Computing for Mac version 1.1 or later

> Splunk Light does not support the Recommendations Service feature and therefore does not require the Python for Scientific Computing app as a prerequisite.

### AWS region limitations

The Splunk Add-on for AWS supports all regions offered by AWS.

If you are in the AWS China region, the add-on only supports the services that AWS supports in that region. The China region does not support Config Rules, Inspector, CloudWatch Logs, or CloudFront services, nor does it offer CloudWatch metrics for ELB logs. For an up-to-date list of what products and services are supported in this region, see <http://www.amazonaws.cn/en/products/>.

If you are in the AWS GovCloud region, the add-on only supports the services that AWS supports in that region. The GovCloud region does not support Config Rules, or Inspector at this time. For an up-to-date list of what services and endpoints are supported in this region, see the AWS documentation: <http://docs.aws.amazon.com/govcloud-us/latest/UserGuide/using-services.html>.

## Install the Splunk App for AWS on Splunk Enterprise

### Download the app and the add-on

You can download both the Splunk app and add-on on for AWS on Splunkbase.

- Splunk App for AWS version 5.1.0.
- Splunk Add-on for Amazon Web Services version 4.4.0 or later. If you are migrating from an existing installation of the Splunk Add-on for AWS, you can upgrade the add-on in place. The new version of the add-on is backwards compatible with older versions.

### Install on a single instance

If your Splunk Enterprise deployment is a single instance, install both the app and the add-on to your single instance. You can use the Install app from file feature in the Manage Apps page in Splunk Web to install both packages, or install manually using the command line.

### Install in a non-clustered distributed environment

If your Splunk Enterprise deployment is distributed and non-clustered, follow these steps.

1. Install both the app and add-on to your search heads.
2. Turn off add-on visibility on your search heads.
3. Configure the search head tier to directly forward data to the indexer tier.
4. Distribute the summary index configurations to the indexer.
5. Install the add-on to a heavy forwarder.

#### Install the app and the add-on to your search heads

If you are installing to one or more independent search heads, follow your preferred method of deploying both the app and the add-on. You can:

- follow the Install app from file wizard on the Manage Apps screen in Splunk Web.
- install manually using the command line.
- use a deployment server to deploy the unconfigured packages to your search heads. Do not configure the app or add-on prior to deploying it.

#### Turn off visibility for the add-on on your search heads

After you have deployed the app and the add-on to your search heads, change the visibility setting for the add-on on each search head to make it not visible. This step helps prevent data duplication errors that can result from running inputs on your search heads instead of (or in addition to) on your data collection node.

1. Go to Apps > Manage Apps.
2. Find the Splunk Add-on for AWS, with the folder name Splunk_TA_aws, in the list, and click Edit properties.
3. Under Visible, click the radio button next to No.
4. Click Save.
5. Repeat these steps on all search heads.

#### Configure the search head tier to directly forward data to the indexer tier

1. Create a outputs.conf file following the example below:
    ```properties
    [indexAndForward]
    index = false  # Turn off indexing on the search head
    [tcpout]
    defaultGroup = my_search_peers  # Name of the search peer group
    forwardedindex.filter.disable = true
    indexAndForward = false
    [tcpout:my_search_peers]
    server=10.10.10.1:9997,10.10.10.2:9997,10.10.10.3:9997  # list of peers
    ```
2. Place the `outputs.conf` file under `$SPLUNK_HOME/etc/apps/splunk_app_aws/local` on the search head.

3. Restart the search head.

#### Distribute the summary index configurations to the indexer

Copy `$SPLUNK_HOME/etc/apps/splunk_app_aws/default/indexes.conf` from the search head to a temporary directory on the indexer and then merge all the settings in the file into `$SPLUNK_HOME/etc/apps/search/local/indexes.conf` to incorporate the summary index configurations.

#### Install the add-on to heavy forwarders

Follow your preferred method of installing the Splunk Add-on for Amazon Web Services to one or more heavy forwarders. You can:

- follow the Install app from file wizard on the Manage Apps screen in Splunk Web.
- install manually using the command line.
- use a deployment server to deploy the unconfigured packages to your forwarders. Do not configure the app or add-on prior to deploying it.

> Note: The add-on does not support universal forwarders or light forwarders because the configuration logic handled by the add-on requires Python. In addition, to configure AWS accounts in the add-on, you must do so using the add-on's configuration UI in Splunk Web rather than in the configuration files.

### Install in a clustered distributed environment

To accelerate reporting, the Splunk App for AWS uses summary indexing that builds separate summary indexes on the search head. If you are deploying the Splunk App for AWS in a clustered environment, you need to distribute the summary index configuration bundle across all the clustered indexers and configure your individual or clustered search heads to directly forward data to the indexer tier so that data summary can be shared across all the search heads.

1. Install the app and the add-on to your search head cluster.
2. Turn off visibility for the add-on on your search heads.
3. Configure the search head tier to directly forward data to the indexer tier.
4. Distribute the summary index configuration bundle across clustered indexers.
5. Install the add-on to heavy forwarders.

#### Install the app and the add-on to your search head cluster

Install the app and the add-on using the deployer. See Use the deployer to distribute apps and configuration updates in the Distributed Search manual in the Splunk Enterprise documentation.

To prepare the app and add-on for deployment in a search head cluster, some files must be removed to prevent validation errors on startup:

On the deployer, remove the eventgen.conf file from the add-on folder: $SPLUNK_HOME/etc/shcluster/apps/Splunk_TA_aws/default
On the deployer, remove the inputs.conf file from the add-on folder: $SPLUNK_HOME/etc/shcluster/apps/Splunk_TA_aws/default
On the deployer, remove all files in the folder $SPLUNK_HOME/etc/shcluster/apps/Splunk_TA_aws/samples.

#### Turn off visibility for the add-on on your search heads(2)

To turn off visibility for the add-on, update the app.conf.

On the deployer, create an app.conf file in the folder $SPLUNK_HOME/etc/shcluster/apps/Splunk_TA_aws/local.
Edit the local/app.conf file.
Turn off visibility using the is_visible setting. Example:
[ui]
is_visible =  false

#### Configure the search head tier to directly forward data to the indexer tier(2)

1. Create a outputs.conf file following the example below:
    ```properties
    [indexAndForward]
    index = false  # Turn off indexing on the search head
    [tcpout]
    defaultGroup = my_search_peers  # Name of the search peer group
    forwardedindex.filter.disable = true
    indexAndForward = false
    [tcpout:my_search_peers]
    server=10.10.10.1:9997,10.10.10.2:9997,10.10.10.3:9997  # list of peers
    ```
2. If you use clustered search heads, place the outputs.conf file under $SPLUNK_HOME/etc/shcluster/apps/splunk_app_aws/local and run the splunk apply shcluster-bundle command on the deployer to push the configuration bundle to peers. If you use multiple independent search heads, place the outputs.conf file under $SPLUNK_HOME/etc/apps/splunk_app_aws/local on all the search heads.

3. Restart the search head instances.

#### Distribute the summary index configuration bundle across clustered indexers

On the indexer cluster master node, merge all the settings from $SPLUNK_HOME/etc/apps/splunk_apps_aws/default/indexes.conf into $SPLUNK_HOME/etc/master-apps/_cluster/local/indexes.conf to incorporate the summary index configurations.
On the master node, run this CLI command to distribute the indexes.conf to the peer nodes:
splunk apply cluster-bundle

When the configuration bundle distribution is complete, the indexes.conffile is copied to $SPLUNK_HOME/etc/slave-apps/_cluster/local on the peer nodes.

#### Install the add-on to heavy forwarders(2)

Follow your preferred method of deploying the Splunk Add-on for Amazon Web Services to one or more heavy forwarders. You can:

follow the Install app from file wizard on the Manage Apps screen in Splunk Web.
install manually using the command line.
use a deployment server to deploy the unconfigured packages to your forwarders. Do not configure the app or add-on prior to deploying it.
Note: The add-on does not support universal forwarders or light forwarders because the configuration logic handled by the add-on requires Python. In addition, to configure AWS accounts in the add-on, you must do so using the add-on's configuration UI in Splunk Web rather than in the configuration files.

## Use a custom index for storing AWS accounts and inputs data

Most configuration for the app is handled in the add-on. For information on how to set up and manage the configuration for your AWS accounts and inputs using the Splunk Add-on for AWS, see Installation and configuration overview for the Splunk Add-on for AWS.

By default, your AWS accounts and inputs data are stored in a predefined index named "summary." If you want to use a custom index, perform the following steps:

Create an index in which you want to store AWS accounts and inputs data. You must create the index on an indexer or indexer cluster, and not on a search head or heavy forwarder. See Create custom indexes for information about creating an index.
In the Splunk Add-on for AWS, modify the aws-account-index and aws-input-index macros to include the custom index you created.
Go to Settings > Advanced Search > Search Macros.
Select the the macro from the list.
For the index field, replace summary with the name of the index you created.
In the Splunk Add-on for AWS, run these saved searches: Addon Metadata - Migrate AWS Accounts and Addon Metadata - Summarize AWS Inputs.
Go to Settings > searches, reports, and alerts.
In the Actions column, click Run for each saved search.
In the Splunk App for AWS, modify the aws-account-summary and aws-input-summary macros to include the custom index you created.
Go to Settings > Advanced Search > Search Macros.
Select the macro from the list.
For the index field, replace summary with the name of the index you created.
In the Splunk App for AWS, run the Addon Synchronization saved search to sync the macros.

## Upgrade the Splunk App for AWS

When you upgrade from a previous version to the 5.1.0 version of the Splunk App for AWS, be aware of the following changes.

This version of the app requires Splunk Add-on for AWS 4.4.0 or later.
The Topology and EC2 Insights, and Insights Overview dashboards require the use of the Python for Scientific Computing libraries to be fully functional.
Upgrade from version 5.0.x to 5.1.0
Perform an in-place upgrade, no additional configuration steps required.

Upgrade from version 4.1.0 to 5.1.0
Perform an in-place upgrade. If you use a non-clustered distributed Spunk deployment, you do not need to perform any additional upgrade activity. If you use a clustered distributed Splunk deployment, you need to perform some additional steps:

Configure the search head tier to directly forward data to the indexer tier.
Distribute the summary index configuration bundle across clustered indexers.
For detailed instructions, see Install in a clustered distributed environment.

Upgrade from version 4.0.0 to 5.1.0
You need to upgrade from version 4.0.0 to 4.1.0 first, then from 4.1.0 to 5.1.0. See Upgrade guide for the Splunk App for AWS in the 4.1.0 version of the documentation for upgrade steps and new behavior to be aware of.

Upgrade from a pre-4.X version to 5.1.0
If you are upgrading from a pre-4.X version of the app, install the version as a new app. Starting from version 4.X, the app has a new folder name, so it does not replace 3.X or older versions in your environment. See Migrate from an unsupported version of the Splunk App for AWS in the version 4.0.0 documentation.