# Using Splunk Enterprise Security

## 5. Risk Analysis

### Objectives

- Understand risk analysis concepts
- Use the Risk Analysis dashboard to monitor activity related to risk
- Manage risk scores for objects or users

### Security Intelligence: Risk Analysis

- Risk Analysis is a dashboard on the `Security Intelligence` menu
- Along with the other dashboards on this menu, `Risk Analysis` is a tool that enables you to examine events in your ES indexes in detail
- These tools give security practitioners an ability to proactively analyze their environment to detect potential problems before they become issues

### Risk Analysis

- Correlation searches can add a numeric risk value to objects
  - Objects can be systems or users
  - Risk can be increased by any event that occurs to an object
  - Admins can configure the amount of risk assigned:
    - Per object
    - Per event
    - Per correlation search
- How is risk is different from priority, severity, or urgency?
  - You can see cumulative risk caused by multiple events over time
  - You can fine-tune the way you interpret threats or vulnerabilities

### Risk Analysis Example

A priority firewall server is attacked periodically over time

1. A few attacks are expected, but as they accumulate over weeks, the risk score for that server will increase
2. If other low priority events are also accumulating for that server, like minor vulnerabilities and low-grade anomalous network activity, they will also contribute to the risk score for the server
3. If the risk for that server increases more than other servers due to this continuing activity, you can be alerted and investigate Difficult to detect without this cumulative approach

### User and Asset Risk Activity Examples

- User risk
  - An employee who begins moving files to a local workstation and emailing attachments to external sites
  - A contractor who begins logging on from many different geographically remote systems throughout the organization
- Asset risk
  - A restricted system (like a point-of-sale station) begins running new processes - A server shows connections to known malicious sites on the internet
- Correlation searches can detect events like the above and add to the objects’ risk scores automatically

### Security Intelligence > Risk Analysis

Filters
Add an ad-hoc risk
Key Risk Indicators
Timeline shows most active risk-increasing events
Objects with most risk
Events affecting risk scores
Events causing the most risk

### Ad-hoc Risk Entry

- Useful if you want to change an object’s risk based on your own investigation
- The risk value you entered is added (or subtracted) to/from the object’s overall risk score

Add a score (positive or negative)
Add a description
Enter the risk object name (an asset or identity)
Select the object type
Click Save

### Lab Exercise 5: Risk Analysis

- Time: 20 minutes
- Scenario:
- Examine your environment for high risk assets or users
- Task:
- Investigate the sources of risk