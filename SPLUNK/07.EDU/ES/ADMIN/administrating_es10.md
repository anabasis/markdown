# Administering Splunk Enterprise Security

## Module 10: Creating Correlation Searches

### Objectives

- Create a custom correlation search
- Manage adaptive responses
- Content import/export

### Creating a New Correlation Search

1. Determine a pattern of events that indicates an issue you want to respond to with a notable event or other action
2. Create a new correlation search in the UI using Configure > Content Management and select Create New Content > Correlation Search -  Use Guided Mode if desired
3. Configure scheduling and throttling
4. Configure the alert responses (notable event, etc.)

### Correlation Search Example: Risk

- For this example, create a new correlation search that generates a notable event once a day for any server with a risk score over 100
- On the Content Management page, select Create New Content > Correlation Search
- Enter the search name, application context, dispatch context, and description
- Select Edit search in guided mode to create the actual search

### Correlation Search Example: Risk (cont.)

- In the first step of the wizard, select the Risk data model and the All_Risk object
- If the Summaries-only option is selected, the correlation search will only search in accelerated data
  - Faster, but un-accelerated data is ignored
- Select the time range for the search
- Click Next

### Correlation Search Example: Risk (cont.1)

- In the next step, add filter expressions to limit the source events the correlation search retrieves
  - This could be used to focus on high priority assets or specific business units

### Correlation Search Example: Risk (cont.2)

- Next, add aggregate functions to perform operations like count, sum, or average on fields in the data model
- Also optionally add split-by conditions to aggregate values categorically
  - In this example, for risk, take the sum of all risk per host
- Click Next

### Correlation Search Example: Risk (cont.3)

- Define the logic to determine what condition will trigger a new notable event
- In this case, a notable event is generated if the risk score for any one host is greater than 100
- Click Next

### Correlation Search Example: Risk (cont.4)

- Finally, the search is parsed and displayed
- After verifying the test, select Done to save the correlation search criteria and continue configuring the rest of the correlation search fields
- If you edit the search string manually later, you will not be able to use guided mode to modify the search string

### Correlation Search Example: Risk (cont.5)

- Configure time range options
  - Earliest and Latest Time: time range
  - Cron schedule: how often to run the search, ‘*/5 \* \* \* \*’ for real-time
- Earliest and latest are relative to the scheduled start time

### Correlation Search Example: Risk (cont.6)

- Scheduling: real-time or continuous
  - Manages real-time scheduling   - Typically, leave the default of real-time
- Schedule Window: seconds (or “auto”)
  - Allow some flexibility in scheduling to improve scheduling efficiency
- Scheduling Priority: higher-priority searches will be selected first by scheduler if a conflict occurs
  - <http://docs.splunk.com/Documentation/Splunk/latest/Admin/Savedsearchesconf>

### Correlation Search Example: Risk (cont.7)

- Normally, a correlation search will trigger its actions (notable, etc.) if any results are found by the search
- You can use the Trigger Conditions to alter this default

### Correlation Search Example: Risk (cont.8)

- Throttling: You should throttle based on a field’s value
  - Example: no more than one notable event per host per day (86,400 seconds)
- More than one field can be selected
  - Throttling is based on all the field values ANDed together

### Correlation Search Example: Risk (cont.9)

1 Expand the list of adaptive responses
2 Select the Notable response

### Correlation Search Example: Risk (cont.10)

- Configure notable event field values
  - Title, description, security domain, severity
  - Default owner and status
  - Drill-down settings
- Embed field values in title, description, and drill-down fields using $fieldname$ format
- Description fields support URLs to external locations
  - Useful for best practices documents, investigation procedures, etc.

### Correlation Search Example: Risk (cont.11)

- You can control the “next steps” and “recommended actions” adaptive responses that appear in Incident Review
  - Next steps appear as links in the notable event details   - Recommended Actions appear in the notable event’s Actions menu

### Correlation Search Example: Risk (cont.12)

- Click Save to create the new correlation search
- Navigate back to the content management page
- Your new search will now display in the custom search list
- You can enable, disable, and change to scheduled or real-time as desired

### Adaptive Response Actions

- Besides (or instead of) creating notable events, adaptive response actions can automate other critical tasks
- One or more adaptive response actions can be added to each correlation search
  - The action will be executed if the correlation search finds any matches
- ES ships with a set of default adaptive responses
- You can also install additional adaptive responses, and control who can access each adaptive response <http://docs.splunk.com/Documentation/ES/latest/Admin/Setupadaptiveresponse>

### Default Adaptive Response Actions

Notable, Risk Create a notable event or add to an object’s risk score Send email Send email to one or more people Run script Execute an automated script. Example: when a correlation
search indicates a host is infected with malware, run a script to quarantine the target server Stream capture Automatically begin collecting detailed network information Nbstat, nslookup, ping Execute diagnostic command and attach output to the notable
event to assist in analysis Send to UBA If User Behavior Analytics is installed and integrated, send the
notable event to UBA for analysis Add Threat Intelligence
Create a threat intel artifact. Example: a new type of infection is discovered; add the characteristics of the infection (file name, source IP, code hash, etc.) to the threat intel database so that future similar attacks will be immediately alerted 

### Managing Adaptive Responses

Select Settings > Alert Actions to enable/disable, change permissions, or add new adaptive responses

### Adaptive Response Action Center

Audit > Adaptive Response Action Center

### Content Import/Export

- You can export any of the content types on the Content Management page by selecting them in the custom search list and choosing Export
- Fill in an app name, label, version and build number, and click Export
  - The content will be downloaded to your workstation as an .spl file   - It can then be installed as a new app into another ES search head

### Example: Content Export

Note
DA-ESS is a recommended prefix for content add- ons, but is not required.

### Content Export Best Practices

- The app name you create for your content export will be uploaded to the etc/apps directory of the receiving server
- Be careful when exporting updates to your content
  - Example: you export correlation1, naming it correlations.spl, and upload it to another ES server. Later you export correlation2, again using correlations.spl as the export name. When you upload correlations.spl to the second server, it overwrites the old version of correlations.spl, deleting correlation1
- Either use new app names each time (which could be difficult to manage) or make sure you always include all content (old and new) each time you export

### Lab Exercise 10: Correlation Searches

- Time: 20 minutes
- Task:
- Create a custom correlation search