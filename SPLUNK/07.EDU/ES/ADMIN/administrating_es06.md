# Administering Splunk Enterprise Security

## Module 6: Initial Configuration

### Objectives

- Configure user roles and capabilities
- Set general configuration options
- Add external integrations
- Configure local domain information
- Customize key indicators
- Customize navigation and view permissions
- Customize incident review settings

### Initial Configuration

- There are several general configuration tasks to perform before you begin to use ES:
  - User roles and capabilities   - General configuration options   - External integrations, such as UBA or domain lookup
  - Configure local domain information   - Configure key indicators   - Customize incident review settings

### General ES Configurations

- ES > Configure > General > General Settings
- Set or modify various ES parameters
- Example: indexed real time on or off, and changing the indexed real time delay <http://docs.splunk.com/Documentation/ES/latest/User/ManageSearches>

### ES Roles

- You can put users in the ES User or ES Analyst role as needed
- Users should not be added to the ES Admin role
- Add the ES Admin role as an inherited role to a regular Splunk role—such as Admin
  - All users in the Splunk Admin role also inherit ES Admin abilities docs.splunk.com/Documentation/ES/latest/Install/ConfigureUsersR oles

### Enabling Role Capabilities

- ES > Configure > General > Permissions
- Enable or disable capabilities for the ess_analyst or ess_user role
- Example: many sites want to allow analysts to manually create or suppress notable events

### Configuration Check

- Configuration > General > Configuration Check
- A list of automatic checks on ES configurations
- Generally leave alone, unless directed by Splunk Support

### Configuring Local and Cloud Domains

- Some correlation searches need to differentiate between your local domain vs. external domains
  - For instance, if you work at Acme Corp, you may have local domains ending in acme.com, acmecorp.com, etc.
- Also, there are external cloud domains you may use frequently that are not suspicious
  - External vendors for accounting, expenses, document sharing, etc.
- Your email system may use different email domains from your standard corporate domain
  - Due to acquisitions, mergers, etc.

### Editing Domain Tables

- Select Configure > Data Enrichment > Lists and Lookups
- Select the following lookups:
  - Corporate Web Domains: domains in your enterprise   - Corporate Email Domains: email domains
  - Cloud Domains: external vendor sites
- Right-click a row, select insert row below and add your domains
  - Right-click and delete any sample rows
- Click Save

### Configuring Domain Analysis

- The New Domain Analysis dashboard relies on domain name lookup information that is typically retrieved via a modular input from an external service such as domaintools.com
  - domaintools.com is a subscription service—you must have an account with user name/password
- Alternatively, you can:
  - Use the built-in domain name lookup modular input (not as robust)
  - Use a custom domain name lookup utility

### Configuring whois_domaintools

1. Add your domaintools.com credentials in the Credentials Manager
2. Configure the settings for the Network Query input
3. Enable whois checking
4. Check for events in the whois index <http://docs.splunk.com/Documentation/ES/latest/User/ThreatListActivitydashboard#Configure_the_external_API_for_WHOIS_data>

### Adding Credentials

- In ES, navigate to Configure > General > Credential Management and click New Credential
- Enter the domaintools.com credentials, select the SA-NetworkProtection app, and click Save

### Configuring the whois_domaintools Input

- Select Configure > Data Enrichment > Whois Management
- Edit the whois_domaintools entry:
  - API Host: domain name of your account’s server   - API User: your domaintools.com user name (password will be retrieved from credential manager automatically)
  - Leave other fields with default values unless you have a proxy or want to alter defaults for queue interval, etc
- Click Save

### Enabling the Domain Analysis Setting

- Modify the domain analysis setting
  - Navigate to Configure > General > General Settings   - Change the Domain Analysis setting to Enabled
- The whois system is now enabled
  - Domain name lookup happens when events with IP addresses are indexed
  - Domain info is stored in the whois index and used by the New Domain Analysis dashboard

### The whois_system Modular Input

- Alternative to whois_domaintools
- Not as robust—may not return as much detail about domain registrations as Domaintools
- To configure:
  - Enable the Domain Analysis setting as per preceding slide
  - Edit $SPLUNK_HOME/etc/apps/SA- NetworkProtection/local/inputs.conf as above
  - Restart server
- See the README files in SA-NetworkProtection for details

```properties
[whois://whois_system]
provider = WhoisSystem
query_interval = 15
queue_interval = 300
index = whois
source = Whois:System
sourcetype = Whois:System
```

### ES and User Behavior Analytics (UBA)

- Splunk User Behavior Analytics (UBA) is a separate solution that extends your ability to detect insider threats
- UBA can forward insider threat intelligence to ES
  - <http://docs.splunk.com/Documentation/TA-ueba/latest/User/UsetoIntegrate>
- ES can forward notable events to UBA for insider threat analysis
  - <http://docs.splunk.com/Documentation/ES/latest/User/SendUBASearchResults>

### Integrating Splunk UBA

- Sending ES notable events to UBA
  - Configure > UBA Setup   - Enter UBA server host and port and select protocol
- Send UBA insider threat intel to ES
  - This integration is on the UBA side

### Configuring Key Indicators

- Key indicators appear on many ES views
- By default, all key indicators have no threshold, so they will always display the current count in black
- You can configure thresholds for each key
  - If the count is above the threshold, the value is shown in red   - Green indicates a value below the threshold
- You can also re-order, delete, or add key indicators
- Hover your pointer over the key indicator row and the edit controls are displayed

### Editing Key Indicators: Order

Click to delete
Click to save changes
Click Edit to modify
Drag and drop to re-arrange

