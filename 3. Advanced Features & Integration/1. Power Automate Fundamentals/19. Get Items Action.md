# 019 Get Items Action

Canonical documentation for 019 Get Items Action. This document defines concepts, terminology, and standard usage.

## Purpose
The **019 Get Items Action** serves as the primary mechanism for the bulk retrieval of discrete data entities from a structured or semi-structured repository. It addresses the requirement for systems to query, filter, and ingest collections of records rather than individual instances. This action is fundamental to data synchronization, reporting, and batch processing workflows, providing a standardized interface for interacting with datasets of varying scales.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   **Query Logic:** The theoretical framework for defining which items are returned.
*   **Pagination and Throttling:** Standards for managing large result sets and resource constraints.
*   **Data Shaping:** The concepts of projection and selection within a retrieval context.
*   **State Management:** The stateless nature of the retrieval request.

**Out of scope:**
*   **Specific Vendor Implementations:** Syntax specific to SQL, NoSQL, or proprietary cloud connectors (e.g., Power Automate, AWS AppFlow).
*   **Authentication Protocols:** The specific handshake methods used to authorize the action.
*   **Physical Storage Layer:** The hardware or specific database engine performance tuning.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Collection** | A grouping of one or more data entities sharing a common schema or type. |
| **Filter Expression** | A logical statement used to include or exclude specific items from the result set based on attribute values. |
| **Pagination** | The process of dividing a large result set into discrete "pages" or chunks to optimize performance and memory usage. |
| **Projection** | The specification of which attributes (fields) should be returned for each item in the collection. |
| **Cursor** | A pointer or token used in pagination to mark the position where the next set of results should begin. |
| **Top/Limit** | A constraint defining the maximum number of items to be returned in a single execution. |

## Core Concepts
The 019 Get Items Action is built upon three fundamental pillars:

1.  **Declarative Retrieval:** The action defines *what* data is needed rather than *how* to fetch it. The underlying system interprets the request parameters to optimize the fetch operation.
2.  **Bounded Result Sets:** To maintain system stability, the action assumes that result sets are finite and, where necessary, constrained by system-level or user-defined limits.
3.  **Schema Consistency:** The action expects that the items returned within a single call adhere to a predictable structure, allowing downstream processes to map data reliably.

## Standard Model
The standard model for a 019 Get Items Action follows a Request-Response pattern:

### 1. The Request (Input)
*   **Source Identifier:** The target table, view, or endpoint.
*   **Filter Criteria:** Boolean logic (e.g., `Status eq 'Active'`).
*   **Ordering:** The sequence in which items are returned (e.g., `CreatedDate DESC`).
*   **Selection:** A list of specific fields to retrieve to minimize payload size.
*   **Pagination Parameters:** `Skip` (offset) or `NextToken` (cursor).

### 2. The Processing
The system evaluates the request against the data source, applies security trimming (ensuring the requester only sees authorized data), and assembles the result set.

### 3. The Response (Output)
*   **Payload:** An array or list of items.
*   **Metadata:** Information about the result set, such as the total count (if requested) and pagination tokens for subsequent calls.

## Common Patterns
*   **The Polling Pattern:** Periodically executing the Get Items action with a filter for `LastModifiedDate > LastRunDate` to detect changes.
*   **The Batch Processing Pattern:** Retrieving a fixed number of items (e.g., 100), processing them, and using a cursor to fetch the next 100 until the collection is exhausted.
*   **The OData Pattern:** Utilizing standardized URL query parameters (`$filter`, `$top`, `$orderby`) to define the retrieval logic.

## Anti-Patterns
*   **The "Select All" Fallacy:** Retrieving all columns and all rows without filters or projections, leading to excessive memory consumption and network latency.
*   **Client-Side Filtering:** Fetching a large dataset and filtering it within the application logic rather than at the source level.
*   **Ignoring Throttling Limits:** Failing to implement retry logic or respect "Rate Limit" headers provided by the data source.
*   **N+1 Retrieval:** Executing a Get Items action to find a list of IDs, and then executing individual "Get Item" (singular) actions for every ID in that list.

## Edge Cases
*   **Empty Result Sets:** The action must return a valid "Success" response with an empty collection rather than a "Not Found" error.
*   **Schema Evolution:** Handling scenarios where items in the collection have inconsistent fields due to versioning or "lazy" migrations.
*   **Concurrent Mutations:** Situations where data is added or deleted while a paginated retrieval is in progress, potentially causing skipped or duplicated items in the result set.
*   **Large Object Handling:** When individual items within the collection exceed the standard buffer size (e.g., items containing large binary blobs).

## Related Topics
*   **020 Get Item (Singular):** Retrieval of a specific entity by a unique identifier.
*   **021 Update Items:** Bulk modification of entities matching a filter.
*   **Data Transformation Patterns:** Methods for mapping retrieved collections to different schemas.
*   **Concurrency Control:** Managing data integrity during high-frequency retrieval and update cycles.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |