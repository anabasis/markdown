# Using Splunk Enterprise Security

## 1. Getting Started With Enterprise Security

### Objectives

- Describe the features and capabilities of Splunk Enterprise Security (ES)
- Explain how ES helps security practitioners detect, prevent, and respond to threats
- Describe correlation searches and notable events
- Describe user roles in ES
- Log on to ES

### Overview of Splunk Enterprise Security

- Built on the Splunk Operational Intelligence platform
  – ES is a Splunk app, installed on a Splunk server
  – Leverages Splunk's powerful search capabilities
- Provides tools for security practitioners to detect, prevent, and respond to security threats and incidents
- Efficiently manage, analyze and mitigate security breaches
- Highly customizable for your specific enterprise requirements
- Real-time, scalable, context-aware, focused on content
- Makes all data—not just your “security data”—relevant to your security effort

### ES Users

- Security Analysts
- SOC Staff
- Security Execs/Mgrs
- Security Auditors

### ES Users (cont.)

### ES Functional Areas

#### Perimeter Defense

- Known threats
- Vulnerability alerts
- Unexpected/prohibited processes or traffic
- Threat activity
- Risk framework

  - Security posture
  - Correlation searches
  - Threat intelligence
  - Network capture

#### Preventative Analysis

- Anomaly detection
- Pattern matching
- Traffic analysis
- Statistical analysis

  - Predictive analytics*
  - Insider threat analysis
  - Advanced threat analysis
  - Risk analysis
  - Protocol intelligence

#### Breach Response

- Investigation journaling
- Incident tracking
- Forensics tools
- Asset and identity management
- Audit

  - Adaptive response
  - Investigation timeline
  - Asset and identity investigators
  - Dashboards, searches and reports for forensics and audit

### Advanced Persistent Threats (APT)

A growing, global threat aimed at undetected insertion, long-term viability, extraction/delivery of valuable information

- Focused attacks on specific systems
  – Recent examples: Yahoo, JPMorgan Chase, ...
- Targets: business, government, individuals
- Many delivery methods
- Metamorphic/polymorphic coding
- Constantly changing and adapting

<https://www.splunk.com/blog/2015/06/17/opm-apt-and-the-need-for-personalized-threat-intelligence/>

### The Kill Chain

|Stage|Attacker Activity|ES Countermeasures|
|:--:|:--|:--|
|Delivery|Email, website malware, social engineering, etc.|Threat lists, vulnerability scanning, real- time monitoring, access monitoring|
|Exploitation/Installation|Open attachment, download from site, upload from memory stick, etc.|Protocol Intelligence, file system alerts, intrusion detection, port monitoring|
|Command and Control|Execute code, open/copy files, change configuration, etc.|Malware tracking, process alerts, change alerts, analytics|
| Accomplish mission|Upload payload to remote server,disable services, etc|Traffic alerts, network analysis, audits|

Both attacker and defender can use the kill chain methodology

### Anatomy of an APT Attack

- Threat intelligence
- Network Activity/Security
- Host Activity/Security
- Auth - User Roles, Corp Context

이미지

### How ES Works

- Security-related data is acquired by add-ons in your enterprise from servers, routers, etc.
  - This data is forwarded to Splunk indexers and stored as events
- ES runs real-time searches, looking for indicators of threats, vulnerabilities, or attacks
  - If a search discovers something that needs attention, ES displays it on one or more of its dashboards
  - You can then investigate the issue, track it, analyze it, and take the appropriate action

### ES Use Cases

- Malware protection
  - Detection –Patient zero identification –Zero-day investigations
- Insider threat
  - Data exfiltration –Suspicious privileged account activity –Monitor threat activity with a glass table
- Detailed use case descriptions at:
  - <http://docs.splunk.com/Documentation/ES/latest/Usecases>

### ES Data Flow

Firewalls/Proxies

- cisco-pix
- pa-networks
- juniper-networks
- bluecoat

Network Capture (Stream)

- stream:tcp
- stream:udp
- stream:http ...

Vulnerability Scanners(port scanning, testing vulnerabilities)

- nessus
- mcafee

Intrusion Detection System (packet sniffing)

- snort
- dragon-ids
- mcafee

Production Servers (any operating system)

- microsoft-av
- linux-secure
- windows:*
- access-combined

Splunk ES (events, data models)

### Correlation Searches

- `Correlation searches` run in the background looking for specific types of threats, vulnerabilities, or signs of malicious attack
  - Searches can run in real-time
  - or on a schedule

- ES ships with many correlation searches, which can be modified or extended as needed
- If a correlation search finds something that requires attention,
  - An alert is triggered, which creates a `notable event`
  - Can also send emails, run scripts, update risk scores, etc.

### Notable Events

- Correlation searches create notable events in the `notable` index
  - A notable event might indicate a breach, vulnerability, or other issue
- Notable events are created with fields, event types, and tags that provide information necessary for incident investigation and a link to the original source event(s)
- You can search for the notable events in the notable index
  - In ES, select `Search > Search` to run a manual search
  - Run a search like index=notable for a given time period to see the notable events
  - Event `Source` fields show the correlation search that created the notable event

### Assets and Identities

- Notable event urgency is based on the priority of the `assets` and `identities` in your environments
  - `Assets`: devices in your enterprise, like routers and servers
    - Identified by IP number or MAC address
  - `Identities` are people in your enterprise
    - Identified by user name, email address, etc.
- Both are managed in ES with lookup tables
  - ES can show a meaningful name and descriptive information for a server or person instead of an IP number or user ID

### Investigation Timelines

- ES provides investigation journals to help track, coordinate, and manage ongoing investigations
- Visualize and document the progress of incident analyses
  - Notable events, search results, notes and other content
- Collaborate with other analysts

### Beyond Notable Events

- ES provides many advanced tools you can use to examine security data in detail, such as:
- Risk and threat analysis
- Web and user intelligence
- Protocol (stream) intelligence
- Adaptive response

- These tools will help you:
  - During forensic investigation of existing breaches
  - Analyze your environment for new threats
  - Examine the history of old breaches to understand how they happened and prevent them in the future

### ES Role Overview

ES Roles (required for ES login)

ESS User
Runs real-time searches and views all ES dashboards
User

ESS Analyst
Owns notable events and performs notable event status changes
Power

ESS Admin
Configures ES system- wide, including adding ES users, managing correlation searches, and adding new data sources
Admin

### Accessing ES

- Typically, ES runs on a secure (HTTPS) port, so the URL for the server will look something like this: <https://eshostname:8000>
- You must have an assigned role on the ES server
- Once you log on, you see the ES app in the list of apps on the Splunk home page
- You can configure ES to be your default app in your user preferences
  - Click your user name on the top menu bar

### The ES Home Page

Menu bar
Security Posture: monitor status
Incident Review: work on issues
Configuration tools
Documentation site
Community support
Product tour tutorial

### Correlation Searches(Setting)

Correlation searches drive most of Enterprise Security

- Select `Configure > Content Management` and in the `Type` drop-down, choose `Correlation`
- By default, only ES Admins can enable, disable, modify, or add new correlation searches

Type
Schedule

### ES Dashboards and Data Models

- How does raw security data become available to ES dashboards?
  - Splunk or a custom add-on indexes and sourcetypes the raw data
  - Events are mapped and `normalized` to the Splunk Common Information Model (CIM)
  - Events are referenced by the accelerated `CIM data models`
- All ES correlation searches, dashboards, and reports use these accelerated data models in their searches
- You can create your own custom searches based on the events in your index(es) or associated with your accelerated Data Models
  - Use `Search > Search` to begin creating a new search
  - Use `Search > Pivot` to create pivots using ES data models

### ES Search Example

- Use `tstats` or `datamodel` to create reports based on accelerated data models
  - With tstats, use `summariesonly=t` to restrict results to accelerated data for performance improvement
- Use `Search > Pivot` to build pivot reports using ES data models

### Notable Events Example

In the menu bar, click Search
Run a search in the notable index
In the dropdown, click Search
Under Selected Fields, click source, and note the list of sources found over the last 60 minutes, indicating which correlation searches have generated notable events

Lab Exercise 1: Introduction to ES

- Time: 15 minutes
- Tasks:
- Connect to the ES server
- Navigate between ES and other Splunk apps
- Configure your user account
- Examine the source security events ES is referencing
- Examine the correlation searches enabled on your server
- Examine the notable events ES has created