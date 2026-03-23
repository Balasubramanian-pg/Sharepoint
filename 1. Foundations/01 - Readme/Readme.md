# Week 1: SharePoint Foundations - Detailed Documentation

## Day 1: Introduction to SharePoint & Core Concepts

### What is SharePoint?
SharePoint is Microsoft's web-based collaborative platform that integrates with Microsoft 365. It serves as:
- A document management and storage system
- An intranet portal for organizations
- A collaboration platform for teams
- A content management system
- A business process automation tool

### SharePoint Online vs SharePoint Server

**SharePoint Online (Cloud-based)**
- Part of Microsoft 365 subscription
- Hosted by Microsoft in the cloud
- Automatic updates and new features
- No infrastructure management needed
- Accessible from anywhere with internet
- Pay-per-user pricing model
- Modern user interface and features
- Integrated with Microsoft 365 apps (Teams, OneDrive, etc.)

**SharePoint Server (On-Premises)**
- Installed on your own servers
- Full control over infrastructure and data
- Manual updates and patches required
- One-time licensing cost plus maintenance
- Requires IT infrastructure and expertise
- Can be isolated from internet
- May have older feature set
- SharePoint 2013, 2016, 2019, Subscription Edition are versions

**Hybrid Approach**
- Combines on-premises and cloud
- Allows gradual migration
- Can sync specific content to cloud
- Unified search across both environments

### Core SharePoint Terminology

**Sites**
- The basic building block of SharePoint
- A collection of related web pages, lists, and libraries
- Has its own permissions, navigation, and content

**Site Collections**
- A group of SharePoint sites with shared settings
- Top-level site plus any subsites beneath it
- Shares administration, navigation, and sometimes permissions
- Has its own URL

**Subsites**
- Sites created underneath a parent site
- Inherit permissions by default (can be broken)
- Part of the same site collection

**Hub Sites**
- Modern SharePoint feature
- Connect related sites for unified navigation and branding
- Sites remain independent but share common elements
- Don't create a hierarchy like subsites

**Lists**
- Structured collections of data
- Similar to database tables or Excel spreadsheets
- Rows are "items" with various column types
- Examples: task lists, calendars, contacts, custom lists

**Libraries**
- Special type of list designed for storing files
- Document libraries are most common
- Includes version history, check-in/check-out
- Can store any file type

**Pages**
- Web pages within SharePoint sites
- Modern pages (responsive, easy to edit)
- Classic pages (older, more complex)
- Wiki pages (collaborative editing)

**Web Parts**
- Modular components added to pages
- Display specific content or functionality
- Examples: document library viewer, news, events, embed code
- Can be configured and customized

### Hands-On Exercise Day 1
1. Access your SharePoint environment (create Microsoft 365 trial if needed)
2. Navigate through your default SharePoint home page
3. Identify different sites available to you
4. Explore the difference between team sites and communication sites
5. Browse through a document library and a list
6. View a modern SharePoint page and identify web parts

---

## Day 2: SharePoint Architecture & Navigation

### SharePoint Architecture Hierarchy

**Tenant Level**
- Your entire Microsoft 365 organization
- Highest level of SharePoint administration
- Contains all site collections
- Configured in SharePoint Admin Center

**Site Collection Level**
- Top-level site with unique URL
- Independent storage quota
- Own permission structure
- Can contain multiple subsites
- Example: https://yourcompany.sharepoint.com/sites/marketing

**Site Level**
- Individual sites (team site, communication site)
- Contains lists, libraries, and pages
- Has its own settings and permissions

**List/Library Level**
- Container for items or documents
- Has columns defining data structure
- Can have unique permissions

**Item/File Level**
- Individual list items or documents
- Lowest level of content
- Can have unique permissions

### Types of SharePoint Sites

**Team Site**
- Designed for team collaboration
- Includes default document library
- Connected to Microsoft 365 Group
- Automatically gets Teams, Planner, Outlook group
- Private by default (members only)
- Good for: project teams, departments

**Communication Site**
- Designed for broadcasting information
- Modern, visually appealing
- No Microsoft 365 Group by default
- Few editors, many readers
- Good for: news, policies, corporate communications, portals

**Hub Site**
- Not a site type, but a site designation
- Connects related sites together
- Provides shared navigation and branding
- Aggregates content from associated sites
- Good for: departmental portals, divisional intranets

