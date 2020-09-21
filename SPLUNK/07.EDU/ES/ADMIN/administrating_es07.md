# Administering Splunk Enterprise Security

## Module 7: Validating ES Data

### Objectives

- Verify data is correctly configured for use in ES
- Validate normalization configurations
- Install additional add-ons

### ES Data Flow

- ES uses Splunk events for all correlation and analytical searches using the following process:
  1.Data is input from its source, indexed into events and a sourcetype is applied
  2. Tech add-ons apply normalization configurations based on the sourcetypes that assign the events to a data model
  3. The data model events are accelerated and stored into HPAS, with retention periods up to 1 year
  4. All ES correlation searches and dashboard searches are based on accelerated data model events

### Inputs

From Input to Dashboard
Input/Index Search Time
Data Acceleration
Index
Dashboards

### ES Data Models

- ES uses data models in the Common Information Model (CIM) <http://docs.splunk.com/Documentation/CIM/latest/User/Howtousethesereferencetables>
- Each data model defines a standard set of field names for events that share a logical context, such as:
  - Malware: anti-virus logs
  - Performance: OS metrics like CPU and memory usage
  - Authentication: log-on and authorization events   - Network Traffic: network activity
- Data models are conceptual maps, not containers

### Data Normalization

- Normalization converts non-standard field names and values into a uniform set of standardized fields within a data model
- Report designers can build report searches based on these standard terms without needing to know where the data originally came from
- Example: one sourcetype has events with an ACCESS field, containing numeric codes like 0 (access allowed) and 1 (access denied.) Another sourcetype has an Action field, with values “allowed” and “denied”. After normalization, both sourcetypes will have the Action field and use the same values, making it easier to build reports

### Normalization Process

- Normalization is a search-time process based on event sourcetypes and includes steps such as:
  - Adding tags, which control which events are displayed by which data models   - Changing field names and values to conform to data model specifications
- Add-ons automatically normalize most common sourcetypes
- You may have to adjust normalization rules, or create new normalization add-ons for custom data

### CIM Setup

- By default, all indexes are sources for CIM data model searches
- In some cases, there may be indexes that you want to exclude from data model usage
  - Example: personal information
- Navigate to Configure > CIM Setup; select each data model and select the indexes it should search; click Save
  - <http://docs.splunk.com/Documentation/CIM/latest/User/Install>

### ES Data Input Troubleshooting

- Ideally, after installing ES you’ll find that all the searches and dashboards work automatically
- However, if any events have non-standard sourcetypes, the normalization configurations in the tech add-ons won’t work
  - Example: automatic sourcetyping creates the wrong sourcetype name   - Fix: specify the correct sourcetype name in your configuration files
- If you have incoming data from a technology that requires a tech add-on that does not ship with ES, you’ll have to install it
- If you have custom data to use in ES, you might have to create your own TA

### Confirming Normalization

- Match your enabled TAs to CIM data models and verify the events are being added to the correct data models
  - Use the dashboard requirements matrix to determine which data models support each dashboard <http://docs.splunk.com/Documentation/ES/latest/User/DashboardMatrix>
  - Also useful: <http://blogs.splunk.com/2015/05/01/relating-add-ons-to-cim>
- If a sourcetype is not showing up in a data model:
  - Check the sourcetype
  - Make sure the TA is installed

### Steps for Initial Data Verification

1. Make a list of all sourcetypes required by ES - This will be dependent on the exact set of technologies and security products in use at your site
2. Map the sourcetypes to the TA that normalizes it
3. Confirm that the correct sourcetype name is being used - Verify against the TA documentation
4. Install additional TAs if needed
5. Verify that normalization is happening - Make sure the sourcetype is appearing in the correct data model and that all searches are executing as expected

### Map Sourcetypes to Tech Add-ons

1. Match each sourcetype to the tech add-on that will normalize it - Use add-on documentation to determine which sourcetypes are supported <http://docs.splunk.com/Documentation/AddOns>
2. Make sure the correct sourcetype name is being set - Change the sourcetype setting to the correct one, or - Edit the TA to use the local sourcetype name variant if necessary
3. Install (or create) any missing tech add-ons
4. Disable un-needed ES tech add-ons

### Finding More Add-ons

- Splunkbase has additional add-ons available for ES <http://splunkbase.splunk.com/apps/#/page/1/category/security_compliance>
- Add-ons must be CIM-compliant to be compatible with ES
- Search Splunkbase and/or the add-on documentation for the vendor or technology names related to the sourcetype you’re trying to normalize

### Examining Data Model Contents

- Use the datamodel command to examine the sourcetypes contained in the data model

```sql
|datamodel Network_Traffic All_Traffic search | stats count by sourcetype
```

- If the sourcetype is there, the events are being normalized
- If the sourcetype is missing:
  - Locate an add-on in Splunkbase that corresponds to the vendor or technology for the sourcetype, or
  - Build your own

### Problem: Missing Cisco ASA Events

1. As you audit the data in Splunk, you find that you want to use events from the Cisco router logs with sourcetypes cisco:asa and cisco:fwsm
2. Confirm that this data is present in Splunk indexes, but ES is not displaying it in any dashboards
3. The Network Traffic data model does not contain these sourcetypes - This is because the events are not being tagged with the network and communicate tags, and also the fields are not being aliased to the proper names required in the data model

### Solution: the Cisco Add-on

- In Splunkbase, the Cisco add-on is:
  - CIM-compliant   - Designed for use with ES
- Source types:
  - cisco:asa: authentication, session, and VPN
  - cisco:fwsm: authentication, session, and firewall
  - cisco:pix: authentication, session, and intrusion docs.splunk.com/Documentation/AddOns/latest/CiscoASA

### Installing Add-ons on the ES Search Head

- After installing a new add-on, restart the search head
  - ES automatically imports all add-ons during startup
- Add-on import is name-dependent
  - Only add-ons with TA- or Splunk_TA_ prefixes are automatically imported
  - See <http://docs.splunk.com/Documentation/ES/latest/Install/InstallTechnologyAdd-ons> for non-standard TA name procedures
- Check the TA README file
  - If it indicates it does index-time actions, re-generate and re-deploy the Splunk_TA_ForIndexers add-on
  - Carry out any additional TA setup in the README

### Content Profile Audit

- Audit > Content Profile Audit
- Maps data models to searches and dashboard panels
- Quick indication of missing data
- Usually means either a data source has not been configured or normalization is not complete

### Data Model Audit

- Audit > Data Model Audit
- Determine which data models are using the most storage or processor time
- Note that you can easily see each data model’s size, retention settings, and current refresh status

### Forwarder Audit

- Audit > Forwarder Audit
- Ensures hosts are properly forwarding data to Splunk
- Detects forwarders that have failed
- Only monitors hosts that are configured as expected hosts in ES

### Indexing Audit

- Audit > Indexing Audit
- Summary of events indexed per day (EPD)
- Time series shows trends
- Also summarized by index (main, threat_activity, etc.)

### Lab Exercise 7: Validating ES Data

- Time: 25 minutes
- Tasks:
- Audit sourcetypes
- Verify sourcetype normalization in data models
- Audit data model performance
- Install a new add-on
