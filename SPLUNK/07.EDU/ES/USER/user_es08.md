# Using Splunk Enterprise Security

## 8. Threat Intelligence

### Objectives

- Use the Threat Activity dashboard to see which threat sources are interacting with your environment
- Use the Threat Artifacts dashboard to examine the status of threat intelligence information in your environment

### Security Intelligence: Threat Intelligence

Threat intelligence provides tools to help security practitioners find and prevent potential external threats in your environment

Threat Activity
Examine activity from a threat perspective:

- which threats have been identified
- which systems or users are affected, etc.

Threat Artifacts

- Examine the details of threat intel that has been downloaded from online threat libraries

### The Threat Intelligence Framework

- If an indicator of compromise (IOC) is detected from a threat intelligence collection
  - `Threat Activity Detected` correlation search creates a notable event
  - Threat intel collections are populated automatically by downloads from external threat libraries
  - Threats are categorized by:
    - Group:
      - The source or entity originating the threat
    - Category:
      - The type of threat, like backdoor, APT, financial, etc.
    - Collection:
      - Organized by threat method or routing, such as email, file, process, user, etc.

### Threat Intelligence Downloads

ES downloads the following threat intelligence:

- Threat list:
  - A simple list of known malicious sites
- OpenIOC:
  - Indicators of Compromise—see <http://www.openioc.org>
- STIX/TAXII:
  - <https://stix.mitre.org/about/faqs.html> and <https://taxii.mitre.org/about/faqs.html>
- Facebook ThreatExchange:
  - <http://splunkbase.splunk.com/app/3108/>

### Threat Intelligence > Threat Activity

### Using Threat Activity

Displays events related to known threat sites over the desired time period

- Panels
  - Threat activity over time by threat collection
  - Most active threat collections and sources
  - Threat activity detail
- Filters
  - `Threat group` : a known threat source—i.e., “who”
  - `Threat category` : threat type, such as APT, backdoor, etc.
  - `Threat Match Value` : choose a filter from a list of fields

### Threat Activity Details

The most recent threat events: source, destination, sourcetype (i.e., how was it detected), threat collection, group, and category

- You can filter or highlight rows (as with Web Intelligence)
1. Select one or more rows
2. Click Advanced Filter

### Threat Artifacts

### Using Threat Artifacts

Current content of threat intelligence data that ES has downloaded

- You can use the filters at the top:
1. Select a threat artifact type
2. Filter by fields relevant to the selected artifact type
- The threat overview panel displays the items that have been downloaded from threat lists or STIX/TAXII sources
  - The sub-panels display statistics on the threat intelligence data
    - By endpoint, email, network and certificate
  - The tabs allow you to drill down into the categories to see more details about each type of threat

### Integrating Artifacts During Investigation

- Get more information about an active threat with Threat Artifacts dashboard
- Example: On the Threat Activity dashboard’s Most Active Threat Source panel, you see iblocklist_proxy is the most common threat source
  - In Threat Artifacts, you enter iblocklist_proxy in the Intel Source ID field and search
  - You learn that iblocklist_proxy is a CSV type threat list
  - In the Network tab, you can inspect the full list of known IP addresses from this threat list, including locations when known

### Add Threat Intelligence from a Search

- From any search or notable event that produces threat indicators:

```sql
Add | outputlookup local_<threat intelligence type>_intel append=t to the end of the search
```

Example: a search that produces a list of IP addresses that are testing a web server for vulnerabilities and add them to the local_ip_intel lookup to be processed by the modular input and added to the ip_intel KV Store collection

### Lab Exercise 8: External Threats

- Time: 35 minutes
- Scenario:
- You are investigating potential external threats
- Task:
- Use the threat activity dashboard to gain insight to the current state of known threats to your organization