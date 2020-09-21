# Administering Splunk Enterprise Security

## Module 3: Analyst Tools and Data Sources

### Objectives

- Identify ES security analyst tools
- Map security tools to data sources
- Examine management tools for analyst dashboards

### Security Intelligence

- Besides Security Posture and Incident Review, ES provides many dashboards and tools for security practitioners
  - Risk analysis –Protocol analysis –Threat intelligence
  - User intelligence –Web intelligence –Asset and identity viewers
  - Glass tables

### Dashboard Data Dependencies

- Each dashboard panel’s search pulls events from a data model
- If a panel is missing information, examine the panel’s search to see which data model is used; this can help you understand why the data is missing
- Causes:
  - The data is not in Splunk: install add-ons to input the data –The data is present in Splunk but is not normalized correctly: modify normalization settings
- docs.splunk.com/Documentation/ES/latest/User/DashboardMatrix

### Risk Analysis

- Correlation searches can add a numeric risk value to objects
  - Systems or users
- Risk can be increased by any event that occurs to an object
- The amount of risk assigned can be configured per-object and per- event
- This is different than priority, severity, or urgency
  - Allows you to see cumulative risk caused by multiple events over time
  - Allows you to fine-tune the way you interpret threats or vulnerabilities to your enterprise
- Admins configure risk values to correlation searches and objects

### Security Intelligence > Risk Analysis

Filter
Add an Ad-hoc Risk
Key Risk Indicators
Timeline shows most active risk-increasing events
Objects with most risk
Events affecting risk scores
Events causing the most risk

### Risk Data Sources

- Note that the Risk data model is the data source for the panels on the Risk Analysis dashboard
- Each panel has its own search
- Use a search like

```sql
|datamodel Risk All_Risk search to see the sources, sourcetypes and indexes that are being used in this data model
```

### Protocol Intelligence

- Protocol Intelligence in ES provides a set of tools to analyze network stream data
- Access the protocol intelligence dashboards using Security Intelligence > Protocol Intelligence
- Protocol Center: overview of network activity
- Traffic Size Analysis: Overall network traffic activity trends
- Three main subject groups, each with an activity overview and deep search capability, for DNS, SSL, and email

### Stream Events

- Stream events are generated from the Stream app
  - Optional install
- Stream events are stored with stream:xxxx source types
  - Examples: tcp, udp, dns, smtp, http
- Standard field extractions:
  - Capture time, type, size, source/dest info
- Depending on specific source type, additional fields are extracted
  - HTTP: cookies, request parameters, etc.
  - SMTP: sender, receiver, subject, summary of body
  - DNS: DNS query, query type, DNS host, etc.

### Protocol Intelligence: Protocol Center

### User Intelligence: Assets and Identities

- Assets are devices, such as routers, servers, etc.
  - Assets are identified by IP number, MAC address, or host name
- Identities are people
  - Identities are identified by user name, email address, etc.
- Both assets and identities are managed in ES with lookup tables
  - ES can show a meaningful name instead of an IP number or user ID –You can define watchlists for both assets and identities
- Asset and identity lookups are customized for your environment by an ES Admin
  - Discussed in detail later in the course

### Access User Activity from Action Menus

- After running a search, you can open the action menu for the user field and select User Activity
  - Opens the User Activity dashboard displaying only that user’s account activity

### User Intelligence > User Activity

Risk assigned by various correlation searches on user activity
Sorte d by size
Users accessing external sites that
Sorte
have been added to a watchlist
d by size
Users connecting from remote geographic locations
Users in an open incident in an external tracking system

### Access Anomalies

### Using the Access Anomaly Dashboard

- Searches for user access during the requested time period, defaults to 60 minutes
- Displays user access events with locations more than 500 miles from their previous access location
- The distance (miles) and speed (miles per hour) between locations yields an indicator of improbability for a user to actually log in from both locations
- Many access events spanning a short time from many geographically remote locations is suspicious

### Web Intelligence

The Web Intelligence menu contains analysis dashboards that are useful for inspecting various aspects of your website network activity

|HTTP Category|Explore the types of websites being accessed through your network
|:--:|:--:|
|HTTP User Agent|Examine the web user agents being used on your network|
|New Domain|See what external domains are being accessed|
|URL Length|Examine request URLs for unusual contents|

### Web Intelligence: HTTP Category Analysis

- Overview of website use in your organization by category
- Categories are defined by Websense
  <www.websense.com/content/support/l ibrary/web/v76/siem/siem.pdf>

### Per Panel Filtering

- Analysis dashboards provide highlighting or filtering of items on dashboard views
  - After you have determined that an event is not a threat, you can add it to your whitelist to remove it from the dashboard view
  - If an event is determined to be a threat, use the Advanced Filter editor to add the item to your blacklist of known threats
- This feature is unavailable by default for ES Analysts
  - Can be enabled by an ES Admin
- For instance, for HTTP Category Analysis you may want to filter out expected categories and highlight unwanted categories

### Creating Per Panel Filters

Click one or more events in the dashboard
Click Per-panel Filter
Choose to either filter or highlight

### Filtered vs. Highlighted Events

- Filtered events are no longer displayed
- Highlighted events are marked with a red icon in the filter column and are displayed at the top of the list by default

### Managing Per Panel Filtering Lookups

- Configure > Data Enrichment > Lists and Lookups
- Or View/edit existing filters and select an analysis dashboard name to edit
- Edit or delete filters and Click Save

### Per-Panel Filter Audit

- Audit > Per-Panel Filter Audit
- Display data on per-panel filter usage
- See who creates per- panel filters, and what data is being filtered

### Security Domain Dashboards

- The Security Domains menu provides access to analytical dashboards organized by security domain
- Each set of dashboards contains tools to examine and search within a specific set of records
- These dashboards all display results from original source events contained in data models, not from notable events
- Use the dashboard requirements matrix or examine panel searches to determine specific data model dependencies

### Access Domain: Access Center

Filters allow you to focus on specific types of events
Panels show summaries of access notable events over time by action, app, etc.
Key indicators show overview of notable events and trends over previous 24 hours compared to 24-48 hours before

### Access Domain: Access Search

Search events specific to access domain

### Glass Tables

- Glass tables are custom views that can display security indicators as well as symbols, icons and graphics
- Glass tables are useful visualizations that can be used for status displays or to enhance understanding of security status
- Each glass table can display current or past information
- Glass tables are stored in the KV store
- Access can be controlled by roles
- Accessed on the Glass Tables menu

### Glass Table: Standard View

Text Custom icons
Select time
Contextual graphics
Metrics with threshold colors and trend metrics
Timelines
Gauge indicators
Toggle edit mode

### Glass Table: Edit Mode

Metrics
Tools
Settings for selected widget
Controls
Work area

### Glass Table Data Requirements

- Glass tables depend on either ad-hoc searches, or on key indicators, for metric values
  - Ad-hoc searches: SPL that results in a single value to display –Key Indicators are the values displayed at the top of many dashboards, such as Security Posture
- Glass tables are maintained in the KV Store:
  - Application: SplunkEnterpriseSecuritySuite
  - Collection: SplunkEnterpriseSecuritySuite_glasstables
- Glass tables can be exported and imported in Configure > Content Management

### Lab Exercise 3: Dashboard Data Sources

- Time: 15 minutes
- Tasks:
- Examine the dashboard panel data sources
- Examine per-panel filtering settings