# 022 Sorting Data with $orderby

Canonical documentation for 022 Sorting Data with $orderby. This document defines concepts, terminology, and standard usage.

## Purpose
The `$orderby` query transformation exists to provide clients with control over the sequence in which data entities are returned by a service. In stateless distributed systems, the natural order of data is often non-deterministic or optimized for storage efficiency rather than human readability or application logic. The `$orderby` mechanism addresses the need for predictable, repeatable, and meaningful data presentation by allowing the requestor to specify one or more criteria for sorting the result set.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logical syntax and structure of sorting expressions.
* Multi-level sorting precedence and rules.
* Directional modifiers (Ascending/Descending).
* Theoretical behavior of sorting across different data types.

**Out of scope:**
* Specific database engine optimization techniques (e.g., B-tree vs. Hash indexes).
* Vendor-specific syntax variations (e.g., SQL `ORDER BY` vs. NoSQL sort objects).
* Language-specific implementation details (e.g., LINQ, Java Streams).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Sort Expression** | A combination of a property path and an optional direction modifier used to determine the sequence of items. |
| **Ascending (asc)** | A sort order where items are sequenced from the lowest value to the highest value (e.g., A to Z, 1 to 10). |
| **Descending (desc)** | A sort order where items are sequenced from the highest value to the lowest value (e.g., Z to A, 10 to 1). |
| **Primary Sort** | The first criteria applied to a collection; determines the initial sequence. |
| **Secondary Sort** | Subsequent criteria applied only when the preceding sort criteria result in a tie (identical values). |
| **Collation** | The set of rules used to compare characters in a character set for the purpose of sorting. |
| **Stable Sort** | A sorting implementation that preserves the relative order of records with equal keys. |

## Core Concepts

### 1. Directionality
Every sort expression has an inherent direction. If no direction is explicitly provided, the system typically defaults to **Ascending**. Directionality is applied per-property in a multi-sort scenario.

### 2. Property Paths
Sorting is performed against specific properties of an entity. In complex data models, this may include navigation to nested properties (e.g., `Address/City`). The validity of a sort is dependent on the property being "sortable" within the service's schema.

### 3. Precedence
When multiple properties are provided in a single `$orderby` statement, they are evaluated from left to right. The second property only influences the order of items that share an identical value for the first property.

### 4. Data Type Comparisons
Sorting behavior is governed by the underlying data type:
*   **Numeric:** Sorted by mathematical magnitude.
*   **String:** Sorted based on lexicographical order and defined collation.
*   **DateTime:** Sorted chronologically.
*   **Boolean:** Typically treated as discrete values where `false` precedes `true` (or vice versa, depending on implementation).

## Standard Model
The standard model for `$orderby` follows a comma-separated list of expressions. Each expression consists of a property name followed by an optional space and a direction keyword (`asc` or `desc`).

**Syntax Template:**
`$orderby=Property1 [asc|desc], Property2 [asc|desc]`

**Evaluation Logic:**
1.  Identify the collection to be sorted.
2.  Apply the first expression to all items.
3.  For any items where the first expression results in a tie, apply the second expression.
4.  Repeat until all expressions are exhausted or no ties remain.

## Common Patterns

### Single Property Sort
The most frequent use case, where a collection is ordered by a single attribute such as `ReleaseDate` or `LastName`.

### Chronological Sequencing
Using `$orderby` on timestamp properties to create a "feed" or "audit log" view, usually in `desc` order to show the most recent items first.

### Hierarchical Sorting
Sorting by a category first, and then by a name within that category (e.g., `$orderby=Category/Name asc, Price desc`). This groups related items together while maintaining a secondary logic for the groups.

## Anti-Patterns

### Sorting by Unindexed Fields
Requesting a sort on high-cardinality fields that lack supporting indexes. This can lead to "Full Table Scans" and significant performance degradation.

### Over-Sorting
Including unnecessary properties in the `$orderby` clause. Each additional property increases the computational complexity of the request.

### Assuming Default Stability
Relying on a specific order for items with identical sort keys when no secondary sort is provided. Without an explicit secondary sort (usually on a Unique ID), the order of tied items is technically undefined in many systems.

### Client-Side Sorting of Paginated Data
Attempting to sort only the current page of data on the client side rather than requesting a sorted set from the server. This results in "fragmented" sorting where the user cannot see the true global order.

## Edge Cases

### Null Handling
The behavior of `null` values varies. A canonical implementation should define whether `null` is considered the "smallest" possible value (appearing first in `asc`) or the "largest" (appearing last in `asc`). This is often referred to as `Nulls First` or `Nulls Last`.

### Case Sensitivity
In string sorting, the result may differ based on whether the system treats `A` and `a` as identical. Canonical documentation assumes that collation rules (e.g., `en-US`) define this behavior.

### Sorting Complex/Collection Types
Attempting to sort by a property that is itself a collection (e.g., sorting Users by their list of Roles) is generally undefined and should be avoided unless a specific aggregation (like `count`) is applied.

### Internationalization (i18n)
Sorting strings containing accented characters or non-Latin scripts requires specific collation logic to ensure the order matches linguistic expectations (e.g., how `ö` is treated in German vs. Swedish).

## Related Topics
*   **021 Filtering Data with $filter:** Often used in conjunction with sorting to limit the dataset before sequencing.
*   **023 Pagination with $top and $skip:** Sorting is a prerequisite for predictable pagination.
*   **025 Data Modeling and Indexing:** The underlying structure that enables efficient sorting.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |