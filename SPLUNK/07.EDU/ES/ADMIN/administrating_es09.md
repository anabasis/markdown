# Administering Splunk Enterprise Security

## Module 9: Tuning Correlation Searches

### Objectives

- Describe correlation search operation
- Customize correlation searches
- Describe numeric vs. conceptual thresholds

### Plan, Install, Evaluate, Refine

- Start with a base level of enabled correlation searches
  - Security events in the enterprise   - Anomalous audit trails
- Adjust correlation search sensitivity
  - False positives: returning results when none are actually there
  - False negatives: returning no results when something is expected
- Revisit and adjust thresholds as needed
  - New security data is added to your ES install
  - The size of what is monitored shrinks or grows   - Decreased number of open issues (i.e. ES is working!!)

### ES Content

- Correlation searches are one type of ES Content
  - Correlation searches are stored as saved searches
  - Content in ES is any search or view that can be shared and used between multiple ES sites
- Examples:
  - Correlation searches
  - Key indicator searches   - Entity (asset or identity) swim lane searches
  - Views (dashboards and panels)
  - Saved searches

### ES Content Management

Configure > Content Management

### ES Content Management Functions

Enable, disable or export
Type select
Click title to edit
Filter
Create new searches
Enable or disable
Toggle between real-time and scheduled searches

### Enabling Correlation Searches

- All correlation searches are disabled by default
- Enable the correlation searches that make sense for your environment
- Consider:
  - Types of vulnerabilities or threats you have determined might exist   - Type of security operations you are focused on, i.e., malware, intrusion detection, audit, change monitoring, etc.
  - You may need to increase hardware specs if you have many correlation searches running
  - You can improve overall performance by making less critical correlation searches scheduled instead of real-time

### Scheduling a Correlation Search

- By default, all correlation searches run in indexed real-time mode
- If you change a search to scheduled, then by default it will execute every 5 minutes
- When editing the scheduled search, you can change the time range settings Start time, End time, and Cron Schedule

### Tuning Correlation Searches

- Threshold: the criteria that causes a correlation search to trigger
- Scheduling and throttling: how often to run the search and how often to generate notable events for the same type of incidents
- Adaptive Responses: list of actions to take, including possibly creating a notable event or setting risk
  - Notable event settings: severity, default owner, default status, etc.   - Risk: assigning, increasing, or decreasing the risk score for a given type of threat or incident
- Other adaptive responses can be sending email, running scripts, and more

### Correlation Thresholds

- Some correlation searches may generate more (or fewer) notable events than you want
- Examine the search string and look for comparison terms in search or where/xswhere functions and modify as appropriate for your environment
- Two types of thresholds:
  - Numeric   - Conceptual docs.splunk.com/Documentation/ES/latest/User/ConfigureCorrelationSe arches

### Numeric Thresholds

- Simple numeric comparisons
- Example: Excessive DNS Failures
- Note where command with numeric comparison
- Change the numeric value if you need to alter how frequently notable events are generated in your environment

### Conceptual Thresholds

- Use Extreme Search functions
- Example: Brute Force Access Behavior Detected
- Note xswhere command using “medium” as a threshold
- Change as appropriate <http://docs.splunk.com/Documentation/ES/latest/Admin/Extremesearchexample>

### Choosing Conceptual Thresholds

- In the previous example, the brute force correlation search generates a notable event if there are more than “medium” failures in an hour
- Use the xslistconcepts command to determine other conceptual terms for this threshold
- Extreme Search conceptual terms map to dynamic ranges of values that are automatically calculated and updated

### Determining Conceptual Ranges

Use xsdisplaycontext to display a graph of the values used in the concepts

### Correlation Search Throttling

- Once a correlation search has been triggered, you probably don’t want it to immediately re-trigger again for the same issue
- Most OOTB correlation searches throttle alerts to once a day
- If you want to modify this, change the Window duration
- In most cases, leave the Fields to group by alone

### Adaptive Response Actions

- When a correlation search detects an issue, it can initiate one or more adaptive response actions
- The most common response is to create a notable event
- Many also add risk to the objects associated with the issue
- Other responses can include sending email, running a script, stream capture, and sending data to UBA

### Customizing Notable Event Default Values

- Expand the notable adaptive response
- You can modify all the properties of the notable event that is created by a triggered correlation search—typically:
  - Severity   - Default Owner
  - Default Status

### Lab Exercise 9: Tuning Correlation Searches

- Time: 15 minutes
- Tasks:
- Identify numeric thresholds in a correlation search
- Identify conceptual thresholds in a correlation search