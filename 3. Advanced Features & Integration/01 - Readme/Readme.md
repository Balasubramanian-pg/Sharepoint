# Week 3: Advanced Features & Integration - Detailed Documentation

## Day 15: Power Automate Integration (Workflows & Automation)

### Understanding Power Automate

**What is Power Automate?**
- Microsoft's workflow automation platform
- Formerly known as Microsoft Flow
- Cloud-based automation service
- Connects apps and services
- No-code/low-code solution
- Part of Microsoft Power Platform

**Power Automate Capabilities:**
- Automate repetitive tasks
- Connect to 500+ services
- Approval workflows
- Data collection and processing
- Notifications and alerts
- Document generation
- Scheduled tasks
- Business process automation

**Power Automate vs SharePoint Workflows:**
- **Power Automate:** Modern, cloud-based, actively developed
- **SharePoint Designer Workflows:** Legacy, limited support, being retired
- **Power Automate is the future** - focus your learning here

### Types of Flows

**1. Automated Cloud Flows**
- Triggered by events
- Examples:
  - When item is created
  - When file is modified
  - When email arrives
  - When form is submitted
- Most common for SharePoint

**2. Instant Cloud Flows (Button Flows)**
- Manually triggered
- Button in SharePoint
- Mobile app button
- Examples:
  - Request approval
  - Send notification
  - Copy to another library

**3. Scheduled Cloud Flows**
- Run on schedule
- Daily, weekly, monthly
- Specific times
- Examples:
  - Daily status reports
  - Weekly reminders
  - Monthly archive jobs

**4. Desktop Flows (RPA)**
- Robotic Process Automation
- Automate desktop applications
- Record and playback actions
- Advanced use cases

### Accessing Power Automate from SharePoint

**Method 1: From List/Library**
1. Select item or document
2. Click "Automate" in toolbar
3. Choose option:
   - Create a flow
   - See your flows
   - Configure flows

**Method 2: From Power Automate Site**
1. Navigate to https://make.powerautomate.com
2. Click "Create"
3. Choose template or start from blank
4. Connect to SharePoint

**Method 3: From Item Menu**
1. Click "..." on item
2. Select "Power Automate"
3. Choose or create flow

### Creating Your First Flow - Notification Example

**Scenario: Email notification when new item added**

**Step-by-Step:**

1. **Start Flow Creation:**
   - Go to SharePoint list
   - Click "Automate" > "Power Automate" > "Create a flow"
   - Or choose "See your flow templates"

2. **Choose Trigger:**
   - Select "When an item is created"
   - Or start from blank and search for trigger
   - Click "Create"

3. **Configure Trigger:**
   - **Site Address:** Select your SharePoint site (dropdown)
   - **List Name:** Select your list
   - Click "New step"

4. **Add Action - Send Email:**
   - Search for "send email"
   - Choose "Send an email (V2)" - Office 365 Outlook
   - Configure:
     - **To:** Enter email address or use dynamic content
     - **Subject:** "New Item Created: " + Title (use dynamic content)
     - **Body:** Click inside, then add dynamic content:
       - Title
       - Created By
       - Any other fields
   - Use formatting options (bold, bullets, etc.)

5. **Test Flow:**
   - Click "Save"
   - Go to your SharePoint list
   - Create new item
   - Check email inbox
   - Verify notification received

6. **Review Flow Run:**
   - Return to Power Automate
   - Click "My flows"
   - Find your flow
   - View run history
   - Check for success/errors

### Common SharePoint Triggers

**When an item is created**
- New list item added
- Use for: Welcome emails, task assignment, logging

**When an item is modified**
- Item properties changed
- Use for: Update notifications, status tracking, sync to other systems

**When an item is deleted**
- Item removed from list
- Use for: Backup, audit logging, cleanup tasks

**When a file is created (properties only)**
- New file uploaded to library
- Use for: Document routing, approval initiation, metadata enforcement

**When a file is created or modified (properties only)**
- File added or updated
- Use for: Version notifications, compliance checks

**For a selected item/file**
- Manual trigger on specific item
- Use for: On-demand approvals, ad-hoc tasks

### Common SharePoint Actions

**Get items**
- Retrieve multiple items from list
- Filter, sort, limit results
- Use for: Gathering data, reporting

**Get item**
- Retrieve single item by ID
- Use for: Getting details of specific item

**Create item**
- Add new item to list
- Specify column values
- Use for: Creating related records, logging

**Update item**
- Modify existing item
- Change column values
- Use for: Status updates, data enrichment

**Delete item**
- Remove item from list
- Use for: Cleanup, archiving workflows

**Send an HTTP request to SharePoint**
- Advanced REST API calls
- Custom operations
- Use for: Complex scenarios not covered by standard actions

**Create file**
- Add file to library
- Specify content
- Use for: Generating documents, saving attachments

**Copy file**
- Duplicate file to another location
- Use for: Backups, distribution

**Move file**
- Relocate file
- Use for: Organizing, archiving

**Get file content**
- Retrieve file binary content
- Use for: Processing, converting, sending

**Get file metadata**
- Retrieve file properties
- Use for: Reading metadata without downloading

### Dynamic Content and Expressions

**Dynamic Content:**
- Data from previous steps
- Automatically available
- Click to insert
- Examples:
  - Item Title
  - Created By email
  - Modified date
  - Custom columns

**Using Dynamic Content:**
1. Click in text field
2. Lightning bolt icon appears
3. Click to open dynamic content panel
4. Browse or search for field
5. Click to insert

**Expressions:**
- Formulas and functions
- Similar to Excel formulas
- Transform and manipulate data

**Common Expressions:**

```
formatDateTime(utcNow(), 'yyyy-MM-dd')
// Output: 2025-11-09

concat('Hello ', triggerOutputs()?['body/Title'])
// Output: Hello [Item Title]

if(equals(triggerOutputs()?['body/Status'], 'Approved'), 'Yes', 'No')
// Output: Yes or No based on status

length(triggerOutputs()?['body/Description'])
// Output: Character count

toLower(triggerOutputs()?['body/Title'])
// Output: title in lowercase

substring(triggerOutputs()?['body/Title'], 0, 10)
// Output: First 10 characters
```

**Adding Expressions:**
1. Click in text field
2. Select "Expression" tab
3. Choose function or type formula
4. Click "OK"

### Building an Approval Flow

**Scenario: Document approval workflow**

**Flow Structure:**

1. **Trigger: When a file is created**
   - Site Address: Your site
   - Library Name: Documents
   - Folder: /Pending Approval

2. **Action: Start and wait for an approval**
   - Approval type: Approve/Reject - First to respond
   - Title: "Please approve: " + File name (dynamic content)
   - Assigned to: Manager email or use dynamic content
   - Details: Include file link, creator, date
   - Item link: Link to file
   - Item link description: "View Document"

3. **Condition: Check approval outcome**
   - Condition: Outcome equals Approve
   
   **If Yes (Approved):**
   - **Move file:** To /Approved folder
   - **Update file properties:** Status = Approved
   - **Send email:** To creator - "Your document was approved"
   
   **If No (Rejected):**
   - **Move file:** To /Rejected folder
   - **Update file properties:** Status = Rejected, add rejection comments
   - **Send email:** To creator - "Your document was rejected" with comments

4. **Save and Test:**
   - Upload document to Pending Approval folder
   - Check for approval request
   - Approve or reject
   - Verify file moved correctly
   - Check email notifications

### Advanced Flow Concepts

**Variables:**
- Store temporary data
- String, Integer, Boolean, Array, Object types
- Initialize at start, manipulate throughout flow

**Example:**
```
Initialize variable
- Name: Counter
- Type: Integer
- Value: 0

Increment variable
- Name: Counter
- Value: 1
```

**Loops:**

**Apply to each:**
- Iterate through array of items
- Perform actions on each
- Example: Process multiple SharePoint items

**Do until:**
- Repeat until condition met
- Set limit to prevent infinite loops
- Example: Keep checking until approval complete

**Parallel Branches:**
- Run multiple actions simultaneously
- Saves time
- Example: Send emails to multiple people at once

**Scope:**
- Group actions together
- Organize complex flows
- Error handling container
- Example: Group all "approved" actions

**Try-Catch-Finally:**
- Error handling
- Configure scope to run after failure
- Prevents flow failure
- Example: If email fails, log to SharePoint list

### Conditions and Logic

**Condition Action:**
- If-then-else logic
- Compare values
- Multiple conditions (AND/OR)

