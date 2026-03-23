# [016 GET Requests Retrieving List Collections](4. Development & Deployment/REST API Mastery/2. Reading Data and OData Queries/016 GET Requests Retrieving List Collections.md)

Canonical documentation for [016 GET Requests Retrieving List Collections](4. Development & Deployment/REST API Mastery/2. Reading Data and OData Queries/016 GET Requests Retrieving List Collections.md). This document defines concepts, terminology, and standard usage.

## Purpose
The retrieval of list collections via GET requests provides a standardized mechanism for clients to discover and consume sets of resources. This topic addresses the complexities of data density, network efficiency, and server-side performance by defining how multiple resources of the same type should be requested, represented, and navigated. It ensures that large datasets can be accessed in a predictable, performant, and scalable manner.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Request semantics for collection-level resources.
* Standardized query parameters for data manipulation (filtering, sorting, pagination).
* Response structures for multi-resource payloads.
* Metadata requirements for collection navigation.

**Out of scope:**
* Specific database query languages (SQL, NoSQL).
* Authentication and authorization headers.
* Retrieval of single resources by unique identifier (see Topic 015).
* State-changing operations (POST, PUT, DELETE).

## Definitions
| Term | Definition |
|------|------------|
| Collection | A grouping of resources of the same type, often represented as a top-level URI segment. |
| Resource | An individual entity within a collection that possesses a unique identity. |
| Pagination | The process of dividing a large collection into discrete, manageable subsets (pages). |
| Filtering | The application of criteria to a request to restrict the resources returned in the collection. |
| Sorting | The ordering of resources within a collection based on one or more attributes. |
| Projection | The selection of specific fields or attributes to be included in the response, rather than the full resource representation. |
| Idempotency | A property of GET requests where multiple identical requests have the same effect as a single request, returning the same state (assuming no external state changes). |

## Core Concepts
### Collection Semantics
A collection is viewed as a directory or a container. A GET request to a collection URI (e.g., `/resources`) implies a request for all members of that container that the requester is authorized to see.

### Statelessness and Idempotency
GET requests for collections must be stateless. The server should not be required to maintain session state to deliver the next "page" of results. Furthermore, the request must be idempotent; it must not modify the state of the resources on the server.

### Partial Responses
Because collections can be massive, the core concept of "List Retrieval" relies on the ability to return partial representations. This is achieved through pagination and projection to prevent "over-fetching" (retrieving more data than needed) and "under-fetching" (requiring multiple calls to get necessary data).

## Standard Model
The standard model for retrieving list collections involves a structured URI and a predictable response envelope.

### The Request Structure
Requests should utilize the query string to pass modifiers:
*   **Filtering:** `?attribute=value` or `?filter=attribute eq 'value'`
*   **Sorting:** `?sort=attribute:asc` or `?orderby=attribute desc`
*   **Pagination:** `?limit=10&offset=20` or `?page=2&size=10`
*   **Projection:** `?fields=id,name,status`

### The Response Structure
A collection response typically follows one of two formats:
1.  **Bare Array:** The response body is a JSON array `[...]`. This is simple but lacks metadata.
2.  **Envelope Object:** The response body is an object containing the collection and metadata.
    *   `data`: The array of resources.
    *   `metadata`: Information about the collection (e.g., `total_count`, `links` for pagination).

## Common Patterns
### Offset-Based Pagination
Uses `limit` (number of items) and `offset` (number of items to skip). 
*   *Pros:* Easy to implement; allows jumping to specific pages.
*   *Cons:* Performance degrades on large offsets; inconsistent if data changes during navigation.

### Cursor-Based Pagination
Uses a `cursor` (a pointer to a specific record, often encoded) to fetch the next set of results.
*   *Pros:* Highly performant for large datasets; stable against insertions/deletions.
*   *Cons:* Cannot jump to a specific page; more complex to implement.

### Logical Filtering Operators
Standardized strings or symbols used to define complex queries within the URI:
*   `eq` (Equals)
*   `ne` (Not Equals)
*   `gt` / `lt` (Greater than / Less than)
*   `in` (Membership in a set)

## Anti-Patterns
*   **Unbounded Collections:** Returning an entire database table in a single GET request without pagination. This leads to memory exhaustion and timeouts.
*   **Tunneling via POST:** Using a POST request to "query" for a list because the filter criteria are complex. This breaks cacheability and violates REST principles.
*   **Inconsistent Key Naming:** Using `count` in one collection and `total_records` in another.
*   **Leaking Internal Logic:** Exposing raw database column names or internal IDs in the query parameters or response.

## Edge Cases
*   **Empty Collections:** An empty collection should return a `200 OK` with an empty array (`[]`), not a `404 Not Found`. A `404` implies the endpoint/resource type does not exist.
*   **Deep Pagination:** When a user requests an offset that exceeds the total count, the system should return an empty list and a `200 OK`, or a `416 Range Not Satisfiable` if using Range headers.
*   **Concurrent Modifications:** If a resource is deleted between the time Page 1 and Page 2 are requested, items may "shift," causing a duplicate or a missed item in offset-based pagination.
*   **Maximum Limit Enforcement:** Servers should enforce a maximum `limit` (e.g., 1000 items) regardless of what the client requests to protect system resources.

## Related Topics
*   **015 GET Requests Retrieving Single Resources:** The counterpart for individual entity access.
*   **022 Caching Strategies for Read Operations:** How to optimize collection retrieval using ETags and Cache-Control.
*   **045 HATEOAS and Discoverability:** Using links within collection metadata to guide client navigation.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |