# Administering Splunk Enterprise Security

## Module 11: Lookups and Identity Management

### Objectives

- Identify ES-specific lookups
- Describe the interaction of lookups with correlation searches and other ES functions
- Configure asset and identity lookups

### ES Data Enrichment

- ES uses many lookup tables to store extended data for use on dashboards and by searches
  - Examples: assets, identities, incident review, threat intelligence, categories, etc.
- Some lookups are managed by the KV store
  - Examples: incident review, threat intel collections
- Others are stored in CSV files in the lookup directories of several supporting add-ons
- Use Configure > Data Enrichment > Lists and Lookups to manage ES lookups

### ES Lookup Management

Click to edit

### Editing and Viewing Lookup Files

Click Save when done
Right-click to add or remove columns or rows
Click a cell to edit contents

### Lookup Types: 1

|Assets|Information about the devices in your environment|
|:--:|:--|
|Identities|Information about the people in your environment|
|Threat intel|Locally produced and managed collections of threat intelligence|
|Interesting ports, processes, services|Descriptions of ports, processes, and services, including prohibited flags|
|Domains|Configure the local corporate domain; correlation searches watch for non-corporate email and web access|

### Lookup Types: 2

|Categories|Category definitions for assets and identities|
|Action history whitelist|Mask searches from the action history for investigation journals|
|Risk Object Types|Extend risk object type definitions|
|Security Domains|Edit or extend the list of security domains—access,
network, etc Urgency levels Edit or extend the urgency level titles
Expected Views Enable tracking of view use in ES—shows up in View
audit

### Lookups and Correlation Searches

|Lookup Supported Correlation Search Interesting Processes Prohibited Process Detected
Interesting Services Prohibited Service Detected
Prohibited Traffic Prohibited Port Activity Detected Local * Intel Threat List Activity Asset Expected Host Not Reporting
Should Timesync Host Not Syncing Identity Watchlisted Event Observed

### Asset and Identity Investigators

- Both investigator dashboards allow you to enter an asset or identity name and a search range
- Both return a time-sequenced set of swim lanes showing activity for that asset or identity over time, comparing activity between:
  - Threats
  - IDS attacks
  - Authentication activity
  - Malware attacks
  - Notable events
  - Changes (such as firmware or software upgrades, etc.)

### Accessing the Investigators

- Both the asset and identity investigators can be accessed on:
  - The User Intelligence menu, or
  - From field action menus in the Incident Review dashboard

### User Intelligence: Asset Investigator

Choose time span for search
Area graph at bottom shows activity over time period
Enter asset name here
Swim lanes showing activity across areas
Information about the asset
Selecting a bar (set of events) shows details at right
Details about the selected events in the swim lane

### User Intelligence: Identity Investigator

Same tools and functionality as the Asset Investigator
Enter identity name here

### How to Interpret Investigators

- The swim lanes visually show activity in various areas in time sequence, making it easy to see incidents that are simultaneous or sequential
- Activities that coincide in time may have a cause-effect relationship
- For example:
  - A server shows a burst of authentications at 1:15 am   - At 1:17 am, a malware attack notable event is triggered for that server
  - The asset investigator makes it apparent that there is a possible cause-effect relationship spanning across two (or more) swim lanes

### Pan and Zoom

Start
Dragging the pan/zoom controls changes the time frame for the search and re-executes the search, showing only the activity in the selected range
End

### Configuring Swimlanes

- Click Edit and select a collection of swimlanes
- Use the Custom collection to select specific swimlanes
- Customize swimlane colors
- All changes are saved as preferences for the current user
- ES Admins can add new swimlanes and can set overall defaults and  permissions per role as needed

You can also drag swimlanes up and down into the order you prefer

### Special Lookups: Assets and Identities

- Assets and identities are managed by the Identity Management modular input and Identity - XXX - Lookup Gen searches
- All assets and identities are checked for changes automatically every 300 seconds
- The searches create multiple expanded versions of the lookup tables for use during searches
- Identity management lookups are stored in SA-IdentityManagement/lookups
  - docs.splunk.com/Documentation/ES/latest/User/Identitymanagement

### Managing Identity and Asset Lists

- Configure > Data Enrichment > Identity Management
- static_assets and static_identities are the normal lookups
- demo_ lookups should be disabled after installation
- administrative_identities documents privileged accounts like root

### Uploading Assets and Identities

- Disable the demo asset and identity lookups
- Initially, load corporate asset and identity data using Splunk add- ons such as LDAP search or DB Connect
  - <http://docs.splunk.com/Documentation/ES/latest/Admin/Examplemethodsofaddingassetandidentitydata>
- Periodically re-run to keep assets and identities in ES updated
- You don’t need to include every piece of hardware or every person—focus on the ones with the most significance
- You don’t need to populate every column
- Asset and identity lookups scale up into the 10k-100k range

### Example: LDAP Search Identity Upload

```sql
| ldapsearch domain=<domain_name>
 search="(&(objectclass=user)(!(objectClass=computer)))"
| makemv userAccountControl
|search userAccountControl="NORMAL_ACCOUNT"
|eval suffix="" |eval priority="medium"
|eval category="normal" |eval watchlist="false"
|eval endDate=""
|table sAMAccountName,personalTitle,displayName,givenName,sn,suffix,
   mail,telephoneNu mber,mobile,manager,priority,department,category,
   watchlist,whenCreated,endDat e
|rename sAMAccountName as identity, personalTitle as prefix, displayName as nick,
   givenName as first, sn as last, mail as email, telephoneNumber as phone, mobile as phone2,
   manager as managedBy, department as bunit, whenCreated as startDate
|outputlookup my_identity_lookup
```

### Troubleshooting Assets and Identities

- Examine CSV files in $SPLUNK_HOME/etc/apps/SA-IdentityManagement/lookups
  - Verify that all CSV files are properly formatted
  - Verify that expanded versions have been created
- Check the log files

```sql
index=_internal sourcetype=python_modular_input category=asset OR category=identity
```

### More Troubleshooting

- To test an asset match:

```sql
| makeresults | eval src="1.2.3.4" | `get_asset(src)`
```

- To test an identity match:

```sql
| makeresults | eval user="hax0r" | `get_identity4events(user)`
```

### Asset Matching

- ES takes the value from an event’s src, dvc, or dest field and tries to match it to these columns in the asset lookup:

|Order|Column|Description|
|:--:|:--:|:--|
|1|ip|match the IP address or address range|
|2|mac|match on a Media Access Control address|
|3|dns|match on DNS name|
|4|nt_host|match on Windows Machine Name (a.k.a. NetBIOS name)|

- ES uses the above order to make its first match, then checks CIDR-based matches for IP addresses

### CIDR Asset Matching

- For ip and mac field ranges, if more than one range matches, ES matches on the smallest range
- Asset matching allows you to create large, catch-all categories on MAC or IP ranges, yet still single out smaller groups or individual IPs within the larger group

For example, our host=1.2.3.4, matches both the first and second IP ranges; however, it only matches on the second one since that’s the smaller range

### Other Interesting Asset Columns

<table>
  <tr>
    <td>Field</td>
    <td>Value</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>bunit</td><td>Text field</td><td>
    Arbitrary “business unit” for the asset; useful for filtering certain views.
  </tr><tr>
    <td>category</td><td>Text field</td><td>
    User-defined category for asset. The list of options for this field is retrieved from a separate lookup list.
  </tr><tr>
    <td>is_expected</td><td>True/ False</td><td>
    If true, ES expects this asset to always be running and sending data to Splunk. If it stops, a notable event is created. Defaults to False.
  </tr><tr>
    <td>should_timesync, should_update</td><td>True/ False</td><td>
    Works the same as is_expected, except it alerts on failure to time sync or failure to update.
    </td>
  </tr>
</table>

### Identity Matching

- ES takes a value from an event’s user, src_user, email, src_email field and tries to match it in the identities lookup:

|Order|Column|Description|
|:--:|:--:|:--|
|1|identity|Exact match on any one of a list of pipe-separated list of user names in identity column
|2|Email|Exact match|
|3|Email|First part of email, ie “htrapper” of “htrapper@acmetech.com”|
|4|Any|Disabled by default—see “conventions” in identityLookup.conf.spec|

- There is also a configuration UI to specify which of these to use

### Identity Lookup Configuration

Configure > Data Enrichment > Identity Lookup Configuration
<http://docs.splunk.com/Documentation/ES/latest/User/Identitymanagement>

### Other Interesting Identity Fields

|Field|Description|
|:--:|:--|
|Identity(key)|Pipe delimited list of usernames representing the identity|
|Prefix|Prefix of the identity (for example, Dr.)|
|Nick|Nickname of the identity|
|First|First name of the identity|
|Middle|Middle name or initial of the identity|
|Last|Last name of the identity|
|Suffix|Suffix of the identity (for example, Jr.)|
|Email|Email address of the identity|
|Phone|Phone number of the identity|
|watchlist|True/false|

### Watchlisting Assets and Identities

- You can add identities and assets to a watchlist, which then highlights them in various dashboards and searches
  - Example: watchlisted users are shown on the User Activity dashboard
- Watchlist users by setting the watchlist to true in static_identities.csv
- Add assets to watchlists by:
  - Configure > General > General Settings
  - Edit Website Watchlist Search and add asset IP or DNS
- Watchlisted assets or identities also trigger the Watchlisted Event Observed correlation search, if enabled

### Asset Center

- Security Domains > Identity > Asset Center
- Overview of assets
- Visualizations by priority, business unit, and category
- Table at bottom shows all asset lookup columns

All assets with dns, nt_host, ip, mac address, owner, priority, location, category, and PCI domain
Distribution of assets by priority
Distribution of assets by business unit
Distribution of assets by category

### Identity Center

Security Domains Identity > Identity Center >

- Overview of identities
- Bottom table shows all identity lookup columns

Identity information is shown with name, contact info, priority, business unit, watchlist (boolean: true or false), and start and end dates

Identities by priority
Identities by business unit
Identities by category

### Asset/Identity Correlation Performance

- You can disable asset/identity correlation by sourcetype
- This can be useful for performance improvement
- If a given sourcetype does not contain “interesting” assets or identities, there is no need for asset/identity correlation
- Configure > Data Enrichment > Identity Correlation

### Lab Exercise 11: Lookups

- Time: 25 minutes
- Task:
- Edit ES lookup tables