### Editing Key Indicators: Add New

Click to delete key indicator panel
Click to finish
Click to add a new key indicator

### Changing Key Indicator Thresholds

- You may want to use different threshold values
  - For instance, if you have a very large organization, you may expect a few minor security threats per day, and therefore would want to increase some of the thresholds above their defaults
- Edit the key indicator panel
- Enter a value in the Threshold field
- Save the new key indicator panel settings

### Editing Key Indicator Searches

- Configure > Content Management
- Select Type > Key Indicator
- Click a search name to edit indicator search definition
- Click Accelerate to configure an acceleration search schedule
- Select Create New Content > Key Indicator Search

### Adding or Editing a KI Search: 1

- Enter Name, App, Title and Subtitle Add a search that generates a current and delta value
- Drilldown URL can be a search, dashboard or view to open on click
- Add optional acceleration settings

KI Search Example

```sql
| tstats `summariesonly` count as current_count
from datamodel=Risk.All_Risk where All_Risk.risk_object_type="user" All_Risk.risk_score>60 earliest=-24h@h latest=+0s
| appendcols [|tstats `summariesonly` count as historical_count
from datamodel=Risk.All_Risk where All_Risk.risk_object_type="user" All_Risk.risk_score>60 earliest=-48h@h latest=-24h@h ] | `get_delta`
```

- The get_delta macro looks for fields current_count and historical_count and outputs delta
- The two counts should be based on the two previous 24 hour periods
- Use tstats `summariesonly` if possible for performance

### Adding or Editing a KI Search: 2

- The Value is the field containing the current value for the previous 24 hour period
- The Delta is the field containing the difference between the value for the previous 24 hour period and the preceding 24 hour period
- Rendering Options for threshold coloring, suffix notation, and inversion
- Click Save

### Incident Review KV Store

- All incident review status changes and comments are stored in the incident_review KV store collection
- Use the `incident_review` macro to retrieve information from this lookup
- Example: you are working on a new incident that is similar to one you worked on before and you want to search for comments related to the incident

```sql
|`incident_review` | search comment = "*...text...*"
```

### Incident Review KV Store Maintenance

- You may want to periodically clear out data from the incident review KV store:

```sql
| inputlookup incident_review_lookup
| eval age = (now()-time)/86400
| search age < 30 | fields - age
| outputlookup incident_review_lookup append=f
```

- You can also use the splunk clean command to completely clear out the incident review collection:

```bash
splunk clean kvstore -app SA-ThreatIntelligence -collection incident_review
```

- Note that Splunk must be running to use splunk clean kvstore

### Incident Review Status and Settings

- Configure > Incident Management > Incident Review Settings
  - Allows analysts to change notable urgency (default = yes)   - Requires comments when changing status (default = no)   - Sets minimum comment length
  - Customizes field display
- Configure > Incident Management > Notable Event Statuses
  - Changes the names of default statuses or adds new ones
  - Controls the permissions for statuses by role—for instance, restrict who can transition an incident to closed or resolved

### Modifying Urgency Calculation

- You can change the matrix that determines how correlation severity and asset/identity priority combine to set urgency
  - Select Configure > Data Enrichment > Lists and Lookups
  - Select the Urgency Levels lookup
  - Each row is one combination of priority and severity, with the resulting urgency shown in the right-most column
  - Modify as needed, and save

### Customizing Incident Review

- You can add or remove fields to an incident’s table or event attributes
  - Table = collapsed, one line per incident (default)   - Event = expanded details
- Example: display the src (IP) field at the right side of the table attributes
  - Navigate to Configure > Incident Management > Incident Review Settings
  - Under Table Attributes, click Insert below on the last row   - Enter src for the field to be displayed, and Source for the label
  - Click Done

### Example: Adding a Field

### Untriaged Incident Alert

- The correlation search Untriaged Notable Events can be configured and customized for your site as needed
- By default, it prepares a list of all notable events in new status or unassigned owner over the last 48 hours
- You can configure its adaptive response actions to send email to a group, run a script, or create a new notable event with a specific owner responsible to assign incidents to analysts

### ES Configuration Health Audit

- Audit > ES Configuration Health
- Checks ES configurations for settings that may conflict with ES defaults
- Useful to check ES status after initial configuration or an upgrade

### Controlling and Customizing Views

- Set permissions on dashboards and reports to control access
  - Only views the current user has access to are displayed in navigation
- Clone views and edit to create custom alternatives

### Customizing ES Menus

- The ES application enables you to customize the menu system to add, remove, or move menu items
- Navigate to Configure > General > Navigation to edit the navigation menus
  - Use drag and drop to move menus and menu items   - The checkmark icon in an item’s top-left corner makes it the default
  - Use the X icon in an item’s top-right corner to delete it
  - Click Save when finished

### Configuring Navigation

Make default
Add menu items
Edit
Add a new menu
Delete
Drag and drop items to re-sequence menus
i.e. from a new app or add-on
Save
Undo
Note
Add a New View adds a top-level menu item; to add an item to a menu, use the icon.
Adding links to filtered incident review results:
<http://docs.splunk.com/Documentation/ES/4.7.0/Admin/Customizemenubar#Add_a_link_to_a_filtered_view_of_Incident_Review>

### Adding a Menu Item

### Lab Exercise 6: Initial Configuration

- Time: 20 minutes
- Tasks:
- Configure key indicators
- Modify permissions:
  - Remove access to the Predictive Analytics dashboard for analysts
- Modify navigation:
  - Make the Security Posture the default view   - Make a menu item more accessible by moving it to the top row