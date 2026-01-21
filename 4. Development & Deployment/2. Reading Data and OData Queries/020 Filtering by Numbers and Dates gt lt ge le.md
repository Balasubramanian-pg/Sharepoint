# [020 Filtering by Numbers and Dates gt lt ge le](4. Development & Deployment/REST API Mastery/2. Reading Data and OData Queries/020 Filtering by Numbers and Dates gt lt ge le.md)

Canonical documentation for [020 Filtering by Numbers and Dates gt lt ge le](4. Development & Deployment/REST API Mastery/2. Reading Data and OData Queries/020 Filtering by Numbers and Dates gt lt ge le.md). This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of inequality-based filtering is to allow consumers of data to retrieve subsets of information based on quantitative or chronological thresholds. In data systems, exact matches (equality) are often insufficient for analytical or operational requirements. This topic addresses the logic of range-based selection, enabling users to define boundaries for numeric values and temporal points.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
**In scope:**
*   The logical definitions of the four primary inequality operators: `gt`, `lt`, `ge`, and `le`.
*   Application of these operators to numeric data types (integers, decimals, floating points).
*   Application of these operators to temporal data types (dates, timestamps, durations).
*   The conceptual interaction between inclusive and exclusive boundaries.

**Out of scope:**
*   Specific syntax for SQL, NoSQL, or REST frameworks (e.g., OData, GraphQL).
*   Performance optimization or indexing strategies.
*   String-based "alphabetical" comparisons.

## Definitions
| Term | Definition |
|------|------------|
| **gt** (Greater Than) | An exclusive operator that selects values strictly higher than the specified threshold ($x > n$). |
| **lt** (Less Than) | An exclusive operator that selects values strictly lower than the specified threshold ($x < n$). |
| **ge** (Greater Than or Equal) | An inclusive operator that selects values higher than or exactly equal to the specified threshold ($x \ge n$). |
| **le** (Less Than or Equal) | An inclusive operator that selects values lower than or exactly equal to the specified threshold ($x \le n$). |
| **Threshold** | The constant value against which the dataset is compared. |
| **Boundary** | The limit of a range, which may be inclusive (closed) or exclusive (open). |
| **Temporal Precision** | The level of detail in a date/time value (e.g., year, day, millisecond) which affects comparison outcomes. |

## Core Concepts

### Inequality Logic
Inequality filtering operates on the principle of a linear order. For any two distinct values in a set, one must be "greater than" the other. This applies naturally to the number line and the arrow of time.

### Inclusivity vs. Exclusivity
*   **Exclusive (`gt`, `lt`):** The threshold itself is omitted from the results. This is used when the boundary is a limit that should not be reached.
*   **Inclusive (`ge`, `le`):** The threshold is included in the results. This is the standard for most business logic (e.g., "everyone aged 18 and over").

### Temporal Comparison
Dates and timestamps are treated as numeric offsets from a fixed epoch. 
*   `gt` / `ge` refers to events occurring **after** a point in time (future-leaning).
*   `lt` / `le` refers to events occurring **before** a point in time (past-leaning).

## Standard Model

The standard model for inequality filtering follows a triplet structure: `Field`, `Operator`, and `Value`.

1.  **Numeric Comparison:** The value is compared based on its mathematical magnitude.
2.  **Chronological Comparison:** The value is compared based on its position in time. Standard practice dictates the use of ISO 8601 format for date values to ensure unambiguous comparison.
3.  **Range Construction:** A range is defined by combining two operators on the same field.
    *   *Example:* `(price ge 100) AND (price le 200)` defines a closed interval $[100, 200]$.

## Common Patterns

### Range Filtering
The most common application is selecting data within a window.
*   **Closed Interval:** Using `ge` and `le` to include both start and end points.
*   **Half-Open Interval:** Frequently used for time series data where the start is inclusive (`ge`) and the end is exclusive (`lt`). This prevents "double counting" at the boundary of adjacent time buckets (e.g., `[00:00, 01:00)`).

### Pagination (Keyset)
Using `gt` on a unique identifier or timestamp to retrieve the "next" set of records after a known marker, ensuring stable results compared to offset-based pagination.

### Threshold Alerts
Using `gt` or `lt` to trigger logic when a metric crosses a specific critical value (e.g., `temperature gt 100`).

## Anti-Patterns

### Type Mismatching
Attempting to apply numeric operators to non-ordered types (e.g., Booleans or UUIDs) or comparing a string representation of a number (`"10"`) with a numeric type. This leads to unpredictable sorting (e.g., `"10"` being "less than" `"2"` in lexicographical order).

### Inclusive Overlap
Defining adjacent ranges using inclusive operators on both sides (e.g., `Range A: le 100`, `Range B: ge 100`). This causes the value `100` to appear in both subsets, leading to data duplication in reports.

### Ignoring Timezones
Comparing dates/times without a normalized timezone (UTC). This can result in `gt` comparisons returning values that chronologically occurred "before" the threshold but appear "after" due to local clock offsets.

## Edge Cases

### Null/Missing Values
In many systems, `null` represents an unknown value. Standard inequality operators typically return `null` or `false` when comparing against a null value, as it is neither greater than nor less than a known number.

### Precision and Rounding
When using `ge` or `le` with floating-point numbers, infinitesimal precision errors can cause a value that *should* be equal to the threshold to be excluded. 

### Infinity
Representations of positive or negative infinity must be handled. `gt` against positive infinity will always be empty, while `lt` against positive infinity will return all finite values.

### Leap Seconds and Years
In temporal filtering, the system must account for calendar irregularities. A filter for `le 2024-02-29` is valid, whereas the same filter for 2023 is logically impossible and may throw an error or be coerced.

## Related Topics
*   **021 Equality and Negation (eq, ne):** The foundation of discrete filtering.
*   **022 Logical Operators (and, or, not):** Used to combine multiple inequality filters.
*   **025 Null Handling in Filters:** Detailed behavior of operators in the presence of missing data.
*   **ISO 8601 Data Standards:** The authoritative standard for representing dates and times.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |