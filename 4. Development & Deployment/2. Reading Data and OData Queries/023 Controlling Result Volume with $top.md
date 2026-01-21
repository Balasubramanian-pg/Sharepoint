# 023 Controlling Result Volume with $top

Canonical documentation for 023 Controlling Result Volume with $top. This document defines concepts, terminology, and standard usage.

## Purpose
The primary purpose of the `$top` operator is to constrain the cardinality of a result set returned by a data service. In distributed systems and large-scale databases, returning an entire dataset in a single response is often computationally expensive, consumes excessive network bandwidth, and may overwhelm the memory capacity of the requesting client. 

The `$top` mechanism addresses the "unbounded result set" problem by allowing a consumer to specify an upper limit on the number of records retrieved. This facilitates efficient data transfer, improves response latency, and serves as a foundational component for pagination strategies.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The logical definition and behavior of result volume limitation.
* The interaction between `$top` and other query modifiers (e.g., ordering).
* Standard validation rules for numerical constraints.
* Theoretical impact on system performance and resource allocation.

**Out of scope:**
* Specific syntax for SQL (e.g., `LIMIT`, `FETCH FIRST`), NoSQL, or specific API frameworks (e.g., OData, GraphQL) except where used as illustrative examples.
* Physical database indexing strategies.
* Client-side UI implementation details (e.g., CSS for "Load More" buttons).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **$top** | A query parameter or operator used to specify the maximum number of items to be included in a response. |
| **Cardinality** | In this context, the number of elements contained within the returned result set. |
| **Result Set** | The collection of data entities that satisfy the criteria of a query. |
| **Pagination** | The process of dividing a large result set into discrete chunks or "pages." |
| **Deterministic Result** | A result set that is guaranteed to be identical across multiple executions given the same underlying data and query parameters. |
| **Server-side Cap** | A hard limit enforced by the service provider that overrides a requested `$top` value to protect system stability. |

## Core Concepts
The fundamental idea behind `$top` is **subset selection**. When a query is executed, the system identifies all matching records (the "total set"). The `$top` operator truncates this set after the $N^{th}$ record.

### 1. Quantitative Constraint
The value provided to `$top` must be a non-negative integer. It represents a "maximum" rather than a "requirement"; if the total number of matching records is less than the `$top` value, the service returns only the available records without padding.

### 2. Positional Dependency
`$top` is inherently positional. It selects the "first" $N$ records. However, the definition of "first" is arbitrary unless a specific sort order is defined.

### 3. Resource Protection
Beyond user preference, `$top` serves as a defensive mechanism. By limiting the volume of data per request, the system ensures predictable execution times and prevents Denial of Service (DoS) scenarios caused by massive data egress.

## Standard Model
In the standard model for result volume control, the following logic applies:

1.  **Evaluation Order:** The system first applies filters (`$filter`), then determines the sequence (`$orderby`), and finally applies the volume constraint (`$top`).
2.  **Interaction with $skip:** When used for pagination, `$top` (the page size) is used in conjunction with `$skip` (the offset). The formula for the $n^{th}$ page is usually: `skip = (page_number - 1) * top`.
3.  **Response Metadata:** A standard implementation should ideally return metadata alongside the results, indicating whether more data is available (e.g., a `@nextLink` or a `totalCount`).
4.  **Default Behavior:** If `$top` is omitted, the service may either return all records (unbounded) or apply a default server-side limit to ensure performance.

## Common Patterns
*   **Top-N Analysis:** Retrieving the highest or lowest values in a category (e.g., "Top 10 highest-grossing products"). This requires a mandatory `$orderby` clause.
*   **Client-Side Paging:** The client requests a fixed number of records (e.g., `$top=25`) to fill a single page of a data grid.
*   **Infinite Scroll / Load More:** The client initially requests a small subset (e.g., `$top=10`) and requests subsequent subsets as the user interacts with the interface.
*   **Existence Check:** Setting `$top=1` to quickly verify if any records match a specific filter without overhead.

## Anti-Patterns
*   **Unordered Truncation:** Using `$top` without an `$orderby` clause. This leads to non-deterministic results where the "top" records may change between requests even if the data hasn't changed, due to internal database optimization or parallel processing.
*   **Over-fetching:** Setting a `$top` value significantly higher than the client can process or display, leading to wasted bandwidth.
*   **Using $top for Security:** Attempting to use volume control to hide sensitive data. `$top` is a presentation and performance tool, not a security boundary.
*   **Ignoring Server Caps:** Requesting a `$top` value (e.g., 1,000,000) and assuming the server will honor it without checking for truncation or "next page" tokens.

## Edge Cases
*   **$top=0:** Generally valid; the service should return an empty result set with a success code (200 OK), often used to retrieve only metadata or the total count of a collection.
*   **Negative Values:** Should be treated as an invalid request (400 Bad Request).
*   **$top exceeding Total Count:** If `$top=100` but only 5 records exist, the service must return 5 records, not an error.
*   **Large Offsets:** Using a very high `$skip` value with a small `$top` value can still cause performance degradation in some systems, as the engine may still need to scan the skipped records.
*   **Null or Non-Integer Values:** Should result in a validation error.

## Related Topics
*   **024 Offsetting Results with $skip:** The companion operator for pagination.
*   **025 Sorting Results with $orderby:** Essential for making `$top` deterministic.
*   **026 Server-side Paging and Delta Links:** Advanced methods for handling large data volumes.
*   **Data Streaming:** An alternative to volume control for processing large datasets.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |