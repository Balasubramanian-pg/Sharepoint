# Week 4: Development & Expert-Level - Detailed Documentation

## Day 22: SharePoint REST API

### Understanding REST APIs

**What is REST API?**
- **REST:** Representational State Transfer
- Web service communication standard
- Uses HTTP requests (GET, POST, PATCH, DELETE)
- Works with JSON or XML data
- Stateless communication
- Platform and language independent

**Why Use SharePoint REST API?**
- Programmatic access to SharePoint data
- Create custom solutions
- Integrate with external applications
- Automate tasks
- Build custom web parts
- Mobile app integration
- Third-party tool integration

**REST vs. Other APIs:**
- **REST API:** Modern, HTTP-based, widely supported
- **CSOM (Client-Side Object Model):** .NET-based, more complex
- **JSOM (JavaScript Object Model):** Legacy, being deprecated
- **Microsoft Graph:** Newer, unified API (covers more than SharePoint)

### REST API Basics

**Endpoint Structure:**
```
https://{site-url}/_api/{service}/{resource}

Examples:
https://contoso.sharepoint.com/sites/team/_api/web
https://contoso.sharepoint.com/sites/team/_api/web/lists
https://contoso.sharepoint.com/sites/team/_api/web/lists/getbytitle('Tasks')
https://contoso.sharepoint.com/sites/team/_api/web/lists/getbytitle('Tasks')/items
```

**Key Endpoints:**
- `/_api/web` - Current web (site)
- `/_api/site` - Site collection
- `/_api/web/lists` - All lists
- `/_api/web/lists/getbytitle('ListName')` - Specific list
- `/_api/web/lists/getbytitle('ListName')/items` - List items
- `/_api/web/currentuser` - Current user info
- `/_api/search/query` - Search queries

**HTTP Methods:**
- **GET:** Read/retrieve data (most common)
- **POST:** Create new items
- **PATCH/MERGE:** Update existing items
- **DELETE:** Remove items
- **PUT:** Replace items (less common)

### Making REST API Calls

**Basic GET Request (JavaScript):**

```javascript
// Get current web title
fetch("/_api/web?$select=Title", {
    method: "GET",
    headers: {
        "Accept": "application/json;odata=verbose"
    }
})
.then(response => response.json())
.then(data => {
    console.log("Site Title:", data.d.Title);
})
.catch(error => console.error("Error:", error));
```

**With jQuery (Legacy):**

```javascript
$.ajax({
    url: "/_api/web?$select=Title",
    method: "GET",
    headers: {
        "Accept": "application/json;odata=verbose"
    },
    success: function(data) {
        console.log("Site Title:", data.d.Title);
    },
    error: function(error) {
        console.error("Error:", error);
    }
});
```

**Understanding Response Format:**

```json
{
    "d": {
        "Title": "My Team Site",
        "__metadata": {
            "id": "https://contoso.sharepoint.com/sites/team/_api/Web",
            "uri": "https://contoso.sharepoint.com/sites/team/_api/Web",
            "type": "SP.Web"
        }
    }
}
```

- Response wrapped in `d` property
- `__metadata` contains type information
- Actual data at same level as metadata

### OData Query Options

**$select - Choose Specific Fields:**
```javascript
// Get only Title and Description
/_api/web?$select=Title,Description

// In list items
/_api/web/lists/getbytitle('Tasks')/items?$select=Title,Status,DueDate
```

**$filter - Filter Results:**
```javascript
// Items where Status equals "Active"
/_api/web/lists/getbytitle('Tasks')/items?$filter=Status eq 'Active'

// Multiple conditions
$filter=Status eq 'Active' and Priority eq 'High'

// Greater than
$filter=DueDate gt '2025-01-01'

// Contains (substringof - legacy)
$filter=substringof('urgent', Title)

// Starts with
$filter=startswith(Title, 'Project')
```

**Filter Operators:**
- `eq` - Equals
- `ne` - Not equals
- `gt` - Greater than
- `ge` - Greater than or equal
- `lt` - Less than
- `le` - Less than or equal
- `and` - Logical AND
- `or` - Logical OR
- `not` - Logical NOT

**$orderby - Sort Results:**
```javascript
// Sort by Title ascending
$orderby=Title

// Sort descending
$orderby=Title desc

// Multiple sort fields
$orderby=Priority desc,DueDate asc
```

**$top - Limit Results:**
```javascript
// Get first 10 items
$top=10

// Get first 5 items sorted by date
$top=5&$orderby=Created desc
```

**$skip - Skip Results (Pagination):**
```javascript
// Skip first 10, get next batch
$skip=10&$top=10

// Pagination example: Page 3, 10 items per page
$skip=20&$top=10
```

**$expand - Include Related Data:**
```javascript
// Expand Author (User field)
$expand=Author&$select=Title,Author/Title,Author/Email

// Multiple expansions
$expand=Author,Editor&$select=Title,Author/Title,Editor/Title
```

**Combining Query Options:**
```javascript
/_api/web/lists/getbytitle('Tasks')/items?
  $select=Title,Status,DueDate,AssignedTo/Title&
  $expand=AssignedTo&
  $filter=Status eq 'Active'&
  $orderby=DueDate asc&
  $top=20
```

### Working with List Items

**Get All Items:**

```javascript
fetch("/_api/web/lists/getbytitle('Tasks')/items", {
    method: "GET",
    headers: {
        "Accept": "application/json;odata=verbose"
    }
})
.then(response => response.json())
.then(data => {
    const items = data.d.results;
    items.forEach(item => {
        console.log(item.Title, item.Status);
    });
})
.catch(error => console.error("Error:", error));
```

**Get Single Item by ID:**

```javascript
const itemId = 5;
fetch(`/_api/web/lists/getbytitle('Tasks')/items(${itemId})`, {
    method: "GET",
    headers: {
        "Accept": "application/json;odata=verbose"
    }
})
.then(response => response.json())
.then(data => {
    const item = data.d;
    console.log(item.Title);
})
.catch(error => console.error("Error:", error));
```

**Create New Item (POST):**

```javascript
// First, get the form digest token
function getFormDigest() {
    return fetch("/_api/contextinfo", {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose"
        }
    })
    .then(response => response.json())
    .then(data => data.d.GetContextWebInformation.FormDigestValue);
}

// Create item
async function createItem() {
    const digest = await getFormDigest();
    
    const itemData = {
        "__metadata": { "type": "SP.Data.TasksListItem" }, // Note: Adjust type name
        "Title": "New Task",
        "Status": "Not Started",
        "Priority": "High"
    };
    
    fetch("/_api/web/lists/getbytitle('Tasks')/items", {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose",
            "Content-Type": "application/json;odata=verbose",
            "X-RequestDigest": digest
        },
        body: JSON.stringify(itemData)
    })
    .then(response => response.json())
    .then(data => {
        console.log("Item created:", data.d.ID);
    })
    .catch(error => console.error("Error:", error));
}
```

**Important Notes for Creating Items:**
- Must include form digest token in `X-RequestDigest` header
- Must specify `__metadata` type
- List item type name format: `SP.Data.{ListName}ListItem`
- If list name has spaces: `SP.Data.My_x0020_ListListItem`

**Update Item (PATCH/MERGE):**

```javascript
async function updateItem(itemId) {
    const digest = await getFormDigest();
    
    const updateData = {
        "__metadata": { "type": "SP.Data.TasksListItem" },
        "Status": "In Progress",
        "PercentComplete": 0.5
    };
    
    fetch(`/_api/web/lists/getbytitle('Tasks')/items(${itemId})`, {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose",
            "Content-Type": "application/json;odata=verbose",
            "X-RequestDigest": digest,
            "IF-MATCH": "*",  // Use * to overwrite regardless of version
            "X-HTTP-Method": "MERGE"  // or "PATCH"
        },
        body: JSON.stringify(updateData)
    })
    .then(response => {
        if (response.ok) {
            console.log("Item updated successfully");
        }
    })
    .catch(error => console.error("Error:", error));
}
```

**Update Notes:**
- Use `X-HTTP-Method: MERGE` or `PATCH`
- Include `IF-MATCH` header (`*` for any version, or specific ETag)
- Only include fields you want to update
- `MERGE` updates specific fields, `PUT` replaces entire item

**Delete Item:**

```javascript
async function deleteItem(itemId) {
    const digest = await getFormDigest();
    
    fetch(`/_api/web/lists/getbytitle('Tasks')/items(${itemId})`, {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose",
            "X-RequestDigest": digest,
            "IF-MATCH": "*",
            "X-HTTP-Method": "DELETE"
        }
    })
    .then(response => {
        if (response.ok) {
            console.log("Item deleted successfully");
        }
    })
    .catch(error => console.error("Error:", error));
}
```

### Working with Different Field Types

**Person/User Fields:**

```javascript
// Get item with user field
$expand=AssignedTo&$select=Title,AssignedTo/Title,AssignedTo/Email

// Create/Update with user field (use user ID)
{
    "AssignedToId": 12  // User's ID from User Information List
}

// Get user ID
/_api/web/siteusers?$filter=Email eq 'user@contoso.com'&$select=Id
```

**Lookup Fields:**

```javascript
// Get item with lookup
$expand=Project&$select=Title,Project/Title

// Create/Update with lookup (use item ID from other list)
{
    "ProjectId": 5  // ID from Projects list
}
```

**Multi-Value Fields:**

```javascript
// Choice (single)
{
    "Status": "In Progress"
}

// Multi-Choice
{
    "Categories": {
        "__metadata": { "type": "Collection(Edm.String)" },
        "results": ["Category1", "Category2"]
    }
}

// Multi-Lookup
{
    "TagsId": {
        "__metadata": { "type": "Collection(Edm.Int32)" },
        "results": [1, 3, 5]  // IDs from Tags list
    }
}
```

**Date Fields:**

```javascript
// ISO 8601 format
{
    "DueDate": "2025-12-31T23:59:59Z"
}

// JavaScript Date to ISO string
{
    "StartDate": new Date().toISOString()
}
```

**Managed Metadata (Taxonomy) Fields:**

```javascript
// Single taxonomy field
{
    "TaxonomyFieldName": {
        "__metadata": { "type": "SP.Taxonomy.TaxonomyFieldValue" },
        "Label": "Term Label",
        "TermGuid": "guid-here",
        "WssId": -1
    }
}

// Requires term store access - complex
```

### Working with Files (Documents)

**Get Files in Library:**

```javascript
fetch("/_api/web/lists/getbytitle('Documents')/items?$select=FileLeafRef,FileRef,File/Length&$expand=File", {
    method: "GET",
    headers: {
        "Accept": "application/json;odata=verbose"
    }
})
.then(response => response.json())
.then(data => {
    const files = data.d.results;
    files.forEach(file => {
        console.log(file.FileLeafRef, file.File.Length);
    });
});
```

**Upload File:**

```javascript
async function uploadFile(fileInput) {
    const digest = await getFormDigest();
    const file = fileInput.files[0];
    const fileName = file.name;
    
    // Read file as array buffer
    const arrayBuffer = await file.arrayBuffer();
    
    fetch(`/_api/web/GetFolderByServerRelativeUrl('/sites/team/Documents')/Files/add(url='${fileName}',overwrite=true)`, {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose",
            "X-RequestDigest": digest
        },
        body: arrayBuffer
    })
    .then(response => response.json())
    .then(data => {
        console.log("File uploaded:", data.d.Name);
        
        // Update file properties
        const itemId = data.d.ListItemAllFields.__deferred.uri;
        return updateFileProperties(itemId);
    })
    .catch(error => console.error("Error:", error));
}

async function updateFileProperties(itemUri) {
    const digest = await getFormDigest();
    
    const metadata = {
        "__metadata": { "type": "SP.Data.DocumentsItem" },
        "Title": "My Document",
        "DocumentType": "Report"
    };
    
    fetch(itemUri, {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose",
            "Content-Type": "application/json;odata=verbose",
            "X-RequestDigest": digest,
            "IF-MATCH": "*",
            "X-HTTP-Method": "MERGE"
        },
        body: JSON.stringify(metadata)
    });
}
```

**Download File:**

```javascript
function downloadFile(serverRelativeUrl) {
    fetch(`/_api/web/GetFileByServerRelativeUrl('${serverRelativeUrl}')/$value`, {
        method: "GET",
        headers: {
            "Accept": "application/json;odata=verbose"
        }
    })
    .then(response => response.blob())
    .then(blob => {
        // Create download link
        const url = window.URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = serverRelativeUrl.split('/').pop();
        document.body.appendChild(a);
        a.click();
        window.URL.revokeObjectURL(url);
    });
}
```

**Delete File:**

```javascript
async function deleteFile(serverRelativeUrl) {
    const digest = await getFormDigest();
    
    fetch(`/_api/web/GetFileByServerRelativeUrl('${serverRelativeUrl}')`, {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose",
            "X-RequestDigest": digest,
            "IF-MATCH": "*",
            "X-HTTP-Method": "DELETE"
        }
    })
    .then(response => {
        if (response.ok) {
            console.log("File deleted");
        }
    });
}
```

### Batch Requests

**Why Use Batch Requests?**
- Multiple operations in single HTTP request
- Reduces network overhead
- Improves performance
- All operations succeed or fail together

**Creating Batch Request:**

```javascript
async function batchRequest() {
    const digest = await getFormDigest();
    const batchGuid = generateGuid();
    const changesetGuid = generateGuid();
    
    const batchBody = [
        `--batch_${batchGuid}`,
        `Content-Type: multipart/mixed; boundary="changeset_${changesetGuid}"`,
        '',
        `--changeset_${changesetGuid}`,
        'Content-Type: application/http',
        'Content-Transfer-Encoding: binary',
        '',
        'POST /_api/web/lists/getbytitle(\'Tasks\')/items HTTP/1.1',
        'Content-Type: application/json;odata=verbose',
        '',
        JSON.stringify({
            "__metadata": { "type": "SP.Data.TasksListItem" },
            "Title": "Task 1"
        }),
        '',
        `--changeset_${changesetGuid}`,
        'Content-Type: application/http',
        'Content-Transfer-Encoding: binary',
        '',
        'POST /_api/web/lists/getbytitle(\'Tasks\')/items HTTP/1.1',
        'Content-Type: application/json;odata=verbose',
        '',
        JSON.stringify({
            "__metadata": { "type": "SP.Data.TasksListItem" },
            "Title": "Task 2"
        }),
        '',
        `--changeset_${changesetGuid}--`,
        '',
        `--batch_${batchGuid}--`
    ].join('\r\n');
    
    fetch('/_api/$batch', {
        method: 'POST',
        headers: {
            'Accept': 'application/json;odata=verbose',
            'Content-Type': `multipart/mixed; boundary="batch_${batchGuid}"`,
            'X-RequestDigest': digest
        },
        body: batchBody
    })
    .then(response => response.text())
    .then(data => {
        console.log("Batch completed:", data);
    });
}

function generateGuid() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        const r = Math.random() * 16 | 0;
        const v = c === 'x' ? r : (r & 0x3 | 0x8);
        return v.toString(16);
    });
}
```

### Error Handling

**Comprehensive Error Handling:**

```javascript
async function robustFetch(url, options = {}) {
    try {
        const response = await fetch(url, options);
        
        if (!response.ok) {
            // Handle HTTP errors
            const errorText = await response.text();
            let errorMessage;
            
            try {
                const errorJson = JSON.parse(errorText);
                errorMessage = errorJson.error.message.value;
            } catch {
                errorMessage = errorText;
            }
            
            throw new Error(`HTTP ${response.status}: ${errorMessage}`);
        }
        
        // Check if response has content
        const contentType = response.headers.get("content-type");
        if (contentType && contentType.includes("application/json")) {
            return await response.json();
        } else {
            return await response.text();
        }
        
    } catch (error) {
        console.error("Request failed:", error);
        
        // Handle specific error types
        if (error.message.includes("404")) {
            console.error("Resource not found");
        } else if (error.message.includes("401")) {
            console.error("Unauthorized - check permissions");
        } else if (error.message.includes("403")) {
            console.error("Forbidden - insufficient permissions");
        } else if (error.message.includes("500")) {
            console.error("Server error");
        }
        
        throw error;
    }
}

// Usage
robustFetch("/_api/web/lists/getbytitle('Tasks')/items")
    .then(data => {
        console.log("Success:", data.d.results);
    })
    .catch(error => {
        // Already logged, handle in UI
        alert("Failed to load items: " + error.message);
    });
```

**Common Error Codes:**
- **400 Bad Request:** Invalid syntax
- **401 Unauthorized:** Not authenticated
- **403 Forbidden:** No permission
- **404 Not Found:** Resource doesn't exist
- **409 Conflict:** Version mismatch
- **500 Internal Server Error:** Server-side error

### REST API Best Practices

**1. Use $select to Limit Data:**
```javascript
// Bad - retrieves all fields
/_api/web/lists/getbytitle('Tasks')/items

// Good - only needed fields
/_api/web/lists/getbytitle('Tasks')/items?$select=Title,Status,DueDate
```

**2. Cache Form Digest:**
```javascript
let cachedDigest = null;
let digestExpiry = null;

async function getFormDigest() {
    if (cachedDigest && digestExpiry && new Date() < digestExpiry) {
        return cachedDigest;
    }
    
    const response = await fetch("/_api/contextinfo", {
        method: "POST",
        headers: { "Accept": "application/json;odata=verbose" }
    });
    
    const data = await response.json();
    cachedDigest = data.d.GetContextWebInformation.FormDigestValue;
    
    // Digest valid for ~30 minutes, cache for 25
    digestExpiry = new Date(new Date().getTime() + 25 * 60 * 1000);
    
    return cachedDigest;
}
```

**3. Handle Pagination for Large Lists:**
```javascript
async function getAllItems(listName) {
    let allItems = [];
    let nextUrl = `/_api/web/lists/getbytitle('${listName}')/items?$top=1000`;
    
    while (nextUrl) {
        const response = await fetch(nextUrl, {
            headers: { "Accept": "application/json;odata=verbose" }
        });
        
        const data = await response.json();
        allItems = allItems.concat(data.d.results);
        
        // Check for next page
        nextUrl = data.d.__next || null;
    }
    
    return allItems;
}
```

**4. Use Specific Queries:**
```javascript
// Bad - get all then filter in JavaScript
const allItems = await getAllItems('Tasks');
const activeItems = allItems.filter(item => item.Status === 'Active');

// Good - filter on server
const activeItems = await fetch(
    "/_api/web/lists/getbytitle('Tasks')/items?$filter=Status eq 'Active'"
);
```

**5. Reuse Functions:**
```javascript
// Create reusable API wrapper
class SharePointAPI {
    constructor(siteUrl) {
        this.siteUrl = siteUrl || _spPageContextInfo.webAbsoluteUrl;
        this.digest = null;
    }
    
    async getDigest() {
        // Cached digest logic
    }
    
    async getItems(listName, query = '') {
        const url = `${this.siteUrl}/_api/web/lists/getbytitle('${listName}')/items${query}`;
        const response = await fetch(url, {
            headers: { "Accept": "application/json;odata=verbose" }
        });
        const data = await response.json();
        return data.d.results;
    }
    
    async createItem(listName, itemData) {
        // Implementation
    }
    
    async updateItem(listName, itemId, itemData) {
        // Implementation
    }
    
    async deleteItem(listName, itemId) {
        // Implementation
    }
}

// Usage
const sp = new SharePointAPI();
const tasks = await sp.getItems('Tasks', '?$filter=Status eq \'Active\'');
```

### Hands-On Exercise Day 22

**Exercise 1: Basic GET Requests**
1. Open browser developer console (F12) on SharePoint site
2. Execute these requests and examine responses:
```javascript
// Get web info
fetch("/_api/web?$select=Title,Url,Created")
    .then(r => r.json())
    .then(d => console.log(d));

// Get all lists
fetch("/_api/web/lists?$select=Title,ItemCount")
    .then(r => r.json())
    .then(d => console.log(d.d.results));

// Get current user
fetch("/_api/web/currentuser")
    .then(r => r.json())
    .then(d => console.log(d));
```

**Exercise 2: Query List Items**
1. Create test list "API Test" with columns: Title, Status (Choice), Priority (Choice), DueDate (Date)
2. Add 10 sample items
3. Practice queries:
```javascript
// All items
fetch("/_api/web/lists/getbytitle('API Test')/items")
    .then(r => r.json())
    .then(d => console.table(d.d.results));

// High priority only
fetch("/_api/web/lists/getbytitle('API Test')/items?$filter=Priority eq 'High'")
    .then(r => r.json())
    .then(d => console.table(d.d.results));

// Active and high priority
fetch("/_api/web/lists/getbytitle('API Test')/items?$filter=Status eq 'Active' and Priority eq 'High'")
    .then(r => r.json())
    .then(d => console.table(d.d.results));

// Sort by due date
fetch("/_api/web/lists/getbytitle('API Test')/items?$orderby=DueDate desc")
    .then(r => r.json())
    .then(d => console.table(d.d.results));

// Select specific fields
fetch("/_api/web/lists/getbytitle('API Test')/items?$select=Title,Status,DueDate")
    .then(r => r.json())
    .then(d => console.table(d.d.results));
```

**Exercise 3: Create, Update, Delete**
Create helper functions and test CRUD operations:

```javascript
// Helper to get digest
async function getDigest() {
    const response = await fetch("/_api/contextinfo", {
        method: "POST",
        headers: { "Accept": "application/json;odata=verbose" }
    });
    const data = await response.json();
    return data.d.GetContextWebInformation.FormDigestValue;
}

// Create item
async function createTestItem() {
    const digest = await getDigest();
    const itemData = {
        "__metadata": { "type": "SP.Data.API_x0020_TestListItem" },
        "Title": "API Created Item",
        "Status": "Active",
        "Priority": "High",
        "DueDate": new Date().toISOString()
    };
    
    const response = await fetch("/_api/web/lists/getbytitle('API Test')/items", {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose",
            "Content-Type": "application/json;odata=verbose",
            "X-RequestDigest": digest
        },
        body: JSON.stringify(itemData)
    });
    
    const data = await response.json();
    console.log("Created item ID:", data.d.ID);
    return data.d.ID;
}

// Update item
async function updateTestItem(itemId) {
    const digest = await getDigest();
    const updateData = {
        "__metadata": { "type": "SP.Data.API_x0020_TestListItem" },
        "Status": "Complete"
    };
    
    await fetch(`/_api/web/lists/getbytitle('API Test')/items(${itemId})`, {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose",
            "Content-Type": "application/json;odata=verbose",
            "X-RequestDigest": digest,
            "IF-MATCH": "*",
            "X-HTTP-Method": "MERGE"
        },
        body: JSON.stringify(updateData)
    });
    
    console.log("Updated item:", itemId);
}

// Delete item
async function deleteTestItem(itemId) {
    const digest = await getDigest();
    
    await fetch(`/_api/web/lists/getbytitle('API Test')/items(${itemId})`, {
        method: "POST",
        headers: {
            "Accept": "application/json;odata=verbose",
            "X-RequestDigest": digest,
            "IF-MATCH": "*",
            "X-HTTP-Method": "DELETE"
        }
    });
    
    console.log("Deleted item:", itemId);
}

// Test sequence
async function testCRUD() {
    const id = await createTestItem();
    await new Promise(resolve => setTimeout(resolve, 1000));
    await updateTestItem(id);
    await new Promise(resolve => setTimeout(resolve, 1000));
    await deleteTestItem(id);
}

testCRUD();
```

**Exercise 4: Build Simple Dashboard**
Create an HTML page that displays SharePoint data:

```html
<!DOCTYPE html>
<html>
<head>
    <title>SharePoint REST Dashboard</title>
    <style>
        body { font-family: Arial; margin: 20px; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #0078d4; color: white; }
        .high { color: red; font-weight: bold; }
        .medium { color: orange; }
        .low { color: green; }
    </style>
</head>
<body>
    <h1>Task Dashboard</h1>
    <div>
        <label>Filter: </label>
        <select id="statusFilter">
            <option value="">All</option>
            <option value="Active">Active</option>
            <option value="Complete">Complete</option>
        </select>
        <button onclick="loadTasks()">Refresh</button>
    </div>
    <table id="tasksTable">
        <thead>
            <tr>
                <th>Title</th>
                <th>Status</th>
                <th>Priority</th>
                <th>Due Date</th>
            </tr>
        </thead>
        <tbody id="tasksBody">
        </tbody>
    </table>
    
    <script>
        async function loadTasks() {
            const status = document.getElementById('statusFilter').value;
            let url = "/_api/web/lists/getbytitle('API Test')/items?$select=Title,Status,Priority,DueDate&$orderby=DueDate";
            
            if (status) {
                url += `&$filter=Status eq '${status}'`;
            }
            
            try {
                const response = await fetch(url, {
                    headers: { "Accept": "application/json;odata=verbose" }
                });
                const data = await response.json();
                displayTasks(data.d.results);
            } catch (error) {
                console.error("Error:", error);
                alert("Failed to load tasks");
            }
        }
        
        function displayTasks(tasks) {
            const tbody = document.getElementById('tasksBody');
            tbody.innerHTML = '';
