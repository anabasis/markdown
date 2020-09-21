# Using Splunk Enterprise Security

## Appendix: Reports, Dashboards, Data Models and Predictive Analytics

### Objectives

- Use and customize ES reports
- Use and customize ES dashboards
- Understand ES data models
- Use ES predictive analytics to detect unexpected trends

### ES Reports

- Select Search > Reports
- Over 200 reports in more than 20 categories
- Execute any report by selecting its name
- Select Edit to open in search, modify, and save as a new report
- Use Share, Print or Export as appropriate Export

Share
Print

### ES Datasets

- Access the data models ES uses via the Search > Datasets menu option
- Select Pivot to quickly build new reports
  - These reports can then be enhanced with charts and saved for future use

### Common Information Model

- The Common Information Model (CIM) is a library of data models
- The CIM is built into the data models that are included with ES
  - Many of the data models used by ES are actually configured in the CIM app
- One important service provided by the CIM is normalization
- Different data sources might use different names for one logical field name
  - Example: “Sev”, “Severity”, “SevCode”, etc all map to the logical field name “Severity” <http://docs.splunk.com/Documentation/CIM/latest/User/Overview>

### Predictive Analytics

- Search > Predictive Analytics
- Displays the actual values over time (blue line) vs. predicted values (yellow line) and the possible range of values for the selected object over time (shaded area), within two standard deviations
- Outlier data (outside 2 standard deviations) displays in the table below
- Use fields at top of page to select data model, object, and optional settings range

### Using Predictive Analytics

- Scenario: There seems to be a lot of failed login attempts—is it outside of what you should expect?
  - In Predictive Analytics, select the Authentication data model and Failed Authentication data set
  - Select the Count function and leave Attribute set to None –Select the appropriate time range for your investigation and run the search
- Examine the time graph for time segments where the blue line is outside the shaded area
  - The Outliers table identifies these segments
- The outliers represent anomalous trends

### Predictive Analytics Correlation Searches

- Only ES Admins can use this command
- After configuring the Predictive Analytics dashboard, it can be saved as a new correlation search
- Select Save as Correlation Search...
- Set the fields in the dialog as appropriate
- Click Save
- Now ES will automatically search for outliers from this predictive analysis and create notable events when they occur in the future to alert you to investigate

### How to Set Advanced Predict Options

- Select the Advanced... link
  - You can change the timespan, algorithm, future timespan, holdback, and lower and higher confidence intervals –These normally can be left defaulted
- To learn more, see: <http://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Predict>