# Administering Splunk Enterprise Security

## Module 1: Introduction to ES

### Objectives

- Describe the features and capabilities of Splunk Enterprise Security (ES)
- Explain how ES helps security practitioners detect, prevent, and respond to threats
- Describe correlation searches and notable events
- Describe user roles in ES
- Log on to ES

### Course Overview

- This course is for consultants and administrators who will be planning, installing, or configuring ES
- In the first part of the course, a very high-level overview of the ES features that are used by security practitioners are discussed
  - Focus is on how the product works for end-users
- In the second part, the details of deployment, installation and configuration are discussed
  - Focus is on how to get the system up and running, and tune it for site requirements

### Overview of Splunk Enterprise Security

- Built on the Splunk Operational Intelligence platform
  - ES is a Splunk app, installed on a Splunk server
- Leverages Splunk's powerful search capabilities
- Provides tools for security practitioners to detect, prevent, and respond to security threats and incidents
- Efficiently manage, analyze and mitigate security breaches
- Highly customizable for your specific enterprise requirements
- Real-time, scalable, context-aware, focused on content
- Makes all data—not just your “security data”—relevant to your security effort

### ES Users

Security Analysts
SOC Staff
Security Execs/Mgrs
Security Auditors
This image cannot currently be displayed.

### How ES Works

- Security-related data is acquired by add-ons in your enterprise from servers, routers, etc.
  - This data is forwarded to Splunk indexers and stored as events
- ES runs real-time searches, looking for indicators of threats, vulnerabilities, or attacks
  - If a search discovers something that needs attention, ES displays it on one or more of its dashboards
  - You can then investigate the issue, track it, analyze it, and take the appropriate action

### ES Data Sourcing

Firewalls/Proxies
cisco-pix
pa-networks
juniper-networks
bluecoat

Vulnerability Scanners
(port scanning, testing vulnerabilities)
nessus
mcafee

Intrusion Detection System
(packet sniffing)
snort
dragon-ids
mcafee

Network Capture (Stream)
stream:tcp
stream:udp
stream:http
...

Production Servers
(any operating system)
microsoft-av
linux-secure
windows:*
access-combined

Splunk ES
(events, data models)

### Correlation Searches

- Correlation searches run in the background to detect evidence of attacks, known threats, or vulnerabilities
  - These searches run either in real-time or on a schedule
- ES ships with many correlation searches, which can be modified or extended as needed
- Each correlation search is looking for one specific type of threat, vulnerability, or sign of malicious attack
- If a correlation search finds something that requires attention, an alert is triggered which creates a notable event
  - Can also send emails, run scripts, update risk scores, etc.

### Notable Events

- Correlation searches create notable events in the notable index
  - A notable event might indicate a breach, vulnerability, or other issue
- Notable events are created with fields, event types, and tags that provide information necessary for incident investigation and a link to the original source event(s)
- You can search for the notable events in the notable index
  - In ES, select Search > Search to run a manual search
  - Run a search like index=notable for a given time period to see the notable events
  - Event Source fields show the correlation search that created the notable event

### Beyond Notable Events

- ES provides many advanced tools you can use to examine security data in detail, such as:
  - Risk and threat analysis
  - Threat activity detection
  - Protocol (stream) intelligence
  - Adaptive response

- Security practitioners use these tools:
  - Forensic investigation of existing breaches
  - Analyze your environment for new threats
  - Examine the history of old breaches to understand how they happened and prevent them in the future

### ES Roles

ES Roles (required for ES login)
Runs real-time searches and views all ES dashboards
ES User
User
Admin
Standard Splunk Roles
Owns notable events and performs notable event status changes
ES Analyst
Power

Configures ES system- wide, including adding ES users, managing correlation searches, and adding new data sources
ES Admin

### Accessing ES

- Typically, ES runs on a secure (HTTPS) port, so the URL for the server will likely look something like this : <https://eshostname:8000>
- You must have an assigned role on the ES server
- Once you log on, you see the ES app in the list of apps on the Splunk home page
- You can configure ES to be your default app in your user preferences

Click your user name on the top menu bar

### The ES Home Page

Security Posture: monitor status
Incident Review: work on issues
Community support
Product tour tutorial
Documentation site
Configuration tools
Menu bar

### Active Correlation Searches

• Select Configure > Content Management, then select Correlation in the Type drop-down
• Note which searches are enabled or disabled
• By default, only ES Admins can enable, disable, modify, or add new correlation searches
Schedule

Type
Enabled/ Disabled

### Events and Data Models in ES

• As raw data is input into ES, it is processed as follows:
–
The raw data is converted into events and stored in indexes –Events are normalized into the Splunk Common Information Model –Events are added to accelerated data models
• All ES correlation searches, dashboards, and reports use these accelerated data models in their searches
• You can create your own custom searches based on the events in main or the data models as needed
–Use Search > Search to begin creating a new search –Use Search > Datasets to view or create datasets using data models

### ES Search Example

• Use tstats or datamodel to create reports based on accelerated data models
–With tstats, use summariesonly=t to restrict results to accelerated
data for performance improvement
• Use Search > Datasets to build datasets using ES data models

### Notable Events Example

• Select Search > Search and run a search in the notable index
• Note the list of sources found over the last 24 hours, indicating which correlation searches have generated notable events

### Lab Exercise 1: Introduction to ES

Time: 10 minutes Tasks:
• Connect to the ES server
• Navigate between ES and other Splunk apps
• Examine the source security events ES is referencing
• Examine the correlation searches enabled on your server
• Examine the notable events ES has created