### Navigation Structure

**Top Navigation (Suite Bar)**
- App launcher (waffle icon) - access all Microsoft 365 apps
- SharePoint home
- Search bar
- Notifications and settings

**Hub Navigation**
- Appears below suite bar
- Shared across all sites associated with hub
- Customizable links to important resources

**Local Navigation (Left Side)**
- Specific to current site
- Quick launch links
- Can be customized per site
- Shows site contents, pages, documents

**Breadcrumb Navigation**
- Shows your current location
- Click to navigate up the hierarchy

### Site Contents
Access "Site contents" to see everything in your site:
- All lists and libraries
- Pages library
- Site Assets
- Style Library
- Apps you can add

### Hands-On Exercise Day 2
1. Create a new Team Site (Name: "Practice Team Site")
2. Create a new Communication Site (Name: "Practice Comm Site")
3. Explore the differences in layout and default features
4. Navigate through Site Contents
5. Customize the left navigation on your Team Site
6. Add quick links to your home page
7. Understand your current site's URL structure

---

## Day 3: Permissions & Security Model

### SharePoint Permission Levels

**Default Permission Levels:**

**Full Control**
- Complete control over the site
- Can manage permissions
- Can delete the site
- Sees all content regardless of other permissions

**Design**
- Can create lists and libraries
- Can modify pages and apply themes
- Cannot manage permissions

**Edit**
- Can add, edit, and delete items and documents
- Can create lists and libraries
- Most common for content creators

**Contribute**
- Can add, edit, and delete items in existing lists/libraries
- Cannot create new lists or libraries
- Good for team members who add content

**Read**
- Can view pages and items
- Can download documents
- Cannot make any changes
- Good for viewers and guests

**View Only**
- Can view items but cannot download
- Most restrictive read access
- Good for sensitive documents

### SharePoint Groups

**Default SharePoint Groups:**

**[Site Name] Owners**
- Has Full Control permission level
- Can manage everything including permissions
- Typically managers, site administrators

**[Site Name] Members**
- Has Edit permission level (can be customized)
- Can create and edit content
- Typical team members

**[Site Name] Visitors**
- Has Read permission level
- Can view content only
- Guest users, read-only stakeholders

### Permission Inheritance

**How Inheritance Works:**
- By default, all content inherits permissions from parent
- Site inherits from site collection
- Library inherits from site
- Folder inherits from library
- Item inherits from folder or library

**Breaking Inheritance:**
- You can stop inheritance at any level
- Creates unique permissions for that object
- Changes to parent no longer affect this item
- Increases management complexity

**Best Practice:** Use inheritance whenever possible. Only break when absolutely necessary for security.

### Types of Users

**Internal Users**
- Employees with Microsoft 365 accounts
- Full access to all features
- Can be added to SharePoint groups

**External Users (Guests)**
- People outside your organization
- Require invitation or sharing link
- Limited features (no Microsoft 365 Group access)
- Can be granted specific permissions

**Everyone Except External Users**
- Special group for all internal users
- Useful for company-wide content
- Automatically includes all employees

**Everyone**
- Includes everyone with access, including guests
- Use cautiously

### Sharing Settings

**Sharing Links Types:**

**Anyone Links (Anonymous)**
- No sign-in required
- Can be forwarded to anyone
- Most open sharing option
- Can be disabled by admin

**People in Your Organization**
- Requires company sign-in
- Can be forwarded internally
- Good for internal-only content

**Specific People**
- Only specified users can access
- Must sign in
- Most secure option
- Best for sensitive content

**Link Expiration:**
- Set time limits on shared links
- Automatically revokes access
- Good for temporary collaborators

### Security Best Practices

1. **Principle of Least Privilege:** Give users minimum permissions needed
2. **Use Groups, Not Individuals:** Add users to groups rather than individual permissions
3. **Limit Full Control:** Keep owners list small
4. **Regular Audits:** Review permissions quarterly
5. **Document Decisions:** Keep record of why permissions were broken
6. **External Sharing Policy:** Establish clear rules for guest access
7. **Sensitive Data:** Use view-only for highly sensitive documents

### Hands-On Exercise Day 3
1. Create three test users (or use colleagues)
2. Add users to Owners, Members, and Visitors groups
3. Create a document library called "Permission Test"
4. Upload a test document
5. Break inheritance on that document
6. Grant specific people access to just that document
7. Test access by signing in as different users
8. Create a sharing link for "Specific People"
9. Create a sharing link for "Anyone with the link"
10. Review who has access to your site (Site Settings > Site Permissions)

---

## Day 4: Document Libraries - Basics

### What is a Document Library?

A document library is a secure location where you and your team can create, collect, update, and share files. Each library displays a list of files and key information about them.

### Creating a Document Library

**Method 1: From Site Contents**
1. Go to Site Contents
2. Click "New" > "Document library"
3. Enter name and description
4. Click "Create"

**Method 2: From Home Page**
1. Click "+ New" 
2. Select "Document library"
3. Configure settings
4. Click "Create"

### Document Library Views

**All Documents (Default View)**
- Shows all documents in library
- Columns: Name, Modified, Modified By

**Standard View Types:**
- Standard view (most common)
- Datasheet view (grid, like Excel)
- Calendar view (for dated items)
- Gantt view (project timelines)

**Creating Custom Views:**
1. Click view dropdown > "All Documents" dropdown
2. Select "Edit current view" or "Create new view"
3. Choose columns to display
4. Set filter criteria
5. Set sort order
6. Set grouping
7. Save view

### Uploading Files

**Single File Upload:**
- Click "Upload" > select file
- Drag and drop directly into library

**Multiple Files:**
- Click "Upload" > select multiple files (Ctrl+click)
- Drag and drop multiple files
- Upload folder (preserves structure)

**Sync with OneDrive:**
- Click "Sync" button
- Files appear in File Explorer
- Work offline, syncs when online
- Two-way synchronization

### Organizing Documents

**Folders:**
- Click "New" > "Folder"
- Organize documents hierarchically
- Can have unique permissions
- Note: Modern SharePoint favors metadata over folders

**Metadata (Columns):**
- Better than folders for organization
- Allows multiple categorizations
- Enables powerful filtering and views
- Appears in search results

### Version History

**Enabling Versioning:**
1. Library settings > "Versioning settings"
2. Choose version type:
   - Major versions only (1.0, 2.0, 3.0)
   - Major and minor versions (1.0, 1.1, 1.2, 2.0)
3. Set number of versions to retain

**Working with Versions:**
- Right-click document > "Version history"
- View all previous versions
- Restore previous version
- Delete specific versions
- See who made changes and when

**Major vs Minor Versions:**
- Major: Published, visible to all with read access (1.0, 2.0)
- Minor: Draft, visible only to those with edit access (0.1, 0.2)
- Good for review processes

### Check-In and Check-Out

**Check-Out:**
- Prevents others from editing
- You have exclusive lock
- Right-click > "Check out"
- Document shows lock icon

**Check-In:**
- Releases lock
- Makes changes visible to others
- Add comments about changes
- Right-click > "Check in"

**Requiring Check-Out:**
- Library settings > "Versioning settings"
- Enable "Require documents to be checked out"
- Forces users to check out before editing
- Prevents conflicts

### Document Properties and Details

**Viewing Properties:**
- Select document
- Details pane appears on right
- Shows metadata, version, activity

**Editing Properties:**
- Click "Edit all" in details pane
- Or right-click > "Properties" (classic)
- Update metadata fields
- Save changes

### Co-Authoring

**Real-Time Collaboration:**
- Multiple users edit simultaneously
- Works with Office Online and desktop Office
- See others' cursors and changes
- Auto-saves periodically

**Requirements:**
- Office Online or Office 2016+
- Document must be in SharePoint/OneDrive
- Check-out must be disabled

### Hands-On Exercise Day 4
1. Create a document library called "Team Documents"
2. Upload 5-10 sample documents
3. Create folders: "Drafts", "Final", "Archive"
4. Move documents into appropriate folders
5. Enable versioning (major versions, keep 10 versions)
6. Edit a document multiple times
7. View version history
8. Restore a previous version
9. Enable require check-out
10. Check out a document, edit, check in with comments
11. Create a custom view showing only documents modified in last 7 days
12. Sync the library to your computer
13. Practice co-authoring with a colleague

---

## Day 5: Lists - Basics and List Types

### What is a SharePoint List?

A list is a collection of data that you can share with team members. It's similar to a spreadsheet or database table but provides more structure, validation, and integration capabilities.

### Creating a List

**Method 1: From Site Contents**
1. Site Contents > "New" > "List"
2. Choose creation method:
   - Blank list
   - From Excel
   - From existing list
   - Template

**Method 2: Microsoft Lists App**
1. Access from Microsoft 365 app launcher
2. Click "New list"
3. More templates available

### Standard List Templates

**Custom List**
- Blank list, fully customizable
- Start with Title column only
- Add any columns you need

**Task List**
- Pre-configured for task management
- Includes: Task name, assigned to, status, priority, due date
- Timeline view available

**Issue Tracking**
- Track bugs, issues, problems
- Includes: Title, assigned to, status, priority, category
- Good for support teams

**Calendar**
- Display events in calendar format
- Includes: Title, location, start time, end time
- Sync with Outlook

**Announcements**
- Share news and status updates
- Includes: Title, body, expiration date
- Can show on home page

**Contacts**
- Store contact information
- Includes: name, phone, email, address
- Sync with Outlook

**Links**
- Collection of hyperlinks
- Includes: URL, description
- Good for resource lists

**Survey**
- Gather feedback and opinions
- Anonymous responses option
- Branching logic available

### List Columns (Field Types)

**Single Line of Text**
- Short text input
- Max 255 characters
- Example: Name, Title, Department

**Multiple Lines of Text**
- Long text, paragraphs
- Plain text, rich text, or enhanced rich text
- Example: Description, Notes, Comments

**Choice**
- Dropdown or radio buttons
- Define specific options
- Can allow fill-in choices
- Example: Status (New, In Progress, Complete)

**Number**
- Numeric values
- Can set min/max, decimals
- Validation rules
- Example: Quantity, Budget, Score

**Currency**
- Monetary values
- Set currency format
- Decimal places
- Example: Cost, Price, Budget

**Date and Time**
- Date picker
- Date only or date + time
- Can set default to today
- Example: Due Date, Created Date

**Yes/No**
- Checkbox
- True/false, boolean
- Example: Approved, Completed, Active

**Person or Group**
- Select users from directory
- Single or multiple selections
- Can restrict to specific groups
- Example: Assigned To, Manager, Team Members

**Hyperlink**
- URL with optional description
- Clickable link
- Example: Reference Link, Website

**Picture**
- Image URLs
- Displays thumbnail
- Example: Product Image, Logo

**Calculated**
- Formula-based on other columns
- Like Excel formulas
- Example: Total Cost = Quantity * Price

**Lookup**
- Reference another list
- Create relationships between lists
- Example: Select Project from Projects list

**Managed Metadata**
- Enterprise keywords
- Controlled taxonomy
- Term store integration

### Creating and Managing Columns

**Adding a Column:**
1. Click "+ Add column" in list
2. Choose column type
3. Enter name and settings
4. Set as required if needed
5. Click "Save"

**Column Settings:**
- Required vs optional
- Default values
- Validation formulas
- Conditional formatting
- Column indexing (for performance)

**Editing Columns:**
- List settings > Columns section
- Click column name to edit
- Change type (limited options)
- Modify settings

**Deleting Columns:**
- List settings > Click column name
- Scroll down > "Delete"
- Cannot be undone (data lost)

### Working with List Items

**Adding Items:**
1. Click "+ New" button
2. Fill in form fields
3. Click "Save"

**Quick Edit Mode:**
- Click "Quick edit" at top
- Spreadsheet-like interface
- Edit multiple items quickly
- Copy/paste from Excel

**Editing Items:**
- Click item to open details pane
- Or click "..." menu > "Edit"
- Update fields
- Save changes

**Deleting Items:**
- Select item(s)
- Click "Delete" from toolbar
- Or "..." menu > "Delete"
- Goes to recycle bin

### List Views

**Creating Views:**
1. View dropdown > "Create new view"
2. Choose view type
3. Name your view
4. Select columns to show
5. Set filters (show only items where...)
6. Set sort order
7. Set grouping
8. Set item limit
9. Make public or personal
10. Save

**View Types:**
- **List:** Standard table format
- **Compact List:** Less spacing
- **Tiles:** Card-based layout with images
- **Calendar:** Events in calendar format
- **Gantt:** Timeline for project tracking
- **Board:** Kanban-style cards

**Filtering:**
- Column header dropdowns for quick filters
- Create view with permanent filters
- Filter by: equals, contains, begins with, etc.

