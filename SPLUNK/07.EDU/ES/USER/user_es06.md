# Using Splunk Enterprise Security

## 6. Web Intelligence

### Objectives

- Use the web intelligence dashboards to analyze your network environment
- Filter and highlight events

### Security Intelligence: Web Intelligence

The Web Intelligence menu contains analytical dashboards that are useful for inspecting various aspects of your website network activity

|HTTP Category|Explore the types of websites being accessed through your network
|:--|:--|
|HTTP User Agent|Examine the web user agents being used on your network
|New Domain|See what external domains are being accessed|
|URL Length|Examine request URLs for unusual contents|

### Uses for Web Intelligence Dashboards

- Find URLs associated with unwanted behavior
  - HTTP Category Analysis
- Identify malicious activity in the form of long or malformed user agent strings
  - HTTP User Agent Analysis
- Detect botnet or trojan attacks by high counts of new domains
  - New Domain Analysis
- Look for embedded SQL, cross-site scripting, etc.
  - URL Length Analysis
  - <http://docs.splunk.com/Documentation/ES/latest/User/ThreatListActivitydashboard>

### Example: HTTP Category Analysis

- Overview of website use in your organization by category
- Categories are published and defined by Websense <http://www.websense.com/content/support/library/web/v76/siem/siem.pdf>

### Per Panel Filter

- Highlight or filter items out of dashboard views
- Example: for HTTP Category Analysis. filter out expected categories and highlight unwanted categories
- After it is determined that an event is not a threat
  - Can add it to your whitelist to remove from the dashboard view –If an event is determined to be a threat
  - Add it to your blacklist of known threats with the Advanced Filter editor
- This feature is unavailable by default for ES Analysts
  - Can be enabled by an ES Admin

### Creating Per Panel Filters

- Click one or more checkboxes in the dashboard
- Click Per-panel Filter
- Choose to either filter or highlight

### Filtered vs. Highlighted Events

- Filtered events are no longer displayed
- Highlighted events are:
  - Marked with a red icon in the filter column
  - Displayed at the top of the list by default

### Managing Per Panel Filtering Lookups

<https://docs.splunk.com/Documentation/ES/4.7.1/User/ThreatIntelligence>

### Unhighlighting an Event

If an event is already highlighted

1. Select it
2. Select Advanced Filter
3. Remove the highlight or change to filtering
4. Click Save

```txt
Note
Events can only be "unfiltered" directly from the lookup by removing the corresponding row. (Filtered events are not visible from the UI).
```

### Lab Exercise 6: Web Intelligence

- Time: 25 minutes
- Scenario:
  - You are using the HTTP User Agent dashboard and notice some unusual activity
- Task:
  - Use some of the other advanced threat dashboards to further investigate the situation