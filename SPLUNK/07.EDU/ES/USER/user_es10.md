# Using Splunk Enterprise Security

## 10. Glass Tables

### Objectives

- Design glass tables to display security status information
- Use the glass table editor to create and edit glass tables
- Use key indicators and ad-hoc searches on glass tables
- Add glass table drilldown options

### Glass Tables

- Depict topology and data flow with metrics superimposed over each component
- Key indicators and ad-hoc values can be set to a time range
  - These values are called metrics on a glass table –
  - Metrics are displayed in visual widgets
- Use glass tables to:
  - Create security operations center displays
  - Show the status of critical metrics
  - Display key indicators in a variety of visual styles
  - Use custom icons and graphics to enhance the display

### Glass Table Management

- Navigate to the Glass Tables menu
- Select a glass table name to view
  - Analysts or administrators can edit, delete, or create new glass tables
- Each glass table can be private or shared

### Glass Table: Standard View

Text Custom icons
Select time
Contextual graphics
Metrics with threshold colors and trend metrics
Timelines
Gauge indicators
Toggle edit mode

### Glass Table: Edit Mode

Metrics
Tools
Settings for selected widget
Controls
Work area

### Designing Glass Tables

- Determine which views are needed at the site based on workflow, operations center display requirements, etc.
- Plan the visualization ahead of time
  - Use site graphics and icons, if possible, to enhance conceptual understanding
- Examine existing documentation for design elements
  - Flow charts, schematics, overview diagrams, etc.
- Plan the overall structure of the visualization and identify required metrics

### Creating a Glass Table: 1

Click
Create New Glass Table
Add a description (optional)
Click to keep the glass table private or share it with anyone with app access
Click Create Glass Table
Your new glass table now displays in the list on Saved Glass Tables
Click Glass Tables
Enter a title (will be visible on the glass table)
APPLICATIONS & SERVICES
Click its title to open it in the glass table editor

### Creating a Glass Table: 2

Show / hide side panels
Add custom graphics
Show / hide Add icons
side panels (conceptual graphics)
Selected widget properties can be modified

### Create a Glass Table: 3

Drag and drop Ad hoc searches or existing key indicators to add metrics to the canvas
Set labels, thresholds and drilldown as needed
Select a visualization type
Click Update

### Metrics

- Metrics are based on searches which generate a display value
  - Normally numeric but can display text
  - Can also display
    - Current vs. historic value
    - A sparkline
- Use an ad hoc search to display a new metric value
  - You write the search when you add the widget
- Use key indicators to display standardized values
  - Key indicators are categorized by type: Access, DNS, etc.

### Top Bar Controls

Select
Pan
Image
Edit title or delete glass table
Toggle View/Edit
Connector
Time Range
Box
Icons
Circle
Text\
Line
Revert to saved
Save Glass Table
Show/hide Grid
Delete all

### Configuration Bar: General

- Most widgets have controls for:
  - Position locking
  - Layer control (bring forward/back)
  - Object color (fore color)
  - Fill color
  - Deletion
    - Or, use the delete key

### Configuration Bar: Metrics

- Metric widgets have additional controls:
  - Width and height
  - Label and unit controls
  - Search information (disabled for key indicators)
  - Threshold on/off
  - Custom drilldown
  - Visualization type
- Use the Update button to refresh the glass table with any changes you make to a widget before selecting a different widget

### Configuration Bar: Ad-hoc Search

- Add an ad-hoc search widget to display any search result without a key indicator
- Drag an ad-hoc search widget onto the canvas
- Select search type: Ad-hoc or Data Model
- Enter search criteria
  - Use Run Search to test in a new window
  - Value displayed will be one of the result fields
  - Add the field name containing the value to be displayed in the Threshold Field
  - Click Update

### Metric Time Ranges

- Your search should set relative time ranges using the earliest and latest commands
  - Generally, set latest = + 0 s
  - Set earliest to determine the window size, such as - 60 m
- The search range is relative to the time the user selects
  - Given the example above, if the user selects 60 minutes ago, the metric displays results from – 60 m to – 120 m

### Widget Visualization Types

Single value
Single value delta
Gauge
Sparkline

### Thresholds

- Metric widgets can display colors to indicate severity
- The scale runs from Normal (green) to Critical (red), plus Info and Unknown
  - Info: a value that has no severity
  - Unknown: the value does not exist
- To display threshold colors, click On
- Use the Edit button to configure the threshold map

Unknown
Medium
Normal
Critical
High
Low
Info

### Threshold Mapping

- Add as many threshold levels as you need
  - Enter a value for each level
  - Use Info for values that don’t have a logical severity
- Levels can be in any order
  - Normal high
  - Normal low
  - Normal middle
- Fill in the upper and lower bound if applicable
- Click Done

Upper bound
Lower bound

### Visualization Requirements

- Single value visualization
  - Just needs a single output value
  - The value can be text—but then it can’t use thresholding
- Gauge
  - Requires an upper and lower bound setting
- Sparkline
  - Requires a search using timechart
  - Supports mouse-over for details
- Delta
  - Requires count and delta fields

### Glass Table Drilldown

- By default, clicking a metric widget opens a new window and runs the search for that metric
- Alternatively, you can enable a custom drilldown to open:
  - A saved glass table
  - A dashboard in Splunk
  - Any accessible URL

### Glass Table Storage

- Each glass table is managed in the KV store and is contained within the ES app context
- Each glass table can be either:
  - Private, only accessible by the owner
  - Shared with all users who have access to the Enterprise Security

### Lab Exercise 10: Creating Glass Tables

- Time: 45 minutes
- Task:
- Create a new glass table to monitor network status

### Wrap Up

- Understand how to use ES
- Define correlation search and notable event
- Using the security posture and incident review dashboards
- Using the event investigators for assets and identities
- Performing forensic investigation on current and past incidents
- Using adaptive response actions
- Using risk analysis to monitor risk in your security environment
- Analyzing network events for suspicious behavior
- Detecting insider threats
- Using the threat intelligence framework
- Using protocol intelligence to examine live network data
- Creating glass tables

### What’s Next?

- If you will be installing and/or administering ES, take Administering the Splunk App for Security
- For detailed course and certification information, go to: http://www.splunk.com/view/education/SP-CAAAAH9
- See extended ES use case examples at:
- <http://docs.splunk.com/Documentation/ES/latest/Usecases>
- If you have further questions, send an email to: certification@splunk.com