**Grouping:**
- Organize items by column values
- Collapsible sections
- Multi-level grouping possible

**Sorting:**
- Primary and secondary sort
- Ascending or descending

### Hands-On Exercise Day 5
1. Create a custom list called "Project Tracker"
2. Add these columns:
   - Project Name (Single line text) - Required
   - Status (Choice: Not Started, In Progress, Completed, On Hold)
   - Priority (Choice: High, Medium, Low)
   - Owner (Person or Group)
   - Start Date (Date)
   - Due Date (Date)
   - Budget (Currency)
   - % Complete (Number, 0-100)
   - Description (Multiple lines of text)
3. Add 10 sample project items
4. Create a view showing only "High Priority" projects
5. Create a view grouping by Status
6. Create a view showing projects due this month
7. Practice Quick Edit mode
8. Create a Task List from template
9. Add 5 tasks with different assignees
10. Create a calendar view of your tasks

---

## Day 6: Metadata, Content Types, and Views

### Understanding Metadata

**What is Metadata?**
- Data about data
- Descriptive information about documents/items
- Examples: Author, Date Created, Department, Document Type
- Appears as columns in lists and libraries

**Benefits of Metadata:**
- Better search results
- Easier filtering and sorting
- Consistent categorization
- Less reliance on folder structures
- Automated workflows
- Improved compliance

**Metadata vs Folders:**

*Folders:*
- One location per document
- Difficult to find across categories
- Deep hierarchies are confusing
- Permissions management complex

*Metadata:*
- Multiple categorizations per document
- Find documents multiple ways
- Flat structure, easier navigation
- Simpler permissions

### Site Columns

**What are Site Columns?**
- Reusable column definitions
- Created once, used across multiple lists/libraries
- Ensures consistency
- Update once, applies everywhere

**Creating Site Columns:**
1. Site Settings > "Site columns" (under Web Designer Galleries)
2. Click "Create"
3. Enter column name
4. Choose type
5. Select group (organization/category)
6. Configure settings
7. Click "OK"

**Adding Site Columns to Lists:**
1. List/Library settings
2. Click "Add from existing site columns"
3. Select columns
4. Click "Add"
5. Click "OK"

**Built-in Site Column Groups:**
- Core Contact and Calendar Columns
- Core Document Columns
- Core Task and Issue Columns
- Custom Columns (your created columns)
- Page Layout Columns

### Content Types

**What is a Content Type?**
- Template for a type of content
- Defines columns, workflows, policies
- Example content types: Document, Presentation, Contract, Invoice
- Inherits from parent content types

**Content Type Hierarchy:**
```
System (base)
├── Item
│   ├── Document
│   │   ├── Contract
│   │   └── Report
│   └── Task
└── Folder
```

**Why Use Content Types?**
- Enforce consistent metadata
- Different document types in same library
- Attach workflows automatically
- Apply information management policies
- Set document templates

**Creating Content Types:**
1. Site Settings > "Site content types"
2. Click "Create"
3. Enter name and description
4. Choose parent content type
5. Select group
6. Click "OK"
7. Add site columns
8. Configure settings
9. Set document template (for documents)

**Adding Content Types to Libraries:**
1. Library Settings > "Advanced settings"
2. Enable "Allow management of content types"
3. Click "OK"
4. Back in library settings, click "Add from existing site content types"
5. Select content types
6. Click "Add" > "OK"

**Using Content Types:**
- "New" button shows different content types
- Each type may have different columns
- Different templates for documents
- Can set default content type

### Advanced Views

**View Components:**

**Columns:**
- Select which columns to display
- Reorder columns
- Set column width

**Sort:**
- Primary and secondary sort
- Ascending or descending
- Example: Sort by Due Date, then by Priority

**Filter:**
- Show items matching criteria
- Multiple conditions with AND/OR
- Example: [Status] equals "Active" AND [Priority] equals "High"

**Group By:**
- Organize items into collapsible sections
- Up to 2 levels of grouping
- Example: Group by Department, then by Status

**Totals:**
- Calculate sums, averages, counts
- Appears at bottom of columns
- Example: Total budget, Count of items

**Style:**
- Basic table, Boxed, Shaded
- Changes visual appearance

**Folders:**
- Show items inside folders
- Show all items without folders (flattened)

