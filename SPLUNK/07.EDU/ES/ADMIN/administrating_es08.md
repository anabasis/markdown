# Administering Splunk Enterprise Security

## Module 8: Custom Add-ons

### Objectives

- Use custom data in ES
- Create an add-on for a custom sourcetype
- Describe add-on troubleshooting

### Custom Data Input

- If you have custom data sources you want ES to recognize, create an add-on to make your custom events CIM-compliant
- Your add-on should contain:
  - Data inputs (if required)
  - Field extractions (if required)
  - A tagged event type that maps your sourcetype to the appropriate CIM data model
  - Field aliases to map non-standard field names to CIM field names
  - Eval statements (calculated fields) or lookups to map non-standard field values to CIM field values

### Data Models and the CIM

- Your custom events are referenced by CIM data models
  - See the CIM documentation for a list of all the data models and their contents (docs.splunk.com/Documentation/CIM)
- Once you determine which data model should reference your events, plan which CIM fields relate to your custom fields
- Example:
  - You want the Network Traffic data model to return your events
  - At <http://docs.splunk.com/Documentation/CIM/latest/User/NetworkTraffic>, you see the list of required and optional fields for this data model
  - You make a mapping of your fields to CIM fields

### Normalization Strategy

- Not all of the source fields will match CIM fields
  - You can ignore the extra source fields, or omit them, as appropriate
- Not all of the CIM fields will be present in the source events
  - Use eval statements or regex-based field extractions to generate these fields with valid values if possible, or with placeholder values if no valid values can be determined
- Should you populate every CIM field in the target data model?
  - You need to at least populate the fields used by ES dashboards and correlation searches   - Mapping as many of the data model fields as possible will make your events more robust for future use in new views, searches or reports

### Planning Normalization Requirements

- Determine the dashboards that will display your events
- Use the dashboard requirements matrix to determine the data model(s) and field names the dashboard(s) require: <http://docs.splunk.com/Documentation/ES/latest/User/DashboardMatrix>

### Data Model Definitions

- The data model names in the dashboard requirements matrix are linked to the data model’s CIM documentation
- Use this documentation to determine the tags, field names and field values your events must use to be CIM-compliant

### Mapping Original Fields to CIM Fields

- Plan your normalization settings using a table
- List the required CIM-compliant field names
- Match them to corresponding original source fields
- Determine if normalization is required for each field’s name and value

|Original|CIM|Procedure|
|:--:|:--:|:--:|
|sender|src|alias|
|receiver|dst|alias|
|method|app|alias|
|user|user|none|
|account|unused|ignore|
|missing|signature|Use eval to create default value|
|SSID|unused|regex to mask all but last 4 digits|
|status|action|Use eval to translate source numeric codes to CIM terms|
|...|...||

### Splunk Add-on Builder

- Very fast way to build out the initial TA
- Use it to create your sourcetypes, extractions, and data model mapping
- Your TA can:
  - Automatically input data into Splunk   - Extract fields and map fields to the CIM

  - Create alert actions docs.splunk.com/Documentation/AddonBuilder

### Add-on Builder: Getting Started

- Install the Add-on Builder from Splunkbase
- Navigate to the builder’s home page
- Click New Add-on

### Add-on Builder: Create Add-on

- Enter an add-on name
  - This field becomes the name of the new app   - The builder adds a TA- prefix
- Add other optional project items
- Click Create
  - This creates a new add-on app on the local Splunk server
- Your add-on home page is displayed
- You may see a system message to restart Splunk—you can defer this until done with the new add-on

### Add-on Builder Home Page

1 Add sample
2 Map to CIM data
Note
The Add-on Builder can do a lot of things, but for CIM normalization you only need to add sample data and the CIM mapping function.

### Add-on Builder: Sample Data

- Select Add Sample Data
  - May need to reboot first if you just created the add-on
- If your sample data is already in Splunk, use Add from Splunk
  - Select from a sourcetype list and click Add
  - If you add from a file, you also specify parsing, time-stamping and sourcetype
- You can add multiple sourcetypes if desired

### CIM Mapping

- Select Map to CIM
- Click New CIM Mapping
- This opens the Define Event Type page

### Add-on Builder: Event Types

- Before you can add CIM mappings, you must identify your sourcetype(s) with an event type
  - This is used to generate the correct tags for your events to match the CIM target data model’s constraints
- On the Map to CIM page, each sourcetype you added in the sample data must map to one event type
  - More than one sourcetype can map to the same event type
- You can also add search criteria to filter out unwanted events from your CIM mapping
  - This excludes the events from the data model acceleration

### Add-on Builder: Creating the Event Type

Enter event type name
Select sourcetype
Search criteria

Output from test search
Save
Test search

### CIM Mapping(2)

Select one or more target data models
Select field alias or eval
Select a
5 source field 3
Note
The source event type or expression field can be an eval statement (to transform the source value to the CIM required format).
Click Ok
Click Done
Select a target field 4

### Selecting the CIM Target Data Model

Green checks
Trash can icon indicate fields
removes matching CIM
Select one or more
selected data field names
data models
model 1
Click Select

### Add-on Builder: Validate and Package (1)

- You can validate your add-on for best practices, CIM mapping, and field extractions
  - Any errors indicate a problem that should be corrected
  - Warnings are non-fatal but might need attention
  - If you select App pre-certification, a Splunkbase login is required
- Use Download Package to create an SPL package you can deploy to your production environment
  - The add-on is already active on the local system

### Add-on Builder: Validate and Package (2)

Click to download
Select source
Click to start field
validation

### Troubleshooting an Add-on

- Verify your add-on is imported to ES:

```sql
|rest /services/apps/local/SplunkEnterpriseSecuritySuite/import |fields import
```

- Your TA won’t be imported unless you use the correct naming convention or edit ES import patterns
  - You can modify the apps ES automatically imports
  - Configure > General > App Imports Update docs.splunk.com/Documentation/ES/latest/Install/InstallTechnologyAdd- ons

### Important Links

- Common Information Model : <http://docs.splunk.com/Documentation/Splunk/latest/Knowledge/UnderstandandusetheCommonInformationModel>
- Tags and aliases : <http://docs.splunk.com/Documentation/Splunk/latest/Knowledge/Abouttagsandaliases>
- Troubleshooting dashboards : <http://docs.splunk.com/Documentation/ES/latest/Admin/Troubleshootdashboards>
- Splunk Add-on Builder : <http://docs.splunk.com/Documentation/AddonBuilder/latest/UserGuide/Overview>

### Lab Exercise 8: Custom Add-on

- Time: 45 minutes
- Tasks:
- Create a plan for a new add-on for custom data
- Create the add-on with the Splunk Add-on Builder
