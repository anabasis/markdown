# Using Splunk Enterprise Security

## 2. Security Monitoring and Incident Investigation

### Objectives

- Use the Security Posture dashboard to monitor enterprise security status
- Use the Incident Review dashboard to investigate notable events
- Take ownership of an incident and move it through the investigation workflow
- Create notable events
- Suppress notable events

### Monitoring and Response

#### 1. ES continually runs correlation searches for known types of threats and vulnerabilities

- Over 60 correlation search types built in
- Plus you can create your own

#### 2. When a correlation search detects any indicators of compromise (IOC), ES creates an alert called a notable event or incident

- IOC is an industry term, while notable event & incident are ES terms

#### 3. ES enables you to track, update, and resolve incidents

- Security Posture dashboard provides a cross-domain SOC overview
- Incident Review dashboard to inspect and manage incidents

### The Security Posture Dashboard

- An overview of your Enterprise Security condition
- `Key indicators` at the top provide an at-a- glance view of notable event status over the last 24 hours
- Four panels below provide additional summary information categorized by urgency, time, and most common notable event types and sources

### Key Indicators

Only ES Admins can edit key indicators
Large number = total number of notable events in that category
Red = over threshold
Black = no threshold
Green = under threashold
Amount of change in last 24 hours
Trend indicator: red for increase, green for decrease under threshold

Current total count of events, trend of events, and total increase or decrease over the past 48 hours (from previous 24-hr period to last 24-hr period)

### Key Indicator Drilldown to Incident Review

From the Security Posture Dashboard, Click a key indicator total value
That key indicator’s Incident Review opens

### Security Posture Panels

### Notable Event Urgency

•Each notable event has an urgency field, ranging from informational to critical
•Urgency is a combination of two factors:
–Severity
ê Based on the raw event(s) ê Found by the correlation search –Priority
ê Assigned to the associated assets or identities—i.e., the server or user ê If more than one asset or identity is involved in a single notable event,
the one with the highest priority determines the urgency

### Urgency Table

![Urgency Table](./images/urgency_table.png)

### Drilldown Support

- Hover over an item to preview details about its underlying notable events
- Click an item in the security posture dashboard to open the related notable events in the incident review dashboard
- Once opened in the incident review dashboard, you can drill down into the details of each notable event, take ownership, and "work" the issue

### Incident Review Dashboard

- Urgency
- Filter options
- Timeline & job controls
- Associations
- Click for detail
- Investigation bar
- Add event(s) to an investigation
- Notable events
- Sortable column headers
- Action menu

![Incident Review Dashboard](./images/incident_review_dashboard.png)

### Incident Review Filter Fields

- `Status` : New, In Progress, Pending, Resolved, Closed
- Along with Owner, use to track status of an incident
- `Urgency` : info, low, medium, high, critical
- `Security Domain` : Access, Endpoint, Identity, Network, Threat, Audit
- `Owner` : The user assigned to investigate and resolve an incident
- `Correlation Search Name` : The title of a correlation search— wildcards (*) supported
- `Search` : Splunk search language expressions
- `Tag` : A list of tag names

### Using the Incident Review Dashboard

- Select one or more values per field
  - More than one value per field are `OR`ed together
- Urgency values can be toggled on and off
  - Grey values are “off” and will not be displayed
- If values are set for more than one field, the fields are `AND`ed together
- `Status, owner, domain` and `tag` support multiple `OR` values
  - The default `And` is ignored if other values are selected
- `Name` supports wildcards, `Search` supports full SPL

### Notable Event Details

Notable Event details dropdown
Notable Event Actions menu
Risk score
Field Actions menu
All fields for the notable event, with action menus for each field

```txt
Note
You can’t expand an event until the search is complete. Not all incidents have all the same detail items.
```

### Create a Short ID from Event Details

- Click Create a Short ID for ES to automatically generate a short ID that makes it easier to find and share.
- Then the Short ID replaces the Create Short ID link

### Create a Short ID from Actions Dropdown

- Creating a Short ID is also an option when you choose the Share Notable Event in the Actions dropdown
- Click the Bookmark button to copy the link for sharing or Click and drag the Bookmark button to your bookmarks bar to bookmark the link.

### Search for a Short ID or Investigation

```txt
Note
You can also click Associations > Type > Investigation to search by investigation names
```

```txt
Note
You can search for one or multiple Short IDs. Click in the text entry field to add more Short IDs.
```

Click Associations
Click Type
Click Submit
Click Short ID
Enter all or part of the Short ID (A dropdown appears and filters as you type) or Scroll to the Short ID

### Field Actions Menus

- Each notable event field has an action menu allowing you to
  - Investigate the asset, set tags, access other ES dashboards to view events or analyze the data in the field, and more
- Risk scores for hosts or users are displayed next to fields
  - Click a risk score to open the Risk Analysis dashboard for that asset or identity

```txt
Note
Action menus only display 10 items at a time. Scroll the menu to make sure you see all the items.
```

### Notable Event Actions Menu

- Each notable event has an Actions menu with options related to the event, such as:
  - Adding the event to an investigation
  - Suppressing the notable event
  - Sharing the notable event with others
  - Initiating adaptive response actions

- Share Notable Event creates a short ID
  - Displays a link to copy and share
  - Drag the bookmark icon to the bookmark it toolbar in your browser to bookmark it

### Incident Workflow: Concepts

Investigators are responsible for changing workflow status values as they work incidents

1. Assign an owner
2. Investigate
3. Implement corrective measures

- ES Admins can define and add new status values as necessary
  - New - not yet being worked
  - In progress - investigation underway
  - Pending - -various: work in progress, awaiting action, etc.
  - Resolved - fixed, awaiting verification
  - Closed - fix verified

### Incident Workflow: Procedures

Select one or more events
Click Edit Selected
Set Status, Urgency, Owner, and Comment
Click Save changes

As needed, add selected event(s) to an investigation. It will appear under Related Investigations in the event details view.

As needed, click an icon on the investigation bar view an investigation, add a new one, or perform a quick search

### Incident Review History

- Select View all review activity for this Notable Event to open a new search showing all review events for the current issue
- The `incident_review` macro can be used in your own custom searches and reports for incident status tracking

### Adaptive Response

- A notable event may contain adaptive responses the analyst can initiate
  - Actions menu: Select other adaptive responses to execute
  - Adaptive Responses: See a list of previously executed responses
  - Next Steps: Click a suggested response
- Depending on the type of notable event, different adaptive responses are available
  - Examples: ping host, change risk, run a script, nslookup, send to UBA, etc.

### Adaptive Response Actions

- You can choose from a list of actions to run
- This list is configured by your local ES administrators
- You may see different options depending on availability and permissions

### Adaptive Response Example: Ping

- As you investigate, you may need to see if the affected server is up
- Enter some or all of the action and select it from the list below. (The list filters as you type.)
- Fill in the Host Field with a static value or the name of a field in the event with the server IP or host name and click Run
- Click Ping to open its search and results
- Verify that your action is displayed in the notable event’s list of Adaptive Responses

### Adaptive Response Action: Threat Intel

Similarly, you can add threat artifacts in a threat collection
(needs to be configured by your admin first)

### Tagging Incidents

- You can use tags as a method to associate significant incidents
  - Example: you want to be able to quickly find all the incidents related to servers being used by project “whammo”
- Add a tag to each server (using the action menu for the dest, src or ip fields in this example) and then search for the tag name in the Tag filter field
- Now only notable events/incidents with this tag value are displayed

### Incident Review Audit

`Audit > Incident Review Audit`

- Provides an overview of incident review activity
- Shows how many incidents are being reviewed and by whom
- Displays incident aging over last 48 hours, broken down by status and by reviewer
- Statistics on triage time and closure time

### Creating and Suppressing Notable Events

- `Manual creation` : useful when you have source event data that has not (yet) been identified by ES as suspicious, and you want to create a notable event that will identify the issue and allow you to track it
- `Suppression` : useful if you are getting false positives from a host or a user, and you want to exclude future notable events from that host or user
- By default, ES Analysts do not have permission to perform these actions
  - An ES Admin must enable these capabilities for ES Analysts

### Creating Notable Events

- You can create ad-hoc notable events
  - For instance: if you find an event in Splunk that has not triggered a correlation search's parameters, but you feel it should be investigated
- Steps:
  1. Run a search on the source events
  2. Expand an event and select Event Actions
  3. Select Create notable event
  4. Enter the desired data for the notable event 5. Click Save

### Suppressing Notable Events

You may suppress notable events that are false positives, like a server temporarily misconfigured

- From Incident Review:
1. Expand the notable event's Actions menu
2. Select Suppress Notable Events
3. Set description and dates
4. Click Save

```txt
Note The end date is optional. If left blank, all future notable events from this server are suppressed.
```

### Managing Notable Event Suppressions

- After you create a new notable event suppression, you will see the list of suppressions as a confirmation
- You can access this list via __Configure > Incident Management > Notable Event Suppressions__
- Only ES admins can edit these suppressions by default

### Lab Exercise 2: Monitor and Investigate

- Time: 40 minutes
- Scenario: An expired user account has been detected attempting to log on to high priority resources

Tasks:

- Use incident review to investigate and take ownership
- Determine which server(s) are being threatened
- Use adaptive response to test the server
- Transition the notable events through the investigation workflow
- Suppress notable events