**Example Conditions:**
```
If Title contains "Urgent"
  Then: Send high priority email
  Else: Send normal email

If Status equals "Complete" AND Priority equals "High"
  Then: Notify manager
  Else: Do nothing

If Due Date is less than today
  Then: Mark as overdue
```

**Switch Action:**
- Multiple branches based on single value
- Like switch statement in programming
- Cleaner than nested conditions

**Example:**
```
Switch on Status
  Case "New": Send to processing queue
  Case "In Progress": Update dashboard
  Case "Complete": Archive and notify
  Default: Log error
```

### Flow Scope and Permissions

**Flow Runs As:**
- Flow creator by default
- Uses creator's permissions
- Can access what creator can access

**Connections:**
- Each user's own connection
- Personal credentials
- Cannot share flows without connection setup

**Sharing Flows:**
1. Open flow
2. Click "Share" 
3. Add users or groups
4. Recipients must set up their own connections
5. Run-only users: Can trigger but not edit

**Service Account Flows:**
- Run with service account credentials
- Consistent permissions
- Best for organization-wide flows
- Requires proper licensing

### Monitoring and Troubleshooting

**Flow Run History:**
1. Open flow in Power Automate
2. View run history table
3. Click run to see details
4. See each action's inputs/outputs
5. Identify where failures occur

**Common Errors:**

**"Item not found"**
- Check list/library name
- Verify item ID
- Check permissions

**"Unauthorized"**
- Connection expired
- Insufficient permissions
- Re-authenticate connection

**"Timeout"**
- Flow too slow
- Reduce operations
- Optimize queries

**Flow Checker:**
- Click "Flow checker" icon
- Shows warnings and errors
- Recommends fixes
- Best practices

**Testing Best Practices:**
1. Test with sample data first
2. Use small datasets initially
3. Check all branches (if-then-else)
4. Verify error handling
5. Test permissions with different users
6. Monitor performance

### Flow Templates

**Using Templates:**
1. Power Automate > Templates
2. Search SharePoint templates
3. Popular templates:
   - Approval workflows
   - Notification flows
   - Data collection
   - Sync between lists
4. Click "Use template"
5. Configure connections
6. Customize as needed

**Creating Templates:**
- Save successful flows as templates
- Share with team
- Organizational templates
- Speeds up development

### Performance Optimization

**Best Practices:**

1. **Filter at Source:**
   - Use ODATA filters in Get Items
   - Don't retrieve all items then filter
   - Example: `Status eq 'Active'`

2. **Limit Results:**
   - Set "Top Count" in Get Items
   - Only retrieve what you need
   - Default 100, max 5000

3. **Avoid Loops When Possible:**
   - Use "Apply to each" sparingly
   - Better: Batch operations
   - Consider Power Apps for complex UI

4. **Use Compose Actions:**
   - Test expressions without making changes
   - Debug flows
   - Document logic

5. **Parallel Processing:**
   - Use parallel branches
   - Don't do sequential if not necessary
   - Saves time

### Hands-On Exercise Day 15

**Exercise 1: Simple Notification Flow**
1. Create a custom list "Project Updates"
2. Add columns: Title, Description, Priority (High/Medium/Low)
3. Create flow:
   - Trigger: When item is created
   - Action: Send email to yourself
   - Subject: "New Update: [Title]"
   - Body: Include Title, Description, Priority, Created By
4. Test by adding item
5. Verify email received

**Exercise 2: Conditional Notification**
Modify Exercise 1 flow:
1. Add Condition after trigger
2. Check if Priority equals "High"
3. If Yes: Send email with "URGENT" in subject
4. If No: Send normal email
5. Test with both high and low priority items

**Exercise 3: Document Approval Workflow**
1. Create library "Contract Drafts"
2. Create folders: Pending, Approved, Rejected
3. Create approval flow:
   - Trigger: File created in Pending folder
   - Start approval (assign to yourself for testing)
   - Condition on outcome
   - Move to appropriate folder
   - Send notification emails
4. Upload test document
5. Complete approval
6. Verify document moved

**Exercise 4: Copy Files Between Libraries**
1. Create two libraries: "Source" and "Archive"
2. Create flow:
   - Trigger: When file created in Source
   - Wait 1 minute (delay action)
   - Copy file to Archive
   - Add "_archived" to filename
3. Test flow
4. Check both libraries

**Exercise 5: Status Update Flow**
1. Use "Project Tracker" list from Week 1
2. Create flow:
   - Trigger: When item is modified
   - Condition: Check if Status changed to "Complete"
   - If yes:
     - Update "Completion Date" to today
     - Set "% Complete" to 100
     - Send congratulations email to Owner
3. Test by completing a project

**Exercise 6: Daily Summary Report**
1. Create scheduled flow
2. Run daily at 9 AM
3. Get items from Project Tracker where Status = "In Progress"
4. Create HTML table from results
5. Email table to yourself
6. Test flow manually (don't wait for schedule)

**Exercise 7: Troubleshooting Practice**
1. Create intentionally broken flow:
   - Wrong list name
   - Missing required field
   - Invalid email address
2. Run flow
3. Review error messages
4. Fix issues one by one
5. Document what you learned

---

## Day 16: Power Apps Integration

### Understanding Power Apps

**What is Power Apps?**
- Microsoft's low-code app development platform
- Create custom business apps
- No traditional coding required
- Part of Microsoft Power Platform
- Mobile and web-based apps
- Deep integration with SharePoint

**Power Apps Types:**

**1. Canvas Apps**
- Start with blank canvas
- Full design control
- Pixel-perfect layouts
- Like PowerPoint for apps
- Best for: Custom UI, mobile apps, specific workflows

**2. Model-Driven Apps**
- Start with data model
- Automatic UI generation
- Based on Dataverse
- Form-based interface
- Best for: Complex data, enterprise apps, CRM-style

**3. SharePoint Forms (Customized)**
- Customize existing SharePoint forms
- Replace default new/edit/view forms
- Simplified Power Apps experience
- Best for: Enhanced SharePoint forms

### Canvas Apps Basics

**Creating a Canvas App from SharePoint:**

**Method 1: From SharePoint List**
1. Open SharePoint list
2. Click "Integrate" > "Power Apps" > "Create an app"
3. Name your app
4. Click "Create"
5. Power Apps Studio opens
6. App automatically connected to list

**Method 2: From Power Apps**
1. Go to https://make.powerapps.com
2. Click "Create"
3. Select "Canvas app from blank" or "SharePoint"
4. Choose layout (Phone or Tablet)
5. Connect to SharePoint
6. Select list

**Auto-Generated App Structure:**
- **BrowseScreen1:** List all items (gallery)
- **DetailScreen1:** View single item details
- **EditScreen1:** Edit or create item
- Navigation between screens automatic

### Power Apps Studio Interface

**Key Components:**

**1. Canvas (Center):**
- Design surface
- Add and arrange controls
- WYSIWYG editor

**2. Screens Panel (Left):**
- Lists all screens
- Navigate between screens
- Add new screens

**3. Tree View:**
- Hierarchical view of controls
- Select and organize elements
- Shows control names

**4. Properties Panel (Right):**
- Configure selected control
- Common properties quick access
- Advanced properties available

**5. Formula Bar (Top):**
- Enter formulas and expressions
- Like Excel formula bar
- IntelliSense support

**6. Toolbar:**
- Insert controls
- Connect to data
- Preview app
- Save and publish

### Common Controls

**Display Controls:**

**Label:**
- Display text
- Static or dynamic
- Properties: Text, Font, Color, Alignment

**Image:**
- Display pictures
- From URL or upload
- Properties: Image source, Size, Transparency

**Icon:**
- Built-in icons library
- Visual indicators
- Properties: Icon type, Color, Size

**Gallery:**
- Display list of items
- Repeating template
- Vertical, horizontal, or flexible layout
- Properties: Items, Template, Layout

**Input Controls:**

**Text Input:**
- Single-line text entry
- Properties: Default, Hint text, Mode (single/multi-line)

**Dropdown:**
- Select from list
- Single selection
- Properties: Items, DefaultSelectedItems

**Combo Box:**
- Select from list with search
- Multi-select option
- Properties: Items, DefaultSelectedItems, SelectMultiple

**Date Picker:**
- Calendar selection
- Properties: DefaultDate, Format

**Toggle:**
- On/off switch
- Boolean value
- Properties: Default, True/False text

**Slider:**
- Select value from range
- Properties: Min, Max, Default

**Button Controls:**

**Button:**
- Click to trigger action
- Properties: Text, OnSelect formula

**Icon (as button):**
- Clickable icon
- OnSelect formula

**Action Controls:**

**Form:**
- Display/edit SharePoint item
- Connected to data source
- Modes: New, Edit, View
- Properties: Item, DataSource, OnSuccess

**Edit Form vs Display Form:**
- Edit Form: Modify data
- Display Form: Read-only view

### Connecting to SharePoint Data

**Adding Data Connection:**
1. Click "Data" in left panel
2. Click "Add data"
3. Search "SharePoint"
4. Select "SharePoint"
5. Enter site URL
6. Select list(s)
7. Click "Connect"

**Using SharePoint Data:**

**In Gallery:**
```
Items property = 'YourListName'
// Shows all items

Items = Filter('YourListName', Status = "Active")
// Shows filtered items

Items = Sort('YourListName', Title, Ascending)
// Shows sorted items
```

**In Form:**
```
DataSource = 'YourListName'
Item = Gallery1.Selected
// Shows selected item from gallery
```

**In Label:**
```
Text = Gallery1.Selected.Title
// Shows title of selected item
```

### Power Apps Formulas (Basics)

**Common Functions:**

**Filter:**
```
Filter('Project Tracker', Priority = "High")
// Returns high priority projects

Filter('Project Tracker', Status = "Active" && Owner.Email = User().Email)
// Returns my active projects
```

**Sort:**
```
Sort('Project Tracker', DueDate, Ascending)
// Sort by due date, earliest first

Sort('Project Tracker', Priority, Descending)
// Sort by priority
```

**Search:**
```
Search('Project Tracker', SearchInput.Text, "Title", "Description")
// Search in Title and Description columns
```

**Patch (Create/Update):**
```
// Create new item
Patch('Project Tracker',
  Defaults('Project Tracker'),
  {
    Title: TextInput1.Text,
    Priority: Dropdown1.Selected.Value,
    Status: "New"
  }
)

// Update existing item
Patch('Project Tracker',
  Gallery1.Selected,
  {Status: "Complete"}
)
```

**Navigate:**
```
Navigate(DetailScreen, ScreenTransition.Fade)
// Go to another screen with fade effect
```

**Notify:**
```
Notify("Item saved successfully!", NotificationType.Success)
// Show toast notification
```

**Context Variables:**
```
UpdateContext({varUserName: User().FullName})
// Set local variable

// Use variable
Label1.Text = varUserName
```

### Customizing Auto-Generated Apps

**Enhance Browse Screen:**

1. **Add Search:**
   - Insert > Input > Text input
   - Name: SearchBox
   - Set Gallery Items:
   ```
   Search('YourList', SearchBox.Text, "Title")
   ```

2. **Add Filter Dropdown:**
   - Insert dropdown
   - Set Items: `["All", "Active", "Complete"]`
   - Modify Gallery Items:
   ```
   If(Dropdown1.Selected.Value = "All",
      'YourList',
      Filter('YourList', Status = Dropdown1.Selected.Value)
   )
   ```

3. **Customize Gallery Template:**
   - Select gallery
   - Click "Edit" (pencil icon)
   - Add/remove/rearrange controls
   - Format text, colors, sizes

4. **Add Sort Buttons:**
   - Insert icon (up/down arrows)
   - OnSelect:
   ```
   UpdateContext({sortAscending: !sortAscending});
   ```
   - Gallery Items:
   ```
   Sort('YourList', Title, If(sortAscending, Ascending, Descending))
   ```

**Enhance Detail Screen:**

1. **Improve Layout:**
   - Rearrange fields logically
   - Group related information
   - Add section headers (labels)

2. **Add Calculated Fields:**
   - Insert label
   - Show computed values:
   ```
   "Days Remaining: " & DateDiff(Today(), Gallery1.Selected.DueDate, Days)
   ```

3. **Conditional Formatting:**
   - Change colors based on data:
   ```
   // Label color for status
   Color = If(Gallery1.Selected.Status = "Complete", 
             Color.Green, 
             If(Gallery1.Selected.Status = "Overdue",
                Color.Red,
                Color.Black))
   ```

4. **Add Action Buttons:**
   - Complete task button
   - Delete button
   - Email button

**Enhance Edit Screen:**

1. **Validation:**
   - Required field checks before save
   - Button OnSelect:
   ```
   If(IsBlank(TextInput1.Text),
      Notify("Title is required", NotificationType.Error),
      SubmitForm(Form1);
      Navigate(BrowseScreen, ScreenTransition.None)
   )
   ```

2. **Default Values:**
   - Set smart defaults:
   ```
   Default = User().Email  // For person field
   Default = Today()       // For date field
   Default = "New"         // For status field
   ```

3. **Conditional Fields:**
   - Show/hide based on other fields:
   ```
   Visible = Dropdown1.Selected.Value = "Other"
   ```

4. **Help Text:**
   - Add labels with instructions
   - Tooltip-style hints

### Building a Custom App from Scratch

**Example: Simple Task Tracker**

**Step 1: Create App and Connect Data**
1. Power Apps > Create > Canvas app from blank
2. Tablet layout
3. Name: "Task Tracker"
4. Add data: Connect to SharePoint "Tasks" list

**Step 2: Create Browse Screen**
1. Insert > Gallery > Vertical
2. Set Items: `Tasks`
3. Customize template:
   - Title: `ThisItem.Title`
   - Subtitle: `ThisItem.AssignedTo.DisplayName`
   - Body: `Text(ThisItem.DueDate, ShortDate)`
4. Add search box above gallery
5. Add "New Task" button
6. Add filter dropdown for status

**Step 3: Create Detail Screen**
1. Insert > New Screen > Blank
2. Add form: Insert > Forms > Display
3. Set DataSource: Tasks
4. Set Item: `Gallery1.Selected`
5. Arrange fields nicely
6. Add "Edit" and "Back" buttons
7. Edit button navigates to Edit screen
8. Back button navigates to Browse screen

**Step 4: Create Edit Screen**
1. Insert > New Screen > Blank
2. Add form: Insert > Forms > Edit
3. Set DataSource: Tasks
4. Set Item: `Gallery1.Selected`
5. Customize cards (fields)
6. Add "Save" and "Cancel" buttons
7. Save button: `SubmitForm(Form2)`
8. Form OnSuccess: Navigate back to Browse
9. Cancel button: Navigate back without saving

**Step 5: Add New Item Functionality**
1. On Browse screen, "New Task" button
2. OnSelect: 
```
NewForm(Form2);
Navigate(EditScreen, ScreenTransition.None)
```
3. Form will be in "New" mode
4. Save creates new item

**Step 6: Polish and Test**
1. Add app icon and splash screen
2. Set theme colors
3. Add logo
4. Test all scenarios:
   - Browse items
   - Search items
   - View details
   - Edit item
   - Create new item
   - Filter items
5. Fix any issues

**Step 7: Publish**
1. Click "File" > "Save"
2. Click "Publish"
3. Add version notes
4. Click "Publish this version"

### Customizing SharePoint Forms with Power Apps

**When to Customize Forms:**
- Need conditional logic
- Want better UX than default
- Require calculations
- Need data from multiple lists
- Want mobile-optimized forms

**Customizing a Form:**

1. **Start Customization:**
   - Open SharePoint list
   - Click "Integrate" > "Power Apps" > "Customize forms"
   - Power Apps Studio opens with form

2. **Form Structure:**
   - Single screen with form control
   - All list columns shown as cards
   - Connected to current list

3. **Customize Cards:**
   - Show/hide fields
   - Change field order
   - Modify labels
   - Add validation
   - Change control types

4. **Add Logic:**
   - Conditional visibility:
   ```
   // Hide field unless condition met
   Card_Priority.Visible = Dropdown_Status.Selected.Value = "In Progress"
   ```
   
   - Set field values based on others:
   ```
   // Auto-calculate end date
   DataCardValue_EndDate.Default = DateAdd(DataCardValue_StartDate.Selected, DataCardValue_Duration.Text, Days)
   ```

5. **Add Custom Controls:**
   - Insert labels for instructions
   - Add calculated fields
   - Insert images
   - Add buttons for actions

6. **Publish Form:**
   - Click "File" > "Publish to SharePoint"
   - Form replaces default SharePoint form
   - All users see customized version

**Reverting to Default:**
- List settings > Form settings
- Click "Use default form"

### Mobile Apps

**Optimizing for Mobile:**

1. **Phone Layout:**
   - Choose "Phone" layout when creating
   - Portrait orientation
   - Larger touch targets (40x40 pixels minimum)
   - Scrollable screens

2. **Responsive Design:**
   - Use Parent.Width and Parent.Height
   - Percentage-based sizing
   - Test on different devices

3. **Mobile-Friendly Controls:**
   - Dropdowns over text input (easier to select)
   - Toggle switches
   - Date pickers
   - Camera control for photos
   - Barcode scanner

4. **Performance:**
   - Limit data retrieved
   - Use delegation-friendly formulas
   - Lazy load images
   - Cache data when possible

**Installing Mobile App:**
1. Download Power Apps from app store
2. Sign in with Microsoft 365 account
3. Apps appear automatically
4. Pin favorites
5. Works offline with sync

### Integration Patterns

**Power Apps + Power Automate:**

1. **Trigger Flow from App:**
   - Button OnSelect:
   ```
   'YourFlowName'.Run(parameter1, parameter2)
   ```
   - Flow receives parameters
   - Returns response to app
   - Update UI based on response

2. **Example Use Cases:**
   - Submit for approval
   - Generate document
   - Send complex notifications
   - Call external APIs
   - Update multiple lists

**Power Apps + SharePoint:**

1. **Multiple List Integration:**
   ```
   // Lookup from related list
   LookUp(Customers, ID = Gallery1.Selected.CustomerID).CompanyName
   ```

2. **Create Related Items:**
   ```
   // Create item in related list
   Patch('Order Details',
     Defaults('Order Details'),
     {
       OrderID: Gallery1.Selected.ID,
       Product: Dropdown1.Selected.Value,
       Quantity: TextInput1.Text
     }
   )
   ```

### Sharing and Security

**Sharing Your App:**
1. Click "Share" button
2. Enter user names or groups
3. Choose permission level:
   - Co-owner: Can edit app
   - User: Can only run app
4. Share data connections (if not shared)
5. Send invitation

**Data Permissions:**
- App users need SharePoint permissions
- App inherits SharePoint security
- Cannot bypass list permissions
- Consider this in app design

**App Licensing:**
- Included with Microsoft 365 (standard connectors)
- Premium license for premium connectors
- Per-app or per-user plans
- Check organization's licensing

### Hands-On Exercise Day 16

**Exercise 1: Auto-Generated App**
1. Use "Project Tracker" list from Week 1
2. Create canvas app from SharePoint
3. Explore auto-generated app:
   - Browse screen
   - Detail screen
   - Edit screen
4. Add item using app
5. Edit item using app
6. Test on mobile (Power Apps mobile app)

**Exercise 2: Add Search and Filter**
Enhance the auto-generated app:
1. Add search box to browse screen
2. Connect search to gallery:
   ```
   Search('Project Tracker', SearchBox.Text, "Title", "Description")
   ```
3. Add status filter dropdown
4. Combine search and filter:
   ```
   Search(
     Filter('Project Tracker', 
       Dropdown1.Selected.Value = "All" || Status = Dropdown1.Selected.Value),
     SearchBox.Text,
     "Title"
   )
   ```
5. Test functionality

**Exercise 3: Conditional Formatting**
1. In gallery template, add color coding:
   - High priority items: Red background
   - Medium priority: Yellow background
   - Low priority: Green background
2. Add status icons
3. Show "OVERDUE" label if past due date
4. Publish and test

**Exercise 4: Custom Form**
1. Create new list "Expense Reports"
2. Columns: Title, Amount (Currency), Category (Choice), Receipt (Image), Date
3. Customize form with Power Apps
4. Add validation: Amount must be > 0
5. Add calculated field: "Tax" (Amount * 0.08)
6. Add help text for each field
7. Publish form
8. Test creating expense report

**Exercise 5: Build Simple App from Scratch**
1. Create "Team Directory" list
2. Columns: Name, Title, Department, Phone, Email, Photo (Image URL)
3. Build canvas app:
   - Browse screen with gallery (show Name and Title)
   - Search by name
   - Filter by department
   - Detail screen showing all info including photo
   - No edit screen needed (read-only)
4. Add company logo
5. Set theme colors
6. Publish

**Exercise 6: Power Apps + Power Automate**
1. In your Task Tracker app, add "Request Approval" button
2. Create Power Automate flow:
   - Triggered by Power Apps
   - Receives Task ID parameter
   - Starts approval process
   - Returns approval result
3. Call flow from button
4. Show notification with result
5. Test workflow

---

## Day 17: Power BI Integration

### Understanding Power BI

**What is Power BI?**
- Microsoft's business analytics service
- Interactive data visualizations
- Business intelligence reporting
- Self-service analytics
-
