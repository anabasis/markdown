# Using Splunk Enterprise Security

## 3. Investigations

### Objectives

- Use investigations to manage incident response activity
- Use the investigation timeline
- Add items to investigations

### Investigations

- An investigation is a collection of activities and notes related to work done on a specific issue, such as a breach or other incident
- Investigations are organized chronologically into timelines
- Investigations can be managed by one or more analysts
- Use investigations to:
  - Visualize progress
  - Document work
  - Share information

### Starting an Investigation

- You can create a new investigation:
  - From the Incident Review dashboard’s Actions menu
  - On the Investigations dashboard
  - From any ES dashboard using the Investigation Bar at the bottom of the ES window
  - When searching raw events, from the Event Actions menu
- By default, only ess_admin and ess_analyst users can create investigations
- Each investigation has one owner and can have any number of additional collaborators
- Only owners and collaborators can modify the investigation

### Scenario: Data Exfiltration

- Customers have reported unauthorized use of their account numbers (from your store)
- Start an investigation and begin researching the issue
1. Create a note describing the situation and how you were notified
2. Examine notable events related to the payment processing system and add them to the investigation
3. Run ad-hoc searches and add the results to the timeline
4. Add notes  periodically to explain why you ran searches and what you found in the results
5. Add notes detailing actions taken to mitigate the breach and close the vulnerability

### Investigations Dashboard

Lists all investigations
Filter by time or text
Add investigations
Click an investigation to view its related entries (default view is Timeline)
To edit (delete) an investigation: 1. 1 Click a box or boxes 2
Click the Edit Selection button

### Timeline View

Change view Filter
Scroll left (newer)
Add collaborator
Scroll right (older)

### Rename Investigation Entry

- Modify time as needed. Default = now
- Enter new title
- Add attachments (optional)

### Investigation Bar and Inline Timeline View

- Navigate quickly to investigations from Incident Review
- View and edit the investigation timeline

Timeline
Timeline Zoom
Jump to start
Edit investigation
name Notable and
events description
Create a
 Sortable column headers new
Action
Select investigation
investigation
History Notes
Quick Search
Toggle investigation timeline

### Quick Search

Click quick search icon
Enter search criteria
Determine whether the results are useful to the investigation
Add search to investigation

### Adding Events

Add notable events from incident review
or
Add source events from search result window

### Investigation List View

Delete items
Choose which items to view
Click to edit investigation name
Add / remove collaborators
Edit item names

### Investigation List View (cont.)

Delete items
Click to edit investigation name
Choose which items to view
Add / remove collaborators
This investigation’s notes, notable events, and adaptive response actions that have executed
Edit item names

### Adding Collaborators

- Only the owner and collaborators can work on an investigation
- Owners can’t be deleted

- Hover over any collaborator name
- Click to remove a collaborator change write to view permissions
- Click to add a collaborator
- Search and / or click to add a collaborator

### Adding Items to the Investigation

It’s important to add items to investigations to document the purpose of the steps you’ve taken to research the issue

- Note
  - Add textual information relevant to the investigation
    - Why you ran each search
    - What the results mean for your investigation
- Action History
  - A reverse chronological list of all of your activities in ES: searches run, dashboards used, etc.
  - Add pertinent entries from your Action History to to an investigation and supplement them with notes

### Example: Adding a Note

Modify time as needed Default = now
Add attachments
Enter comments
Click to add a note

Note
Attachments can be in text or binary format. 4 MB max per file (as attachments to notes) and are stored in KV store.

### Example: Adding Action History Items

Modify time as needed.
Select items
Filter as needed

### Lab Exercise 3: Using Investigation Timelines

- Time: 40 minutes
- Task:
- Start an investigation and add items to it as your analysis proceeds
- Create a manual notable event
