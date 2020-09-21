# Administering Splunk Enterprise Security

## Module 5: Installation

### Objectives

- List ES pre-installation requirements
- Identify steps for downloading and installing ES
- Test a new install

### Installation Checklist

- Assuming prep work from previous module is completed, these are the steps for a single server or distributed (non-clustered) site:
  1. Install ES app on search head
  2. Disable un-needed add-ons
  3. Create Splunk_TA_ForIndexers and deploy to indexers
  4. Deploy input-time add-ons to forwarders
- If you’re using deployment server to deploy ES-installed apps and add-ons, disable it before the installation, and re-enable after installation
  - <http://docs.splunk.com/Documentation/ES/latest/Install/DeploymentPlanning#Using_the_deployment_server_with_Splunk_Enterprise_Security>

### ES Apps and Add-ons

Tech Add-ons (input, normalization)
ES Main App
Domain Add-ons (views, UI components)
Supporting Add-ons (Searches, macros, data models, utilities)

### ES Installs These Apps

Domain add-ons

- DA-ESS-AccessProtection
- DA-ESS-EndpointProtection
- DA-ESS-IdentityManagement
- DA-ESS-NetworkProtection
- DA-ESS-ThreatIntelligence

Supporting add-ons

- SA-AccessProtection
- SA-AuditAndDataProtection
- SA-EndpointProtection
- SA-IdentityManagement
- SA-NetworkProtection
- SA-ThreatIntelligence
- SA-UEBA
- SA-Utils
- Splunk_SA_CIM
- Splunk_SA_ExtremeSearch

Stand-alone TAs

- Splunk_TA_bluecoat-proxysg
- Splunk_TA_bro
- Splunk_TA_flowfix
- Splunk_TA_juniper
- Splunk_TA_mcafee
- Splunk_TA_nessus
- Splunk_TA_nix
- Splunk_TA_oracle
- Splunk_TA_ossec
- Splunk_TA_rsa-securid
- Splunk_TA_sophos
- Splunk_TA_sourcefire
- Splunk_TA_symantec-ep
- Splunk_TA_ueba
- Splunk_TA_websense-cg
- Splunk_TA_windows

ES TAs

- TA-airdefense
- TA-alcatel
- TA-cef
- TA-fortinet
- TA-ftp
- TA-nmap
- TA-tippingpoint
- TA-trendmicro

Main ES Application

- SplunkEnterpriseSecuritySuite

### What Gets Installed Where

- Install the full ES app on the search head
  - Installs all DAs, SAs, and TAs
- Create and install Splunk_TA_ForIndexers on indexers and heavy forwarders
  - Includes all configurations from all enabled tech add-ons as well as indexes.conf settings
- Install TAs on forwarders if they do input phase actions
  - See TA readme files and their inputs.conf and props.conf files
- Which .conf settings are active during input or index time: <http://wiki.splunk.com/Where_do_I_configure_my_Splunk_settings>

### Typical Server Architecture

Search Head(s) ES app + all DAs, SAs and TAs
Indexers & heavy forwarders ES index configurations And index-time TA configurations (via Splunk_TA_ForIndexers)
Universal Forwarders gather operational and security data and send to indexers or heavy forwarders
Forwarders input-time TAs

### Installing on a Single Search Head

- Start with a clean basic Splunk installation
- Do not uninstall any of the default apps which are part of the basic Splunk package
  - They are required by ES
- ES functions best without the installation of additional apps on top of the basic Splunk package

### Uploading the ES App

1. Obtain the ES App from SplunkBase/sales rep
2. Upload ES App on the designated ES search head
3. Restart Splunk

### Starting the Installation

- After re-starting Splunk, navigate to the ES app
- You will be prompted to set up the app
  - Click Continue to app setup page to begin the installation process

### Installation: App Management

- The first step of the installer allows you to select apps to either exclude or disable
- Select any apps to disable or exclude, then click Start Configuration Process to continue

### Installation: Complete

When the installation process is complete, you’ll be prompted to re- start the server again

### Splunk Web Now on HTTPS

- ES converts Splunk Web to HTTPS
  - Port is not changed   - You can change the server back to HTTP in web.conf if desired
- The pre-loaded SSL certificates are self-signed
  - This causes a browser warning, but they are completely secure
  - You can install your own externally validated certificates <http://docs.splunk.com/Documentation/Splunk/latest/Security/Howtogetthird-partycertificates>

### ES Is Installed on the Search Head

- ES also installs:
  - Extreme Search   - *Nix or Windows add-on
- The Stream app, if installed, can be integrated with ES
- If you want the Add-on Builder or ES Health Check App, you’ll need to install them yourself

*Nix Add-on
Extreme Search
Stream App
ES

### Standard ES Add-Ons

- ES ships with several add-ons for common security data sources
  - See docs.splunk.com/Documentation/ES/latest/Install/ InstallTechnologyAdd-ons for a complete list
- Each add-on is related to a specific vendor product or technology
- Each has a specific add-on name and one or more event sourcetypes
- Some, like the FTP, *NIX, and Windows add-ons, are designed to input OS data and will require configuration before use
- See the README file in each add-on to for configuration steps

### Disable Unused ES Add-ons

- Tech add-ons are intended for use with specific technologies
  - Example: Splunk Add-on for Websense, Splunk Add-on for Trend Micro, etc.
- If you don’t use the product associated with an add-on, disable it on the search head and do not deploy it to indexers or forwarders

### Installing ES on a Search Head Cluster

- Set up a staging splunk instance
  - A testing or QA Splunk instance that has no other apps installed, and is not connected to production indexers or search peers
- Install ES on the staging instance as per preceding instructions
- Copy all ES-installed apps and add-ons from /etc/apps on the staging instance to /etc/shcluster/apps on the cluster deployer instance
- Use the cluster deployer to push apps to other cluster members

### ES Configuration Page

Navigate to ES > Configure > All Configurations

### Distributed Configuration Management

- Many TAs include index-time configuration settings in props.conf and transforms.conf
- ES uses indexes that must be added to all indexers <http://docs.splunk.com/Documentation/ES/latest/Install/Indexes>
- Use the ES Distributed Configuration Management tool to create a special add-on named Splunk_TA_ForIndexers.spl
  - Manually send this to your indexers, or
  - If your indexers are not clustered, use auto deployment

### Deploy Indexer Configurations

- Navigate to ES > Configure > General > Distributed Configuration Management
- Leave auto deployment set to no
- Click Download the Package
  - Splunk_TA_ForIndexers.spl is downloaded to your workstation
- Deploy to your indexers via deployment server, cluster master, or manual copy
  - docs.splunk.com/Documentation/ES/latest/Install/ InstallTechnologyAdd-ons

### Auto Indexer Deployment

- Select Yes for auto deployment
- Enter credentials for your indexer server management port
- Enter the list of indexers
- Optionally send indexes.conf
  - However, you might need per-server settings if the indexers have different storage volume settings
- Click Save
  - The configurations for TAs and indexes will be automatically deployed
- Note: auto deployment is not available for index clusters

### Data Integrity Control

- You can optionally enable data integrity control to ensure that the data ES relies on in indexes is not tampered with
- Data integrity applies hashes on all indexed data
- Configure in indexes.conf or on the index properties in settings
  - Set enableDataIntegrityControl to true and re-start server   - Only new inputs will be hashed
  - Can be set per-index or globally
- Test integrity from the command line or script:

```bash
bin/splunk check-integrity   - index indexname
```

### Data Protection Audit

- Audit > Data Protection
- Displays status of data protection settings per index
- Also displays status for sensitive data if the Personally Identifiable Information Detected correlation search is enabled

### Splunk Stream and ES

- ES can use wire data captures from the Splunk Stream app
  - Supports Protocol Intelligence
- Install the Splunk Stream app on the ES server
- Install the Stream add-on (Splunk_TA_stream) on machines where you want to capture data
- See docs.splunk.com/Documentation/StreamApp for details on installing and configuring Stream
- See docs.splunk.com/Documentation/ES/latest/Install/ InstallTechnologyAdd-ons for details on integrating Stream with ES

### Stream Data Flow

Splunk ES With Stream app Execute and display search results
Captured data does not include message content unless specifically configured
Indexers Store captured stream data
Production Servers with forwarders and Stream add-on Capture network data and forward to indexers

### Lab Exercise 5: Post-install Configuration

- Time: 15 minutes
- Tasks:
- Disable un-needed add-ons
- Create a Splunk_TA_ForIndexers add-on
- Configure role permissions