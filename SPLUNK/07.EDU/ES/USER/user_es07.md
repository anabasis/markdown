# Using Splunk Enterprise Security

## 7. User Intelligence

### Objectives

- Understand and use user activity analysis
- Use access anomalies to detect suspicious access patterns
- Understand asset and identity concepts
- Use investigators to analyze events related to an asset or identity

### Insider Threats

- Some threats originate from inside your organization
> Social engineering can compromise employee accounts
> Disloyal employees or contractors may engage in unwanted activities

- ES contains many tools to investigate and analyze user activity:
> What user accounts are active and what are they doing?
> What equipment (servers, etc) are they accessing?
> Where are they logging on?
> How much risk has been accumulated by each user or device?

### Security Intelligence > User Intelligence

User intelligence tools provide the security practitioner with analytical tools to find potential internal threats

|||
|:--|:--|
|Asset Investigator|Examine a specific asset, such as a server or workstation, and compare events over time in parallel lanes showing different types of activity|
|Identity Investigator|Examine a specific identity and compare events over time in parallel lanes showing different types of activity|
|Access Anomalies|A survey of network activity by users, highlighting anomalous access (one user account being used multiple times)|
|User Activity|A survey of people and their actions, focused on watchlisted or high-risk users|

### Asset and Identity Investigators

- Both investigator dashboards allow you to enter an asset or identity name and a search range
- Both return a time-sequenced set of `swim lanes` showing activity for that asset or identity over time, comparing activity between:
  - Threats
  - IDS attacks
  - Authentication activity
  - Malware attacks
  - Notable events
  - Changes (such as firmware or software upgrades, etc.)

### Accessing the Investigators

- Asset fields identify a server: dest, src, ip, host
- Two ways to get to the Asset Investigator:
  - `Security Intelligence > User Intelligence >`
    -Enter an asset name or IP address, or
  - In a search result or details in Incident Review
    - Use an asset field’s action menu to select `Asset Investigator`
- Identity Investigator: as above, either:
  - Click `Security Intelligence > User Intelligence >`
  - Or use an identity field’s (user, src_user, etc.) action menu

### User Intelligence > Asset Investigator

Enter asset name here
Information about the asset
Details about the selected events in the swim lane
Choose time span for search
Swim lanes showing activity across areas
Selecting a bar (set of events) shows details at right
Area graph at bottom shows activity over time period

### User Intelligence > Identity Investigator

Enter identity name here
Same tools and functionality as the Asset Investigator

### How to Interpret Investigators

- Swim lanes make it easy to quickly pinpoint incidents that are simultaneous or sequential
  - Scan activity across various areas aligned in time sequence
  - Activities that are proximate in time may have a cause-effect relationship
- For example:
1. A server shows a burst of authentications at 1:15 am
2. At 1:17 am, a malware attack notable event is triggered for that server
- The asset investigator makes it apparent that there is a possible cause-effect relationship spanning across two (or more) swim lanes

### Pan and Zoom

- Drag the pan/zoom controls to:
1. Change the search time frame
2. Re-execute the search to show only activity in the selected range

### Configuring Swim Lanes

- Click Edit and select a collection of swim lanes
- Use the Custom collection to select specific swim lanes
- Customize swim lane colors
- All changes are saved as preferences for the current user
- Admins can add new swim lanes and can set overall defaults and permissions per  role as needed

You can also drag swim lanes up and down into the order you prefer

### User Intelligence > User Activity

Risk assigned by various correlation searches on user activity
Users accessing external sites that
Sorted by size
Sorted by size
have been added to a watchlist
Users connecting from remote geographic locations
Users in an open incident in an external tracking system

### User Activity to Identity Investigator

In User Activity, click a user. Identity Investigator opens
Click a risk score event in investigator; see

### Watchlisted Users and Sites

- Users and sites can be added to watchlists by your ES admins for various reasons:
  - Short term or new contractors
  - Under investigation

- Sites can also be watchlisted to track access
- The correlation search Watchlisted Event Observed creates a notable event if a watchlisted user or site is involved in an event
- Optional swim lanes on the identity and asset investigators also display watchlisted activity

### Search to Actions Menu to User Activity

Run a search
In the search results, click the Actions menu for the user field
The User Activity dashboard displays only that user’s activity

### Using the Access Anomaly Dashboard

Because access events spanning a short time from many geographically remote locations is suspicious, this dashboard:

- Searches for user access during the requested time period, defaults to 60 minutes
- Displays user access events with locations more than 500 miles from their previous access location
- The distance (miles) and speed (miles per hour) from one location to another yields an indicator of improbability for a user to actually log in from both locations

### Access Anomalies

### Splunk User Behavior Analytics Integration

- Splunk User Behavior Analysis (UBA) is a separate solution that extends your ability to detect insider threats
  - UBA can be integrated with ES
  - After integration, UBA can forward insider threat intelligence to ES, and ES can forward notable events to UBA

- ES can use insider threat intelligence to generate notable events for insider threat events
  - These can be viewed on the Incident Review dashboard
  - UBA threats also appear on the asset and identity investigators <http://docs.splunk.com/Documentation/ES/4.7.1/User/IdentityDomaindashboards>

### Identity > Asset Center

Security Domains > Identity > Asset Center

Distribution of assets by priority
Distribution of  assets by business unit
Distribution of assets by category

All asset lookup columns with dns, nt_host, ip, mac address, owner, priority, location, category, and PCI domain

### Identity Center

Security Domains > Identity > Identity Center
Identities by priority
Identities by business unit
Identities by category

Identity lookup columns: name, contact info, priority, business unit, watchlist (boolean), and start and end dates

### Session Center

Security Domains > Identity > Session Center

Over a given time period:

- Provides an overview of network sessions
- Used to correlate network activity to a user
- Session Center is where VPN and DHCP logs can be analyzed
- Helps identify anomalous activity such as excessively long sessions
- You can also open Session Center from Incident Review Dashboard under Action

### Associate Assets and Identities with UBA

Session Center’s UBA tab makes it easier to determine all session- related activities of potential threat actors and devices

- From a URL, determine a user
- From a user, determine an IP address and activity
- Requires UBA

### Related Correlation Searches

Correlation searches related to insider (user) threat:

- Abnormally High Number of Endpoint Changes By User
- Activity from Expired User Identity
- High Volume Email Activity to Non-corporate Domains by User
- New User Account Created On Multiple Hosts
- Web Uploads to Non-corporate Sites by Users
- Watchlisted Event Observed

### Lab Exercise 7: Internal Threats

- Time: 45 minutes
- Scenario:
- You are investigating potential internal threats
- Tasks:
- First, you examine the overall user activity and any anomalous access evidence
- Use the Identity Investigator to learn more about user activities
- Use the Asset Investigator to learn more about the systems being targeted and the pattern of access