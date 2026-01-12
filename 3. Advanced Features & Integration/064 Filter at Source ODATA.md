# 064 Filter at Source ODATA

Canonical documentation for 064 Filter at Source ODATA. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of "Filter at Source ODATA" is to optimize data retrieval by delegating data selection logic to the source system rather than the consuming application. In high-volume data environments, transferring entire datasets to a client for local filtering creates significant overhead in network bandwidth, memory consumption, and processing time. By utilizing the OData protocol's native filtering capabilities, systems can ensure that only the necessary subset of data is transmitted across the wire.

This topic addresses the problem of "Over-fetching," where the discrepancy between the data requested and the data required leads to system latency and potential timeouts.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   The theoretical framework of query delegation via the OData protocol.
*   The mechanics of the `$filter` system query option.
*   Performance implications of server-side vs. client-side processing.
*   Standard logical operators and functions within the OData filtering context.

**Out of scope:**
*   Specific vendor implementations (e.g., SAP Gateway, Microsoft Dynamics, Salesforce OData adapters).
*   Authentication and authorization mechanisms for OData services.
*   Database-specific indexing strategies (though acknowledged as a dependency).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **OData (Open Data Protocol)** | An open protocol that allows the creation and consumption of queryable and interoperable RESTful APIs. |
| **Query Delegation** | The process of passing a data query from a client to a server so the server performs the computation. |
| **Predicate** | A logical expression that evaluates to true or false, used to determine which records should be returned. |
| **Payload** | The actual data transmitted in the body of an HTTP response. |
| **Server-Side Filtering** | The execution of filter logic within the source system's infrastructure before data serialization. |
| **Client-Side Filtering** | The process of receiving a full dataset and applying filters within the consuming application's memory. |

## Core Concepts

### Query Delegation (Push-down)
The fundamental concept of Filter at Source is "pushing down" the logic. Instead of the consumer acting as the engine for data refinement, the consumer sends a declarative statement of its needs. The source system translates this statement into its native query language (e.g., SQL) and executes it against its data store.

### The $filter System Query Option
In the OData standard, the `$filter` parameter is the primary mechanism for source-side filtering. It is a URI-based query option that allows clients to specify a boolean expression. Only entities that satisfy the expression are returned in the response.

### Resource Optimization
Filtering at the source minimizes the "Data Gravity" problem. By reducing the volume of data leaving the source, the system reduces:
1.  **Serialization Time:** The time the server spends converting data to JSON/XML.
2.  **Network Latency:** The time spent moving bits across the network.
3.  **Deserialization Time:** The time the client spends parsing the response.

## Standard Model
The standard model for Filter at Source ODATA follows the Request-Response pattern using specific URI syntax:

1.  **The Request:** The client constructs a GET request appended with the `$filter` parameter.
    *   *Syntax:* `GET [ServiceRoot]/[EntitySet]?$filter=[Property] [Operator] [Value]`
2.  **The Translation:** The OData producer parses the `$filter` string.
3.  **The Execution:** The producer converts the OData expression into a database query.
4.  **The Response:** The producer returns an HTTP 200 OK with a payload containing only the matching records.

### Standard Operators
*   **Logical:** `eq` (Equal), `ne` (Not Equal), `gt` (Greater Than), `lt` (Less Than), `and`, `or`, `not`.
*   **Arithmetic:** `add`, `sub`, `mul`, `div`, `mod`.
*   **String Functions:** `contains`, `startswith`, `endswith`.

## Common Patterns

### Dynamic Predicate Construction
Applications build filter strings dynamically based on user input in a UI. For example, a search bar might generate a `$filter=contains(Name, 'search_term')` query.

### Date Range Filtering
A common pattern for incremental data synchronization where the client requests records modified after a specific timestamp:
`$filter=LastModifiedDate gt 2023-01-01T00:00:00Z`

### Combined Operations
Combining filtering with other OData parameters to further reduce payload:
*   `$filter` + `$select`: Only specific rows AND only specific columns.
*   `$filter` + `$top`: Only the first N records that match the criteria.

## Anti-Patterns

### The "Select All" Fallacy
Fetching the entire entity set (`GET /Products`) and using a programming language (e.g., JavaScript `filter()` or C# `Where()`) to refine the data. This negates the performance benefits of the OData protocol.

### Over-Complex Predicates
Constructing deeply nested or excessively long filter strings that exceed the maximum URI length (typically 2,048 to 8,192 characters). This can lead to HTTP 414 (URI Too Long) errors.

### Filtering on Non-Indexed Fields
Requesting filters on properties that are not indexed in the underlying source database. While technically valid OData, this causes "Full Table Scans" at the source, leading to severe performance degradation.

## Edge Cases

### Case Sensitivity
The OData specification's stance on case sensitivity can vary by version and implementation. Some sources treat `eq 'Value'` and `eq 'value'` as identical, while others do not. This often depends on the underlying database collation.

### Null Handling
Filtering for null values (`$filter=Property eq null`) may behave differently across systems. Some systems treat an empty string and a null value as equivalent, while others maintain a strict distinction.

### Complex Type Filtering
Filtering on properties of nested complex types or navigation properties (e.g., `$filter=Address/City eq 'London'`). Not all OData producers support deep filtering across entity relationships.

## Related Topics
*   **065 Pagination at Source ODATA:** The practice of using `$top` and `$skip` to manage large result sets.
*   **066 Projection at Source ODATA:** The use of `$select` to limit the fields returned.
*   **OData Protocol Specification:** The formal standard maintained by OASIS.
*   **Query Folding:** The process in data integration where transformation steps are converted into a single source query.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |