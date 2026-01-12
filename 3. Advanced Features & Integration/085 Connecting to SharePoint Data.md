# 085 Connecting to SharePoint Data

Canonical documentation for 085 Connecting to SharePoint Data. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of connecting to SharePoint data is to bridge the gap between collaborative content management environments and external data consumers. SharePoint serves as a hybrid repository for both structured data (Lists) and unstructured data (Document Libraries). Establishing a connection allows external systems—such as business intelligence tools, custom applications, and automation engines—to programmatically retrieve, manipulate, and synchronize data stored within the SharePoint ecosystem.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Data Access Protocols:** The theoretical frameworks for interacting with SharePoint (REST, OData, Graph).
* **Authentication Frameworks:** The conceptual requirements for identity and permission validation.
* **Data Structures:** The hierarchy of data containers within the SharePoint environment.
* **Performance Constraints:** Theoretical limits such as throttling and pagination.

**Out of scope:**
* **Specific Vendor Implementations:** Step-by-step guides for specific third-party tools (e.g., Power BI, Tableau, or specific Python libraries).
* **On-Premises Legacy Configuration:** Specific hardware or server-side IIS configurations for SharePoint Server (pre-cloud).
* **UI Customization:** Front-end design of SharePoint sites.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Tenant** | The dedicated instance of the cloud service and its associated data. |
| **Site Collection** | A top-level container that houses multiple sub-sites with shared administrative settings. |
| **List** | A collection of data organized in rows and columns, similar to a database table. |
| **Library** | A specialized list designed specifically for storing and managing files/documents. |
| **Graph API** | The unified gateway for accessing data across the broader cloud ecosystem, including SharePoint. |
| **REST API** | An architectural style for providing interoperability between computer systems via HTTP. |
| **Throttling** | The intentional slowing or rejection of requests by the server to maintain service stability. |
| **OAuth 2.0** | The industry-standard protocol for authorization used to secure data connections. |

## Core Concepts

### Hierarchical Data Architecture
SharePoint data is organized in a strict hierarchy. To connect to a specific data point, a consumer must resolve the path through the following layers:
1.  **Tenant/Root:** The global entry point.
2.  **Site Collection/Site:** The logical grouping of resources.
3.  **List/Library:** The specific entity containing the data.
4.  **Item/File:** The individual record or binary object.

### Authentication and Authorization
Connections require two distinct validations:
*   **Authentication:** Proving the identity of the requester (User-based or Application-based).
*   **Authorization:** Confirming the requester has the specific "Scope" or "Role" required to access the target site or list (e.g., `Sites.Read.All`).

### Data Types and Metadata
SharePoint data is rarely "flat." It includes:
*   **System Metadata:** Created dates, modified dates, and unique identifiers (GUIDs).
*   **Complex Columns:** Lookup fields, Person/Group fields, and Managed Metadata (Taxonomy) which require additional resolution to retrieve human-readable values.

## Standard Model
The standard model for connecting to SharePoint data follows a decoupled, API-first approach:

1.  **Discovery:** The client identifies the target Site ID and List ID via a discovery service or direct URL parsing.
2.  **Token Acquisition:** The client requests an access token from the Identity Provider (IdP) using registered credentials.
3.  **Request Construction:** The client issues an HTTP request (GET, POST, PATCH) using a standardized protocol (Graph or REST).
4.  **Response Handling:** The server returns a JSON payload. The client must handle pagination (nextLink) if the result set exceeds the default page size.
5.  **Resource Release:** The connection is stateless; however, the client manages the lifecycle of the access token.

## Common Patterns

### The Polling Pattern
The client periodically requests data from a list to check for changes. This is often used when real-time updates are not critical.

### The Webhook Pattern
The client registers a listener URL with SharePoint. When data changes, SharePoint sends an HTTP POST notification to the client, triggering a targeted data retrieval.

### The Delta Query Pattern
The client requests only the changes (deltas) that have occurred since the last synchronization, significantly reducing bandwidth and processing overhead.

### Service Principal Access
Using a "headless" account or application identity rather than a specific user's credentials. This ensures connection stability regardless of individual staff turnover.

## Anti-Patterns

*   **Hardcoding Credentials:** Embedding usernames and passwords directly in connection strings or source code.
*   **Over-fetching:** Requesting all columns (`SELECT *`) instead of specifying only the required fields, which increases latency and server load.
*   **Ignoring Throttling Headers:** Failing to implement "Retry-After" logic when the server signals it is overwhelmed.
*   **Direct Database Access:** Attempting to query the underlying SQL databases of SharePoint (in on-premises scenarios), which bypasses security and integrity logic.
*   **Synchronous Large-Scale Transfers:** Attempting to pull millions of rows in a single synchronous request without pagination.

## Edge Cases

*   **The 5,000 Item Limit:** While SharePoint can store millions of items, views and queries on non-indexed columns often fail once a list exceeds 5,000 items (the "List View Threshold").
*   **Multi-Geo Tenants:** Data residency requirements may mean a single tenant has sites hosted in different global regions, requiring the connection to resolve regional endpoints.
*   **Item-Level Permissions:** A user may have access to a List, but not to specific items within that list. Connections must be robust enough to handle "Partial Success" or "Access Denied" errors within a single result set.
*   **Hidden Lists:** Certain system-critical data is stored in hidden lists that are not discoverable via the standard UI but are accessible via direct API calls.

## Related Topics
*   **086 Data Virtualization:** Techniques for viewing SharePoint data without moving it.
*   **042 OAuth 2.0 Implementation:** Detailed standards for the authorization layer.
*   **110 API Rate Limiting:** General principles of handling throttled connections.
*   **201 Document Metadata Standards:** How to structure columns for optimal retrieval.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |