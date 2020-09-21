# Administering Splunk Enterprise Security

## Module 12: Threat Intelligence Framework

### Objectives

- Describe threat lists and threat list administration tools
- Configure a new threat list

### The Threat Intelligence Framework

- The Threat Activity Detected correlation search creates a notable event if it detects an indicator of compromise (IOC) contained in a threat intelligence collection
- The threat intel collections are populated automatically by downloads from external threat libraries
- Threats are categorized by:
  - Group: the source or entity originating the threat   - Category: the type of threat, like backdoor, APT, financial, etc.
  - Collection: organized by threat method or routing, such as email, file, process, user, etc.

### Threat Intelligence > Threat Activity

### Using Threat Activity

- Displays events related to known threat sites over the desired time period
- Panels
  - Threat activity over time by threat collection   - Most active threat collections and sources
  - Threat activity detail
- Filters
  - Threat group: a known threat source—i.e., “who”   - Threat category: threat type, such as APT, backdoor, etc.
  - Threat Match Value: Choose a filter from a list of fields

### Threat Activity Details

- Use threat details to examine the most recent threat events, including source, destination, sourcetype (i.e., how was it detected), threat collection, group, and category
- You can also filter or highlight as per the other Advanced Threat dashboards
  - Select one or more rows, then click Advanced Filter

### Threat Artifacts

### Using Threat Artifacts

- Threat Artifacts displays the current content of the threat intelligence data that ES has downloaded
- You can use the filters at the top to select a threat artifact type, and then filter by fields relevant to the selected artifact type
- The threat overview panel displays the items that have been downloaded from threat lists or STIX/TAXII sources
- The sub-panels display statistics on the threat intelligence data by endpoint, email, network and certificate
- The tabs allow you to drill down into these categories and gain additional details for each type of threat

### Configuring Threat Intelligence

- ES can download the following threat intelligence types:
  - Threat lists: IP addresses of known malicious sites
  - STIX/TAXII: detailed information about known threats, including threat type, source, etc.
  - OpenIOC: Additional detailed information about known threats
- You can also configure local threat lists
- Many intel sources require regular refresh from external sources
- This information is used by the Threat Activity Detected correlation search

### The Threat Intelligence Framework(2)

- Threat intel is downloaded regularly from external and internal sources by the Threat Download Manager modular input
  - This data is parsed into KV store collections with “_intel” suffixes   - These are used as lookups during threat generation searches
- Threat gen searches run periodically (by default every 5 minutes) and scan for threat activity related to any of the threat collections
  - When threat matches are found, events are generated in the threat_activity index and appear in the Threat Intelligence data model
- This data model is scanned by the Threat Activity Detected correlation search and new notables for threat activity are created

### Threat Download Management

Configure > Data Enrichment > Threat Intelligence Downloads

Note
Threat downloads cannot have spaces in the names. Use underscores or dashes instead.

### Editing Threat Downloads

### About Threat Downloads

- Type field is generic—you can use your own labels
  - Except for TAXII feeds—must be “taxii”
- Post arguments are passed to the source server if needed
- Proxy settings allow you to get threat downloads to ES across your firewall
- Parsing options allow you to extract data from a formatted file
- Downloaded files are delivered to app/local/data/threat_intel and then parsed into threat intel collections
  - app is usually either DA-ESS-ThreatIntelligence or SA-ThreatIntelligence

### Manual Intelligence File Upload

- To manually upload one threat intelligence file:
  1.Navigate to Configure > Data Enrichment > Threat IntelligenceUploads
  2. Add a file name for an OpenIOC, STIX or CSV file
  3. Configure options for weight, category, etc.
  4. Click Save

### OpenIOC Batch File Upload

- OpenIOC threat collections are not handled via the threat intelligence downloader
- To add OpenIOC files to your ES threat intelligence framework:
  1. Copy the OpenIOC files to etc/apps/DA-ESS-ThreatIntelligence/local/data/threat_intel
  2. Check var/log/splunk/threat_intelligence_manager.log for the progress of the download
  3. Examine the contents of the target threat intel KV store collections for threat artifacts from the source OpenIOCs
- This can be done on an automated, scheduled basis

### Facebook ThreatExchange

- The Splunk Add-on for Facebook ThreatExchange integrates threat intelligence from Facebook’s ThreatExchange repository into the ES threat intelligence framework
- Install the app: splunkbase.splunk.com/app/3108/
- Configure the Facebook modular input:   - docs.splunk.com/Documentation/FBTX
- Requires an account at Facebook’s ThreatExchange site:
  - developers.facebook.com/products/threat-exchange

### Threat Intel Collections

- After download, threat intel data is stored in KV store collections with an “_intel” suffix, such as file_intel, ip_intel, email_intel, etc.
  - Use Settings > Lookups to see them all   - Use |inputlookup to examine their contents
- Use the Threat Artifacts dashboard to examine the overall contents of the entire threat intelligence framework

### Threat List Audit

- Audit > Threat Intelligence Audit displays status and time for all downloads as well as recent audit events
- Filter by enabled/disabled state, local/remote, source, etc.
- Failed downloads are reported in the system message list at the top of the SplunkWeb page

### Lab Exercise 12: Lookups and Threat Lists

- Time: 20 minutes
- Tasks:
- Edit an existing threat download
- Add a new threat list download

### Support Programs

- Community
  - The Splunk Answers Site: <http://answers.splunk.com> Post specific questions and get them answered by Splunk community experts.
  - Splunk Docs: docs.splunk.com These are constantly updated. Be sure to select the version of Splunk you are using.
  - Wiki: wiki.splunk.com

A community space where you can share what you know with other Splunk users.   -  IRC Channel: #splunk on the EFNet IRC server Many well-informed Splunk users “hang out” here.

- Global Support Support for critical issues, a dedicated resource to manage your account   -  24 x 7 x 365.
  - Email: support@splunk.com
  - Web: <http://www.splunk.com/index.php/submit_issue>
- Enterprise Support

Access you customer support team by phone and manage your cases online 24 x 7 (depending on support contract).