**Item Limit:**
- Number of items to display
- Performance optimization
- Pagination settings

**Mobile:**
- Optimize for mobile devices
- Number of items to show

**Creating a Complex View Example:**

*Scenario: Active Projects by Department*
1. Create new view named "Active Projects by Department"
2. Columns: Project Name, Owner, Due Date, Budget, Status
3. Filter: [Status] equals "Active"
4. Sort: Due Date (ascending)
5. Group: Department
6. Totals: Count of Project Name, Sum of Budget
7. Save as public view

### View Formatting (JSON)

**What is View Formatting?**
- Customize how columns display
- Use JSON code
- Add colors, icons, progress bars
- Conditional formatting

**Simple Example - Color Coding Priority:**
```json
{
  "elmType": "div",
  "txtContent": "@currentField",
  "style": {
    "color": "=if(@currentField == 'High', 'red', if(@currentField == 'Medium', 'orange', 'green'))"
  }
}
```

**Where to Add:**
1. Click column header dropdown
2. Select "Column settings" > "Format this column"
3. Choose template or "Advanced mode"
4. Paste JSON
5. Click "Save"

**Common Formatting Scenarios:**
- Color-code status values
- Add icons based on conditions
- Progress bars for percentages
- Clickable buttons
- User profile pictures
- Data bars for numbers

### Hands-On Exercise Day 6

**Part 1: Site Columns and Content Types**
1. Create site columns:
   - Department (Choice: IT, HR, Finance, Operations)
   - Document Status (Choice: Draft, Review, Approved)
   - Review Date (Date)
   - Classification (Choice: Internal, Confidential, Public)

2. Create content type "Company Document":
   - Based on Document
   - Add all site columns created above
   - Set a custom document template (Word document)

3. Create content type "Financial Report":
   - Based on Company Document
   - Add columns: Fiscal Year, Quarter
   
4. Add both content types to a document library

**Part 2: Advanced Views**
1. In your Project Tracker list, create these views:
   - "High Priority Active" - Show only high priority, in progress projects
   - "By Owner" - Group by Owner, sort by due date
   - "Budget Summary" - Show all projects with total budget sum
   - "Overdue" - Filter for past due date items, show in red

2. Apply JSON formatting:
   - Color-code Priority column (Red=High, Yellow=Medium, Green=Low)
   - Add progress bar to % Complete column
   - Add icons to Status column

3. Create a calendar view of project due dates

**Part 3: Metadata Practice**
1. Upload 10 documents to your library with content types
2. Tag each with appropriate metadata
3. Create views to find documents by:
   - Department
   - Status
   - Classification level
4. Practice searching by metadata
5. Compare finding with folders vs metadata

---

## Day 7: Search Functionality & Week 1 Review

### SharePoint Search Basics

**Search Box Location:**
- Top of every SharePoint page
- Searches across all sites you have access to
- Results ranked by relevance

**What Search Indexes:**
- Document content (Word, PDF, Excel, etc.)
- List item data
- Page content
- Metadata (columns)
- File names and properties
- Comments and conversations

**Search Scope:**
- **This Site:** Only current site
- **All Sites:** Entire SharePoint tenant
- Can filter after searching

### Basic Search Techniques

**Simple Searches:**
- Type keywords in search box
- Press Enter
- Results show with snippets

**Phrases:**
- Use quotes for exact phrases
- Example: "quarterly financial report"
- Finds exact match

**Wildcards:**
- Use asterisk (*) for partial words
- Example: finan* finds finance, financial, financing
- Cannot start with wildcard

**Boolean Operators:**
- **AND:** Both terms must appear (budget AND 2024)
- **OR:** Either term can appear (invoice OR receipt)
- **NOT:** Exclude term (report NOT draft)
- Must be uppercase

### Advanced Search Operators

**Property Searches:**
- Search specific metadata
- Format: PropertyName:Value
- Examples:
  - `author:"John Smith"`
  - `filename:budget`
  - `created:2024`
  - `filetype:pdf`
  - `title:proposal`

**Date Ranges:**
- `created>=2024-01-01`
- `modified:today`
- `modified:yesterday`
- `modified:thisweek`
- `modified:thismonth`

**Size Searches:**
- `size>=5000000` (bytes)
- `size<1000000`

