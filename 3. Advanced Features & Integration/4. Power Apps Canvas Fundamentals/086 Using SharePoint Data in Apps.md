# 086 Using SharePoint Data in Apps

Canonical documentation for 086 Using SharePoint Data in Apps. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of using SharePoint data in applications is to leverage an existing collaborative content management infrastructure as a structured data source. This approach addresses the need for rapid application development where the data layer requires built-in versioning, granular security inheritance, and seamless integration with document management workflows without the overhead of provisioning dedicated relational database management systems (RDBMS).

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   The conceptual framework for interacting with SharePoint Lists and Libraries as data entities.
*   Data retrieval, manipulation, and synchronization strategies.
*   Security and permission modeling at the data layer.
*   Constraints inherent to collaborative platforms used as backends.

**Out of scope:**
*   Specific programming language syntax (e.g., C#, JavaScript, Power Fx).
*   Vendor-specific UI components or "widgets."
*   Server-side installation or infrastructure maintenance of the SharePoint environment itself.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **List** | A collection of data organized in rows (items) and columns (fields), similar to a table in a database. |
| **Library** | A specialized list designed for storing files (blobs) along with associated metadata. |
| **Item** | A single record within a List. |
| **Field/Column** | A specific attribute or data type defined within a List or Library. |
| **Delegation** | The process where the data source performs the processing of a query (filtering, sorting) rather than the client application. |
| **Throttling** | A performance-limiting mechanism enforced by the host to prevent resource exhaustion during high-volume API calls. |
| **Lookup Field** | A field type that creates a relationship by pulling data from another list on the same site. |
| **View Threshold** | A limit on the number of items that can be processed in a single operation to maintain performance. |

## Core Concepts

### 1. Data as Content
Unlike traditional databases, SharePoint treats data as "Content." Every record is inherently tied to a content type and includes system-generated metadata (Created, Modified, Author, Editor) by default.

### 2. Schema Flexibility
SharePoint allows for "Schema-on-Write" capabilities where users can add columns dynamically. Applications consuming this data must be resilient to schema drift or implement strict governance over the underlying list structure.

### 3. Security Inheritance
Security is managed at the Site, List, or Item level. Applications typically inherit the context of the logged-in user, meaning the data layer enforces security before the application layer receives the payload.

### 4. The API Surface
Interaction with SharePoint data occurs through standardized interfaces (REST, Graph, or SOAP). These interfaces abstract the underlying SQL storage, providing a logical representation of the content.

## Standard Model
The standard model for using SharePoint data in apps follows a **Decoupled Consumer Pattern**:

1.  **Data Source:** A SharePoint List or Library acts as the "Source of Truth."
2.  **Connectivity Layer:** An API or Connector facilitates the handshake between the app and the source, handling authentication (OAuth2/OpenID Connect).
3.  **Data Mapping:** The application maps SharePoint fields (Internal Names) to local application variables or objects.
4.  **State Management:** The application maintains a local cache or state of the data, synchronizing changes back to the source via PATCH or POST operations.

## Common Patterns

### The "Sidecar" Metadata Pattern
Storing binary files in a Library while maintaining complex relational metadata in a separate List. The two are linked via a unique identifier (ID) or GUID.

### The Caching Proxy
To circumvent throttling and threshold limits, applications often implement a middle-tier cache (e.g., Redis or a local collection) that periodically syncs with SharePoint rather than querying it on every user action.

### Delegation-First Querying
Designing queries specifically to be "delegable." This ensures that filtering and sorting happen on the server side, allowing the application to handle datasets that exceed the local record limit.

## Anti-Patterns

### Database Mimicry (The "SQL-in-SP" Trap)
Attempting to build complex many-to-many relationships using SharePoint Lists. SharePoint is optimized for flat or shallowly hierarchical data; forcing deep relational integrity often leads to catastrophic performance degradation.

### Deep Folder Nesting for Data Organization
Using folders to categorize data items within a list. This complicates API queries and often leads to issues with URL length limits and indexing. Metadata (columns) should be used for categorization instead of physical folders.

### Polling for Changes
Continuously querying the entire list to check for updates. This triggers throttling. The preferred approach is using Webhooks or Change Tokens.

### Using "Display Names" in Code
Hard-coding logic based on the visible name of a column. Display names can be changed by end-users; applications should always reference the **Internal Name** of a field.

## Edge Cases

*   **The 5,000 Item Threshold:** When a list exceeds 5,000 items, non-indexed queries will fail. Applications must ensure that any field used for filtering is explicitly indexed in the SharePoint settings.
*   **Multi-Value Fields:** Fields that allow multiple selections (e.g., Person or Choice) return complex objects/arrays, which can complicate data flattening and reporting.
*   **System Account Context:** When an app runs under a "Service Principal" or "App-Only" context, system fields like "Modified By" will reflect the app identity rather than the human user, potentially breaking audit trails.
*   **Field Deletion Recovery:** If a column is deleted in SharePoint, the API payload simply omits it, which may cause "Null Reference" errors in strictly typed applications.

## Related Topics
*   **042 API Authentication Standards:** Understanding OAuth2 flows for data access.
*   **105 Data Normalization:** Principles of structuring data (and when to deviate for SharePoint).
*   **210 Webhooks and Event-Driven Architecture:** Handling real-time updates from data sources.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |