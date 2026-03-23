# Week 2: Site Building & Administration - Detailed Documentation

## Day 8: Creating and Managing Sites

### Understanding Site Creation

**When to Create a New Site:**
- New project or team formation
- Department-specific workspace needed
- Initiative requiring collaboration
- Publishing information broadly
- Creating an intranet portal

**Site Planning Considerations:**
- Purpose and audience
- Content structure needed
- Who needs access
- Integration with other tools
- Long-term maintenance

### Creating a Team Site

**Step-by-Step:**

1. **From SharePoint Home:**
   - Go to SharePoint home (office.com/sharepoint)
   - Click "+ Create site"
   - Select "Team site"

2. **Site Configuration:**
   - **Name:** Clear, descriptive (e.g., "Marketing Team")
   - **Description:** Purpose and scope
   - **Privacy:** 
     - Public: Anyone in organization can find and access
     - Private: Only members can access
   - **Language:** Select primary language
   - **Advanced settings:** (click Show advanced settings)
     - Time zone
     - Site address (URL)

3. **Add Members:**
   - Add owners (site administrators)
   - Add members (contributors)
   - Skip if setting up structure first

4. **Microsoft 365 Group Creation:**
   - Team site automatically creates:
     - Outlook group mailbox
     - Planner board
     - OneNote notebook
     - Teams team (optional)
     - Shared calendar

**Team Site Components Created:**
- Home page with default web parts
- Documents library
- Site notebook (OneNote)
- Conversations (Outlook group)
- Site pages library

### Creating a Communication Site

**Step-by-Step:**

1. **From SharePoint Home:**
   - Click "+ Create site"
   - Select "Communication site"

2. **Choose Design:**
   - **Topic:** General purpose, versatile layout
   - **Showcase:** Visual focus, large images
   - **Blank:** Start from scratch

3. **Site Configuration:**
   - **Name:** Clear, professional (e.g., "Company News Hub")
   - **Description:** What readers will find
   - **Site address:** Custom URL slug
   - **Language:** Primary language
   - **Time zone:** Organization timezone

4. **No Group Creation:**
   - Communication sites are standalone
   - No automatic Microsoft 365 Group
   - Manual permission management

**Communication Site Features:**
- Modern, responsive pages
- News web part
- Hero web part for visual impact
- Events web part
- People web part
- Emphasis on readability

### Site Templates and Designs

**Built-in Templates:**

**Team Site Templates:**
- Crisis Management
- Department
- Project Management
- Team
- Training
- Event Planning

**Communication Site Designs:**
- Topic (default)
- Showcase
- Blank

**Applying Site Templates:**
1. During site creation, select "Use a template"
2. Choose from available templates
3. Template provides:
   - Pre-configured pages
   - Sample content
   - Recommended web parts
   - Lists and libraries

**Custom Site Templates (Site Scripts/Designs):**
- Defined by administrators
- Organization-specific
- Consistent branding and structure
- Applied during or after creation

### Site Settings Overview

**Accessing Site Settings:**
- Click gear icon (⚙️) > "Site settings"
- Or "Site information" > "View all site settings"

**Key Settings Categories:**

**Users and Permissions:**
- Site permissions
- Site collection administrators
- Access requests
- Permission levels
- Site collection features

**Web Designer Galleries:**
- Site content types
- Site columns
- Master pages and page layouts (classic)
- Themes
- Solutions

**Site Administration:**
- Regional settings (time zone, locale)
- Language settings
- Site libraries and lists
- User alerts
- RSS
- Search settings
- Navigation
- Site closure or deletion
- Storage metrics

**Look and Feel:**
- Title, description, icon
- Navigation
- Theme (colors, fonts)
- Site logo
- Header layout
- Footer

**Site Actions:**
- Manage site features
- Save site as template (classic)
- Create subsites (classic)
- Delete this site

### Site Information Panel

**Accessing Site Information:**
- Click "Site information" at top of home page
- Or gear icon > "Site information"

**Editable Elements:**
- Site name
- Site description
- Site logo/icon
- Privacy settings (team sites)
- Hub association
- Site address (limited changes)

**View Options:**
- Site activity
- Storage metrics
- Permissions link
- Site settings link

### Managing Site Features

**Site Collection Features:**
- Activated at site collection level
- Affect all subsites
- Examples:
  - Search Server Web Parts
  - SharePoint Server Enterprise Site features
  - Reporting features

**Site Features:**
- Activated per site
- Examples:
  - Team Collaboration Lists (Events, Tasks)
  - Wiki Page Home Page
  - Following Content
  - Metadata Navigation
  - Content Organizer

**Activating/Deactivating Features:**
1. Site Settings > "Manage site features" or "Site collection features"
2. Click "Activate" or "Deactivate"
3. Some features add web parts, lists, or functionality
4. Deactivating may hide content (not delete)

### Regional Settings

**Time Zone:**
- Critical for date/time accuracy
- Set to organization's primary timezone
- Affects all timestamps
- Users can set personal timezone (overrides)

**Locale:**
- Date/number format
- Currency
- Calendar type
- Sort order
- Examples: English (United States), English (United Kingdom)

**Calendar:**
- Gregorian (default)
- Buddhist Era
- Hijri
- Hebrew

**Work Days and Hours:**
- Define business week (Monday-Friday typical)
- Start/end times for calendars
- Affects Gantt charts and schedules

**Setting Regional Settings:**
1. Site Settings > "Regional settings"
2. Configure all options
3. Click "OK"
4. Takes effect immediately

### Language Settings

**Alternate Languages:**
- Enable multilingual features
- Users choose preferred language
- Interface translates (not content)

**Enabling Alternate Languages:**
1. Site Settings > "Language settings"
2. Select additional languages
3. Click "OK"

**User Experience:**
- Language selector appears
- Navigation, buttons, labels translate
- Content remains in original language
- Use multilingual pages feature for content translation

### Hands-On Exercise Day 8

**Exercise 1: Team Site Creation**
1. Create a Team Site named "Project Alpha Team"
2. Set as Private
3. Add description: "Collaboration space for Project Alpha"
4. Add 2 owners and 3 members
5. Explore the automatically created Microsoft 365 Group resources:
   - Check Outlook for group mailbox
   - Open Planner board
   - View OneNote notebook
6. Customize site name and logo
7. Set timezone to your location

**Exercise 2: Communication Site Creation**
1. Create Communication Site: "Company Newsletter"
2. Choose "Showcase" design
3. Add description
4. Set appropriate timezone and locale
5. Add 3 pages: "Latest News", "Events", "Resources"
6. Configure home page with News and Events web parts

**Exercise 3: Site Settings Management**
1. Navigate to Site Settings
2. Enable these site features:
   - Team Collaboration Lists
3. Configure regional settings for your location
4. Enable one alternate language
5. Review storage metrics
6. Check site collection administrators

**Exercise 4: Site Comparison**
Create a comparison chart:
| Feature | Team Site | Communication Site |
|---------|-----------|-------------------|
| Microsoft 365 Group | Yes | No |
| Default Privacy | Private | Open |
| Primary Use | Collaboration | Broadcasting |
| Automatic Teams | Optional | No |
| Typical Editors | Many | Few |
| Typical Readers | Team | Organization |

---

## Day 9: Site Templates and Designs

### Understanding Site Templates

**What is a Site Template?**
- Pre-configured site structure
- Includes pages, lists, libraries, web parts
- Saves setup time
- Ensures consistency
- Provides best practices

**Template Components:**
- Page layouts
- Navigation structure
- Pre-created lists and libraries
- Sample content
- Web parts configuration
- Theme settings

### Built-in Microsoft Templates

**Team Site Templates:**

**1. Crisis Management**
- Coordinate crisis response
- Includes:
  - Announcements list
  - Crisis status dashboard
  - Contact list
  - Document library
  - Task tracking
  - News feed

**2. Project Management**
- Track project deliverables
- Includes:
  - Project tasks list
  - Timeline view
  - Document library
  - Issues tracker
  - Risks register
  - Project dashboard

**3. Training**
- Deliver training materials
- Includes:
  - Course catalog
  - Training schedules
  - Materials library
  - Attendee tracking
  - Certification records

**4. Event Planning**
- Organize events
- Includes:
  - Event calendar
  - Task list
  - Budget tracker
  - Vendor contacts
  - Guest list
  - Document library

**5. Department**
- General department workspace
- Includes:
  - Staff directory
  - Document library
  - Announcements
  - Calendar
  - Links
  - Tasks

**Communication Site Templates:**

**Topic Design:**
- General-purpose communication
- Features:
  - Hero web part (1-5 images)
  - News web part
  - Quick links
  - Events
  - Highlighted content
- Best for: Company news, department portals

**Showcase Design:**
- Visual storytelling
- Features:
  - Large hero section
  - Image galleries
  - Emphasis on visuals
  - Full-width layouts
- Best for: Product launches, campaigns, visual content

**Blank Design:**
- Clean slate
- Features:
  - Empty page
  - Add any web parts
  - Complete flexibility
- Best for: Custom designs, specific requirements

### Applying Templates

**During Site Creation:**
1. Click "+ Create site"
2. Select site type (Team or Communication)
3. Click "Use a template" (if available)
4. Browse template gallery
5. Select desired template
6. Preview template
7. Click "Use template"
8. Complete site details
9. Template provisions with sample content

**After Site Creation:**
- Cannot change site template after creation
- Must manually add lists/libraries from other templates
- Or create new site with correct template

### Customizing Template-Based Sites

**Modifying Pre-configured Content:**

**Pages:**
- Edit existing pages
- Delete unwanted pages
- Add new pages
- Rearrange sections

**Lists and Libraries:**
- Add custom columns
- Create additional views
- Modify existing items
- Delete sample content

**Web Parts:**
- Reconfigure properties
- Remove unused web parts
- Add additional web parts
- Rearrange layout

**Navigation:**
- Edit site navigation
- Add/remove links
- Create nested structure
- Add external links

**Best Practices:**
1. Keep useful structure from template
2. Replace sample content with real data
3. Remove unused components
4. Train users on template features
5. Document customizations

### Site Designs (Technical)

**What are Site Designs?**
- JSON-based site provisioning
- Applied during site creation
- Automate site setup
- Administrator-created
- Organization-specific

**Site Design Components:**

**Site Scripts:**
- JSON files defining actions
- Example actions:
  - Create lists/libraries
  - Add site columns
  - Set theme
  - Install solutions
  - Configure navigation
  - Apply regional settings

**Site Designs:**
- Container for site scripts
- Visible during site creation
- Can include multiple scripts
- Has icon and description

**Example Site Script (Simplified):**
```json
{
  "verb": "createSPList",
  "listName": "Customer Tracking",
  "templateType": 100,
  "subactions": [
    {
      "verb": "setTitle",
      "title": "Customer Tracking"
    },
    {
      "verb": "addSPField",
      "fieldType": "Text",
      "displayName": "Customer Name"
    }
  ]
}
```

**Viewing Available Site Designs:**
- Shown during site creation
- Organization-specific designs appear
- Hover for description
- Preview shows what will be created

### Custom Branding Through Templates

**Theme Application:**
- Colors (primary, text, background)
- Fonts
- Logo
- Applied consistently

**Layout Consistency:**
- Navigation structure
- Page templates
- Section layouts
- Web part placement

**Content Structure:**
- Standard lists/libraries
- Naming conventions
- Column definitions
- View configurations

### PnP Provisioning Templates

**What is PnP?**
- Patterns and Practices
- Community-driven SharePoint development
- Open-source tools and templates

**PnP Provisioning:**
- XML-based templates
- More powerful than site designs
- Can include:
  - Complete site hierarchies
  - Content types
  - Workflows
  - Custom branding
  - Sample data

**Using PnP Templates:**
- Requires PowerShell
- Administrator-level access
- Can export existing sites as templates
- Can apply templates to new sites

**Example PnP Commands:**
```powershell
# Connect to site
Connect-PnPOnline -Url https://contoso.sharepoint.com/sites/template

# Extract template from existing site
Get-PnPSiteTemplate -Out "MyTemplate.xml"

# Apply template to new site
Apply-PnPProvisioningTemplate -Path "MyTemplate.xml"
```

### Site Template Strategy

**When to Use Templates:**
- Repeating site structures needed
- Standardization across organization
- Rapid deployment required
- Training/onboarding sites
- Project sites with similar needs

**Template Governance:**
- Define approved templates
- Document template purposes
- Provide template selection guidance
- Regular template reviews
- Update templates as needs evolve

**Creating Your Own Strategy:**
1. Identify common site patterns
2. Document required components
3. Create pilot sites
4. Extract as templates
5. Test with users
6. Deploy organization-wide
7. Maintain and update

### Hands-On Exercise Day 9

**Exercise 1: Explore Built-in Templates**
1. Create a Team Site using "Project Management" template
2. Explore pre-configured:
   - Lists (Tasks, Issues, Risks)
   - Document library structure
   - Home page layout
   - Sample content
3. Identify useful components
4. List what you'd customize

**Exercise 2: Template Comparison**
Create three Communication Sites with different designs:
1. Topic design - "Company Updates"
2. Showcase design - "Product Gallery"
3. Blank design - "Custom Portal"

Compare:
- Initial layout
- Available web parts
- Visual emphasis
- Best use cases

**Exercise 3: Customize Template Site**
Using your Project Management site:
1. Replace sample projects with 3 real project tasks
2. Add custom column "Budget" to tasks list
3. Create view "My Assigned Tasks"
4. Customize home page:
   - Update hero image
   - Edit welcome text
   - Add Quick Links web part
5. Configure navigation
6. Delete unused sample content

**Exercise 4: Document Template Needs**
Create a document listing:
1. Three types of sites your organization needs regularly
2. For each type, list:
   - Purpose
   - Required lists/libraries
   - Necessary web parts
   - User roles
   - Content types needed
3. Sketch ideal template for one site type

**Exercise 5: Site Design Research**
1. Check what site designs are available in your tenant during site creation
2. If custom designs exist, document:
   - Design name
   - Description
   - When to use
3. Research how site designs are created (read Microsoft documentation)

---

## Day 10: Branding and Theming

### Understanding SharePoint Themes

**What is a SharePoint Theme?**
- Collection of colors and fonts
- Applied to entire site
- Provides consistent visual identity
- Easy to change
- No coding required

**Theme Components:**
- Primary color
- Text color
- Background color
- Accent colors
- Font schemes

### Applying Built-in Themes

**Accessing Theme Settings:**
1. Click gear icon (⚙️)
2. Select "Change the look"
3. Or Site Settings > "Change the look"

**Built-in Themes:**
- **Red:** Bold, energetic
- **Orange:** Warm, creative
- **Green:** Fresh, natural
- **Blue:** Professional, trustworthy (default)
- **Purple:** Elegant, sophisticated
- **Teal:** Modern, balanced
- **Yellow:** Bright, optimistic
- **Gray:** Neutral, professional

**Applying a Theme:**
1. Click on theme thumbnail
2. Preview appears
3. Review header, text, buttons
4. Click "Save" to apply
5. Changes take effect immediately

**Theme Preview:**
- Shows site header
- Sample text and links
- Button styles
- Background colors
- Allows comparison before committing

### Creating Custom Themes

**Custom Theme Options:**

**Method 1: Through UI**
1. Change the look > Theme selection
2. Click on a theme
3. Scroll down to customization options
4. Adjust colors using color pickers:
   - Header background
   - Header text
   - Navigation background
   - Site background
   - Command links
   - Link hover color
5. Preview changes
6. Save theme

**Method 2: PowerShell (Administrator)**
```powershell
# Define custom theme
Add-SPOTheme -Identity "CompanyBrand" `
  -Palette @{
    "themePrimary" = "#0078d4";
    "themeLighterAlt" = "#eff6fc";
    "themeLighter" = "#deecf9";
    "themeLight" = "#c7e0f4";
    "themeTertiary" = "#71afe5";
    "themeSecondary" = "#2b88d8";
    "themeDarkAlt" = "#106ebe";
    "themeDark" = "#005a9e";
    "themeDarker" = "#004578";
    "neutralLighterAlt" = "#faf9f8";
    "neutralLighter" = "#f3f2f1";
    "neutralLight" = "#edebe9";
    "neutralQuaternaryAlt" = "#e1dfdd";
    "neutralQuaternary" = "#d0d0d0";
    "neutralTertiaryAlt" = "#c8c6c4";
    "neutralTertiary" = "#a19f9d";
    "neutralSecondary" = "#605e5c";
    "neutralPrimaryAlt" = "#3b3a39";
    "neutralPrimary" = "#323130";
    "neutralDark" = "#201f1e";
    "black" = "#000000";
    "white" = "#ffffff";
  } `
  -IsInverted $false
```

**Theme Color Properties:**
- **themePrimary:** Main brand color, buttons
- **themeSecondary:** Hover states, accents
- **themeDark/themeDarker:** Dark variations
- **themeLight/themeLighter:** Light variations
- **neutralPrimary:** Body text
- **neutralSecondary:** Secondary text
- **neutralTertiary:** Disabled text
- **white/black:** Backgrounds and contrast

### Site Logo and Icon

**Adding Site Logo:**
1. Site Settings > "Title, description, and logo"
2. Or "Change the look" > "Header"
3. Click "Change" under logo
4. Upload image:
   - Recommended: 192x192 pixels
   - Square format
   - PNG or JPG
   - Under 10MB
5. Preview and save

**Site Icon (Favicon):**
- Small icon in browser tab
- 32x32 or 64x64 pixels
- Appears in bookmarks
- Upload via "Change the look"

**Logo Best Practices:**
- Use transparent background (PNG)
- High contrast with header color
- Simple, recognizable design
- Test at small sizes
- Company branding consistency

### Header Layouts

**Compact Header:**
- Minimal space
- Logo, title, navigation
- More content area
- Professional look
- Default option

**Standard Header:**
- Medium height
- Prominent logo placement
- Good balance
- Clear navigation

**Extended Header:**
- Large header area
- Big logo display
- Visual impact
- Less content area
- Best for communication sites

**Minimal Header:**
- Very compact
- Maximum content space
- Subtle branding
- App-like experience

**Changing Header Layout:**
1. Change the look > "Header"
2. Select layout type
3. Configure:
   - Background color
   - Logo placement
   - Emphasis
4. Preview
5. Save

### Navigation Styling

**Mega Menu:**
- Modern navigation style
- Dropdown submenus
- Visual flyouts
- Supports icons
- Better organization

**Cascading:**
- Traditional dropdown
- Hierarchical
- Text-based
- Compact

**Configuring Navigation Style:**
1. Site Settings > "Navigation"
2. Choose style
3. Configure menu items
4. Set icons (mega menu)
5. Save changes

### Footer Customization

**Footer Components:**
- Logo
- Name
- Links
- Social media icons
- Copyright text

**Configuring Footer:**
1. Change the look > "Footer"
2. Enable/disable footer
3. Add logo
4. Add footer name
5. Configure links:
   - Internal pages
   - External sites
   - Document links
6. Save

**Footer Link Organization:**
- Group by category
- Use clear labels
- Limit number (6-8 recommended)
- Include important resources:
  - Privacy policy
  - Terms of use
  - Contact
  - Help/Support

### Page Background and Sections

**Section Backgrounds:**
- Applied per section
- Colors from theme
- Custom images
- Overlays for readability

**Adding Section Background:**
1. Edit page
2. Hover over section
3. Click "..." menu
4. Select section background
5. Choose:
   - Solid color
   - Gradient
   - Image
6. Adjust opacity if using image
7. Save

**Background Image Tips:**
- High resolution (1920x1080+)
- Not too busy (text must be readable)
- Use overlays for contrast
- Optimize file size
- Consider mobile view

### Typography and Fonts

**Modern SharePoint Fonts:**
- Segoe UI (default, Microsoft standard)
- System fonts for performance
- Cannot easily change fonts without code

**Text Styling Options:**
- Headings (H1, H2, H3, H4)
- Normal paragraph text
- Callout
- Quote
- Code block

**Font Customization (Advanced):**
- Requires SharePoint Framework (SPFx)
- Application customizer
- CSS injection
- Organization-wide deployment

**Best Practices:**
- Stick with default fonts for readability
- Use heading hierarchy properly
- Sufficient contrast ratios (WCAG)
- Readable font sizes
- Consistent styling

### Responsive Design Considerations

**Mobile View:**
- Themes automatically adapt
- Test on mobile devices
- Check header on small screens
- Verify logo visibility
- Ensure touch-friendly navigation

**Tablet View:**
- Intermediate layout
- Different breakpoints
- Test all orientations
- Verify web part rendering

**Desktop View:**
- Full featured
- Wide layouts supported
- Hero images display fully

**Testing Responsive Design:**
1. Browser developer tools (F12)
2. Device emulation mode
3. Test common resolutions:
   - 320px (mobile)
   - 768px (tablet)
   - 1366px (laptop)
   - 1920px (desktop)

### Brand Governance

**Establishing Brand Guidelines:**
- Define official colors (hex codes)
- Specify logo usage rules
- Document theme settings
- Create approval process
- Train site creators

**Maintaining Consistency:**
- Provide theme templates
- Limit custom themes
- Regular brand audits
- Update guidelines as needed
- Provide support resources

**Multi-Brand Organizations:**
- Different themes per division
- Hub-level branding
- Consistent corporate elements
- Local customization allowed within guidelines

### Hands-On Exercise Day 10

**Exercise 1: Theme Exploration**
1. Create a new Communication Site
2. Try each built-in theme:
   - Red, Orange, Green, Blue, Purple, Teal
3. Document which theme works best for:
   - Professional services
   - Creative agency
   - Healthcare
   - Education
   - Technology

**Exercise 2: Custom Theme Creation**
1. Choose your favorite base theme
2. Customize colors to create brand:
   - Primary color: #0066CC (or your choice)
   - Secondary color: complementary
   - Background: light neutral
   - Text: dark for contrast
3. Create multiple variations:
   - Light theme
   - Dark theme (if supported)
4. Save and apply

**Exercise 3: Logo and Header**
1. Create or find a logo image (192x192px)
2. Upload to your site
3. Test different header layouts:
   - Compact
   - Standard
   - Extended
   - Minimal
4. Choose best layout for your content
5. Add site icon (favicon)

**Exercise 4: Complete Branding Package**
Create a fully branded site:
1. Apply custom theme
2. Add logo and icon
3. Configure header layout
4. Set up footer with:
   - 6 useful links
   - Copyright notice
5. Create branded page with:
   - Hero section with background image
   - Sections using theme colors
   - Consistent typography
6. Test on mobile device

**Exercise 5: Brand Guidelines Document**
Create a document specifying:
1. Official color palette (hex codes):
   - Primary
   - Secondary
   - Accent
   - Neutral colors
2. Logo usage rules
3. Header layout standard
4. Footer requirements
5. Typography guidelines
6. When to use which theme variation

---

## Day 11: Content Types Deep Dive

### Understanding Content Types in Depth

**What Makes Content Types Powerful?**
- Reusable metadata schemas
- Enforce business rules
- Enable automation
- Support workflows
- Improve search
- Ensure compliance
- Document templates

**Content Type Hierarchy:**
```
System (root)
├── Item (base for all list items)
│   ├── Announcement
│   ├── Contact
│   ├── Event
│   ├── Issue
│   ├── Task
│   └── Custom Item Types
├── Document (base for all documents)
│   ├── Wiki Page
│   ├── Form
│   ├── Picture
│   └── Custom Document Types
└── Folder
    └── Discussion
```

**Inheritance Benefits:**
- Child inherits parent columns
- Modifications to parent cascade down
- Can add columns at any level
- Can override parent settings
- Organized hierarchy

### Creating Content Types

**Site Content Type Creation:**

1. **Access Content Type Gallery:**
   - Site Settings > "Site content types"
   - Shows all available content types
   - Organized by parent type

2. **Create New Content Type:**
   - Click "Create"
   - **Name:** Descriptive, unique (e.g., "Contract Document")
   - **Description:** Purpose and usage
   - **Parent Content Type:** Choose base
     - Document for files
     - Item for list data
   - **Group:** Organize (Custom Content Types or create new group)
   - Click "OK"

3. **Configure Content Type:**
   After creation, configure:
   - Add site columns
   - Set column order
   - Associate workflow
   - Set information management policy
   - Configure document template
   - Set advanced settings

### Adding Columns to Content Types

**Adding Site Columns:**
1. Open content type
2. Click "Add from existing site columns"
3. Select columns from dropdown
4. Click "Add"
5. Click "OK"

**Adding New Columns:**
1. Open content type
2. Click "Add from new site column"
3. Configure column:
   - Name
   - Type
   - Settings
   - Required vs optional
4. Click "OK"

**Column Configuration:**
- **Required:** Must be filled
- **Optional:** Can be empty
- **Hidden:** Not shown to users
- **Read-only:** Display only, cannot edit

**Column Order:**
1. Click "Column order"
2. Use dropdowns to set position
3. Top-to-bottom order
4. Click "OK"

### Document Templates

**What are Document Templates?**
- Pre-formatted documents
- Applied when creating new files
- Can include:
  - Company branding
  - Standard formatting
  - Boilerplate text
  - Placeholder content
  - Form fields

**Setting Document Template:**

1. **For Document Content Types:**
   - Open content type
   - Click "Advanced settings"
   - Upload document template
   - Must be Office format (.docx, .xlsx, .pptx)
   - Can be URL to template

2. **Template Requirements:**
   - Compatible with Office Online
   - Saved in correct format
   - Under 10MB recommended
   - Tested and validated

3. **Template Best Practices:**
   - Include styles and formatting
   - Add instructional text
   - Use content controls (Word)
   - Include company branding
   - Test template creation

**Example Template Uses:**
- Contract template with standard clauses
- Proposal template with company branding
- Meeting minutes template with agenda structure
- Invoice template with calculation fields
- Report template with required sections

### Content Type Publishing

**Publishing Content Types:**
- Makes available to subsites
- Enables reuse across site collection
- Maintains consistency
- Centralized management

**Publishing Process:**
1. Create content type at site collection root
2. Configure completely
3. Subsites automatically see it
4. Can add to libraries in subsites

**Content Type Hub:**
- Enterprise feature (SharePoint Server)
- Publish across site collections
- Managed metadata service required
- Organization-wide content types

### Using Content Types in Libraries

**Enabling Content Types:**
1. Library Settings
2. "Advanced settings"
3. "Allow management of content types" > Yes
4. Click "OK"

**Adding Content Types to Library:**
1. Library Settings
2. Click "Add from existing site content types"
3. Select content types
4. Click "Add"
5. Click "OK"

**Setting Default Content Type:**
1. Library Settings
2. Under Content Types section
3. Click "Change new button order and default content type"
4. Reorder using Position dropdown
5. Top item is default
6. Uncheck to hide from New menu
7. Click "OK"

**Content Type Columns in Library:**
- Columns from content type appear automatically
- Library can have additional columns
- Content type columns cannot be removed (only hidden)
- Can set column defaults per content type

### Content Type Workflows

**Associating Workflows:**
1. Open content type
2. Click "Workflow settings"
3. Add workflow
4. Configure workflow for this content type
5. Workflow applies to all items of this type

**Workflow Examples:**
- **Approval workflow:** For contracts requiring review
- **Feedback workflow:** For documents needing input
- **Disposition workflow:** For records management
- **Custom workflows:** Power Automate flows

**Workflow Benefits:**
- Automatic on creation
- Consistent process
- No manual initiation needed
- Enforces compliance

### Information Management Policies

**Policy Types:**

**1. Retention:**
- Specify retention period
- Action after period (delete, archive)
- Based on creation/modification date
- Example: Delete after 7 years

**2. Auditing:**
- Track document events
- Opening, editing, deleting
- Who and when
- Compliance reporting

**3. Barcodes:**
- Generate unique identifiers
- Track physical documents
- Print on documents
- Inventory management

**4. Labels:**
- Print document properties
- Add to documents automatically
- Include metadata
- Compliance requirements

**Setting Policies:**
1. Open content type
2. Click "Information management policy settings"
3. Enable desired policies
4. Configure settings
5. Click "OK"

### Content Type Syndication

**Site Collection Content Types:**
- Available throughout site collection
- Inherit to subsites
- Maintain consistency
- Easier management

**Cross-Site Collection Sharing:**
- Requires Content Type Hub
- Managed Metadata Service
- Timer job for syndication
- Enterprise-level standardization

### Advanced Content Type Features

**Sealed Content Types:**
- Cannot be edited
- Prevents modifications
- Ensures standardization
- Administrator control

**Read-Only Content Types:**
- Can view but not modify
- Protects configurations
- Prevents breaking changes

**Hidden Content Types:**
- Not visible in UI
- Used programmatically
- Custom solutions
- API access only

### Content Type Best Practices

**Planning:**
1. Identify document/item types
2. Define required metadata
3. Map inheritance structure
4. Document business rules
5. Design template documents

**Implementation:**
1. Start with base content types
2. Test thoroughly
3. Deploy to pilot sites
4. Train users
5. Roll out organization-wide

**Maintenance:**
1. Regular reviews
2. Update templates as needed
3. Retire unused content types
4. Document changes
5. Communicate updates

**Governance:**
- Define approval process
- Document content type purpose
- Control who can create
- Regular audits
- Version control

### Hands-On Exercise Day 11

**Exercise 1: Create Content Type Hierarchy**
1. Create site content type "Company Document"
   - Parent: Document
   - Add columns:
     - Department (Choice)
     - Document Owner (Person)
     - Classification (Choice: Public, Internal, Confidential)
     - Review Date