**Combining Operators:**
- `author:"Jane Doe" AND filetype:xlsx AND modified>=2024-01-01`
- `(budget OR financial) AND department:finance`

### Search Results Page

**Results Components:**

**Search Box:**
- Refine your search
- Suggestions appear

**Filters (Left Panel):**
- Result type (Files, Sites, People, News)
- Modified date
- People (author)
- Tags
- Click to apply filters

**Results List:**
- Title (clickable)
- Path/location
- Modified date
- Preview snippet
- Thumbnail (for images/videos)

**Result Actions:**
- Hover over result for quick actions
- Open, Share, View details
- See who has access

**Sort Options:**
- Relevance (default)
- Most Recent
- Oldest

### Search Verticals

**Files:**
- Documents and files
- Default search vertical
- Shows all file types

**Sites:**
- SharePoint sites
- Team sites, communication sites
- Shows site descriptions

**People:**
- User profiles
- Shows contact information
- Organizational hierarchy

**News:**
- News posts from SharePoint sites
- Organization-wide news
- Filtered by date

**Videos:**
- Video files
- Microsoft Stream content
- Thumbnails and descriptions

### Improving Search Results

**For Users:**
- Use descriptive file names
- Add comprehensive metadata
- Include keywords in document content
- Tag documents appropriately
- Add descriptions to files

**For Administrators:**
- Configure search schema
- Promote important results
- Create query rules
- Manage result sources
- Set up custom search verticals

### Search Tips and Best Practices

**Effective Searching:**
1. Start broad, then narrow with filters
2. Use metadata properties for precise results
3. Check spelling
4. Use synonyms if first search fails
5. Filter by date for recent content
6. Use filetype to find specific formats

**Common Issues:**
- **No results:** Check spelling, try synonyms, remove filters
- **Too many results:** Add more keywords, use AND, apply filters
- **Wrong results:** Use quotes for phrases, use property searches
- **Missing content:** May not be indexed yet (crawl frequency), check permissions

### Personal Search Preferences

**Recent Searches:**
- Automatically saved
- Click search box to see history
- Quick access to previous searches

**Search Suggestions:**
- Appear as you type
- Based on popular searches
- Your previous searches
- Click to use suggestion

### Week 1 Review & Assessment

**Concepts Covered This Week:**

**Day 1:**
- SharePoint purpose and capabilities
- Online vs Server comparison
- Core terminology (sites, lists, libraries, pages, web parts)

**Day 2:**
- Architecture hierarchy (tenant, site collection, site, list, item)
- Site types (team sites, communication sites, hub sites)
- Navigation structure
- Site contents

**Day 3:**
- Permission levels (Full Control, Design, Edit, Contribute, Read, View Only)
- SharePoint groups (Owners, Members, Visitors)
- Permission inheritance and breaking
- Sharing settings and links
- Security best practices

**Day 4:**
- Document library creation and management
- Uploading and organizing files
- Version history (major and minor versions)
- Check-in and check-out
- Co-authoring and collaboration

**Day 5:**
- List types and templates
- Column types and properties
- Creating and managing list items
- Views (creating, filtering, grouping, sorting)
- Quick edit mode

**Day 6:**
- Metadata concepts and benefits
- Site columns for reusability
- Content types for consistency
- Advanced view creation
- JSON view formatting basics

**Day 7:**
- Search functionality and scope
- Search operators and syntax
- Property searches
- Search verticals
- Optimizing search results

### Week 1 Hands-On Final Project

Create a complete collaborative workspace:

**Project: Department Collaboration Site**

1. **Site Setup:**
   - Create a Team Site called "[Your Department] Hub"
   - Customize the home page with web parts
   - Set up navigation links

2. **Permissions:**
   - Add 3 users to Members group
   - Add 1 user to Visitors group
   - Create a document with unique permissions

3. **Document Management:**
   - Create "Shared Documents" library with versioning
   - Enable require checkout
   - Upload 5 documents
   - Create 3 folders: Working, Final, Archive
   - Check out, edit, and check in a document

4. **Lists:**
   - Create "Project Tasks" list with columns:
     - Task Name, Assigned To, Priority, Status, Due Date, % Complete
   - Add 10 tasks
   - Create 3 custom views: My Tasks, High Priority, Overdue

5. **Metadata & Content Types:**
   - Create site columns: Project Name, Phase, Department
   - Create content type "Project Document"
