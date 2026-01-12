# 024 Send HTTP Request to SharePoint

Canonical documentation for 024 Send HTTP Request to SharePoint. This document defines concepts, terminology, and standard usage.

## Purpose
The "Send HTTP Request to SharePoint" protocol exists to provide a standardized interface for interacting with the SharePoint REST API and OData services when high-level, abstracted actions are insufficient. It addresses the need for granular control over SharePoint objects, metadata, and configurations that are not exposed through simplified graphical user interfaces or standard connector sets. 

This mechanism allows for the execution of complex queries, the manipulation of security scopes, and the management of site structures by communicating directly with the SharePoint service layer via the Hypertext Transfer Protocol (HTTP).

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   The structural requirements of an HTTP request directed at SharePoint resources.
*   The relationship between HTTP methods and SharePoint CRUD (Create, Read, Update, Delete) operations.
*   Standard header configurations and payload structures.
*   The conceptual framework of the SharePoint REST API (`_api`) namespace.

**Out of scope:**
*   Specific vendor-specific workflow engine configurations (e.g., Power Automate, Logic Apps, Nintex).
*   Client-side library implementations (e.g., PnP JS, SPFX) except where they illustrate underlying HTTP principles.
*   Network-level troubleshooting (DNS, Firewalls).

## Definitions
| Term | Definition |
|------|------------|
| **Endpoint** | The specific URI (Uniform Resource Identifier) representing a SharePoint resource (e.g., a List, Web, or User). |
| **OData** | Open Data Protocol; the standard used by SharePoint to define how to query and update data via REST. |
| **Verb/Method** | The HTTP action being performed (GET, POST, PUT, PATCH, DELETE, MERGE). |
| **Payload** | The data sent in the body of a POST, PUT, or PATCH request, typically formatted as JSON. |
| **Internal Name** | The immutable, underlying name of a SharePoint column or object, distinct from its Display Name. |
| **Form Digest** | A security token used in specific authentication contexts to prevent Cross-Site Request Forgery (CSRF). |

## Core Concepts
### The REST API Architecture
SharePoint exposes its functionality through a Representational State Transfer (REST) service. Every addressable object in SharePoint (Sites, Webs, Lists, Items, Folders) is mapped to a specific URI. By navigating the `_api` or `_api/web` namespace, callers can traverse the SharePoint hierarchy.

### Statelessness and Authentication
Each HTTP request to SharePoint must be self-contained. It must include the necessary authorization context (typically via an OAuth 2.0 Bearer token or a validated session cookie) to verify the identity and permissions of the requester.

### Resource Addressing
Resources are addressed using a hierarchical path. The standard entry point is the Site or Subsite URL followed by the `/_api/` prefix. 
*   Example: `https://tenant.sharepoint.com/sites/finance/_api/web/lists`

## Standard Model
The standard model for sending an HTTP request to SharePoint follows a four-part structure:

1.  **The Method:**
    *   `GET`: Retrieve data without modification.
    *   `POST`: Create new resources or execute complex operations (like breaking permission inheritance).
    *   `PATCH/MERGE`: Update existing resources by changing only specified properties.
    *   `DELETE`: Remove a resource.
2.  **The URI:** A fully qualified path to the resource, including OData query parameters (e.g., `$select`, `$filter`, `$expand`).
3.  **Headers:** Metadata about the request.
    *   `Accept`: Defines the format of the data the caller expects (usually `application/json;odata=verbose` or `application/json;odata=nometadata`).
    *   `Content-Type`: Defines the format of the payload being sent.
    *   `IF-MATCH`: Used for concurrency control (ETags).
    *   `X-HTTP-Method`: Used to tunnel specific verbs over POST in restricted environments.
4.  **The Body:** The JSON-formatted data required for the operation.

## Common Patterns
### The "Expand" Pattern
Used to retrieve related data in a single request. Instead of making two calls (one for a list item and one for its author), the `$expand` parameter allows the server to join these entities and return them in one response.

### The "Batch" Pattern
To optimize performance and stay within rate limits, multiple HTTP requests can be grouped into a single `POST` request to the `_api/$batch` endpoint. This reduces round-trips between the caller and the SharePoint server.

### The "ValidateUpdateListItem" Pattern
A specific POST operation used to update list item metadata without creating new file versions or changing the "Modified By" system fields, often used in data migration or governance scenarios.

## Anti-Patterns
*   **Over-fetching:** Requesting all columns (`$select=*`) instead of specific fields. This increases latency and resource consumption.
*   **Hardcoding IDs:** Using GUIDs or Integer IDs in URIs that are specific to a single environment (e.g., "Dev") which will fail in "Production."
*   **Ignoring Throttling:** Failing to monitor for HTTP 429 (Too Many Requests) or 503 (Service Unavailable) status codes and failing to implement a "retry-after" logic.
*   **Synchronous Chaining:** Making multiple sequential GET requests to find a single item instead of using a single filtered query.

## Edge Cases
*   **Large File Handling:** Standard HTTP POST requests have size limits. Files exceeding these limits must be uploaded using "StartUpload," "ContinueUpload," and "FinishUpload" methods.
*   **Special Characters in URIs:** SharePoint internal names or folder paths containing characters like `#`, `%`, or `&` require specific encoding or the use of "Parameter Aliasing" to avoid URI malformation.
*   **Hidden Lists:** Certain system lists (e.g., User Information List) require specific endpoint paths that do not follow the standard `/lists/getbytitle` convention.
*   **Schema Changes:** Sending a request to update a field that has been deleted or renamed will result in a 400 Bad Request, requiring the caller to handle schema drift.

## Related Topics
*   **012 SharePoint REST API Fundamentals:** The underlying service architecture.
*   **045 OAuth 2.0 Authentication Flows:** The mechanism for securing HTTP requests.
*   **088 OData Query Syntax:** The language used for filtering and selecting data.
*   **102 SharePoint Throttling and Limits:** Understanding service boundaries.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |