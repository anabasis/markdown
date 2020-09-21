# Using Splunk Enterprise Security

## 4. Forensic Investigation with ES

### Objectives

- Use ES to inspect events containing information relevant to active or past incident investigation
- Identify security domains in ES
- Use ES security domain dashboards
- Launch security domain dashboards from Incident Review and from action menus in search results

### ES and Forensic Investigation

- When a breach occurs, you need to examine the details related to the incident to determine a root cause and eliminate the risk
- The domain dashboards in ES provide the necessary tools to examine related log and stream data in depth
- You can also use these dashboards as part of a periodic security status evaluation
- The dashboards are organized by security domain

### Identifying Primary Domains

- `Access` : authentication attempts and access control related events (login, logout, access allowed, access failure, etc.)
- `Endpoint` : malware infections, system configuration, system state (CPU usage, open ports, uptime), patch status and history (which updates have been applied), and time synchronization information
- `Network` : information about network traffic provided from devices such as firewalls, routers, network-based intrusion detection systems, network vulnerability scanners, proxy servers, and hosts
- `Identity` : examine identity and asset lookup data

### Types of Domain Dashboards

Dashboard Type

Center, Operations

- High-level status and overview
- Simple filtering
- Drilldown|

Search

- Form-driven filters to search within a given topical area: malware, vulnerability, access, etc.

Changes

- Insight into modifications or other changes to network or endpoint systems

Specialized Dashboards for the Access Domain

- Examples: tracker, management, and activity

Most dashboards

- Provide time range pickers, business unit, and category filters

### Domain Dashboard Navigation

### How to Use Domain Dashboards

- Use the dashboards:
  - During forensic investigation of current or past security incidents
  - To drill down into root causes of notable events
  - Examining events related to an asset or identity you are investigating
  - To periodically evaluate the status of security-related events
- Access domain dashboards from:
  - The Security Domains menu
  - Actions menu in Incident Review
  - Field action menus in search results

### Accessing Forensics Dashboards

When expanding a notable event on the Incident Review dashboard, a field’s Action menu contains links to relevant security domain dashboards

- Example: while investigating the user Hax0r, click Access Search to navigate directly to the Access Search dashboard

### Access Domain: Investigation Scenarios

- The access domain is focused on user identity and authentication
- These dashboards provide tools to research:
  - Brute force attacks
  - Privileged account (i.e., root) misuse
  - Access by rare or new accounts
  - Access by expired or disabled accounts
  - Access via unusual applications (i.e., SSH, VNC, etc.)
- Security Intelligence menu User Activity dashboards provide more tools for access domain investigation

### Access Domain Correlation Searches

|Account Deleted|Geographically Improbable Access Detected|
|:--|:--|
|Brute Force Access Behavior Detected|High or Critical Priority Individual Logging into Infected Machine|
|Brute Force Access Behavior Detected Over 1d|Inactive Account Usage|
|Cleartext Password At Rest|Insecure Or Cleartext Authentication|
|Completely Inactive Account|Short-lived Account Detected|
|Concurrent App Accesses|Asset - Asset Ownership Unspecified|
|Default Account Usage|Activity from Expired User Identity|
|Default Accounts At Rest|High Volume Email Activity with Non- corporate Domains|
|Excessive Failed Logins|Web Uploads to Non-corporate Domains|

### Access Search

From Access Search for the user Hax0r, the Access Search dashboard is automatically populated with Hax0r’s login attempts

### Access Center Examples

- Examine access apps, by success / failure and by time
  - Large failure rates indicate brute force probing
- Examine access by source
  - High access rates from a single source can be malicious
- Access by unique users
  - User accounts with high rates of login activity may be compromised

### Dashboard Filtering

- All of the dashboards support some form of filtering
- For instance, the Access Center has filter options for action, app, business unit, category, and special access

### Other Access Domain Dashboards

`Access Tracker`
Account activity over time for:

• first time access
• inactive accounts
• expired identities

`Account Management`
Account actions, like:
• creation
• deletion
• lockout

`Default Account Activity`

Usage of default accounts, which are built-in to anoperating system, such as:
• root/administrator
• SYSTEM
• guest

### Endpoint Domain Investigation Scenarios

- The endpoint domain watches over your user systems, such as:
  - workstations, PCs, notebooks
  - handheld devices
  - point-of-sale systems

- Potential issues include:
  - Vulnerabilities: missing updates or patches
  - Malware: spyware, ransomware, or other malicious code
  - Unexpected running processes or services
  - Unexpected registry changes

### Endpoint Domain Correlation Searches

|Abnormally High Number of Endpoint Changes By User|Host With Excessive Number Of Processes|
|:--|:--|
|Anomalous New Listening Port|Host With Excessive Number Of Services|
|Anomalous New Processes|Host With Multiple Infections|
|Anomalous New Services|Multiple Primary Functions Detected|
|Anomalous User Account Creation|Old Malware Infection|
|High Number of Hosts Not Updating Malware Signatures|Outbreak Observed|
|High Number Of Infected Hosts|Prohibited Process Detection
|High Or Critical Priority Host With Malware|Prohibited Service Detection|
|Host Sending Excessive Email|Recurring Malware Infection|
|Host With Excessive Number Of Listening Ports|Should Timesync Host Not Syncing|

### Endpoint > Malware Center

- Overview of malware in your environment
  - Allowed vs. blocked
  - Types of infections
  - Statistics on most common infection types
  - New malware identification
- This dashboard can help you prioritize investigations based on infection scope and type

Click a malware signature on the Malware Center dashboard to drill down to that malware type on the Malware Search dashboard

### Endpoint > Malware Search – Patient Zero

- The Malware Search dashboard shows all infected systems, helping you identify the scope of the attack and also the origin
- The first infection should be patient zero

### Endpoint > Endpoint Changes

Track changes on your systems

- By type:
  - file
  - registry
  - etc.

- By system
- List of change details

### Endpoint Scenario: Ransomware

- There are many forms of ransomware trojan APTs in the wild
  - Example: Thor, a variant of Locky
    - Thor is often injected into an endpoint using a link in an email
    - Once activated, Thor begins encrypting files and replacing them with files with a .thor extension
- The Threat Activity Detected correlation search can detect Thor based on the file change activity
  - You can use the Endpoint Changes dashboard to investigate further

### Endpoint > Malware Operations

An overview of malware status, including:

- Infected systems
- Infection duration statistics
- Malware client (i.e., antivirus) information
- Statistics on repeat and aging infections

### Endpoint > Time Center

- Reports the status of time synchronization in your environment
- Systems not properly synchronizing will not send correct time- stamped data to Splunk
  - This in turn can lead to search failure and false negatives in ES

### Other Endpoint Domain Dashboards

|||
|:--|:--|
|System Center|Statistics about the operating systems and versions in use in your environment|
|Update Center|Statistics about the status of patches and other software updates
|Update Search|Search interface for update events|

### Network Domain Investigation Scenarios

- Many network domain scenarios are preventative in nature:
  - Suspicious activity spotted by intrusion detection systems
  - Vulnerabilities
  - Unusual ports being opened
  - Suspicious DNS activity
  - Port scanning
- Other network analysis tools on the Security Intelligence menu:
  - Protocol Intelligence
  - Web Intelligence

### Network Domain Correlation Searches

|||
|:--|:--|
|Excessive DNS Failures|Substantial Increase in Port Activity (By Destination)|
|Excessive DNS Queries|Unapproved Port Activity Detected|
|Excessive HTTP Failure Responses|Unroutable Host Activity|
|High Volume of Traffic from High or Critical Host|Unusual Volume of Network Activity|
|Network Device Rebooted|Vulnerability Scanner Detection (by event)|
|Policy Or Configuration Change|Vulnerability Scanner Detection (by targets)|
|Substantial Increase in an Event|Abnormally High Number of HTTP Method Events By Src|

### Network > Intrusion Center

This dashboard displays events logged from intrusion detection systems (IDS)

> Sourcefire
> Cisco
> McAfee

- Use filters to focus on: types of attacks, severity, business units
- Click any panel to drill down to the Intrusion Search dashboard

### Network > Intrusion Search

As an example, clicking on a system in the results panel displays the specific type of attack (in this case, a denial of service on an Oracle system)

### Network > Vulnerability Center

- Displays statistics on system security settings from vulnerability scanners, such as

>Tenable
>Nessus
>etc.

- Click in any panel to drill down to the Vulnerability Search dashboard

### Network > Vulnerability Search

1. Click a host
2. Events containing that host name are shown in the Vulnerability Search dashboard

### Web Center

- HTTP activity insights from
> web server
> proxy
> firewall logs

- Examine web activity by
> type
> status
> source
> destination

### Other Network Domain Dashboards

Vulnerability Operations
Statistics on:
>vulnerability aging
>scan activity

Network Changes
Events recording changes to network configurations on:
> routers
> firewalls
> etc.

Port and Protocol Tracker
Analysis of network activity by:
> port type
> protocol type

### Lab Exercise 4: Network Anomalies

- Time: 30 minutes
- Scenario:
- Work in the role of a network analyst performing forensic analysis on an open incident
- Task:
  - Follow a network notable event through several dashboards in the process of working the issue