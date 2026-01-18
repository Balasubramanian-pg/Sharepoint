# [019 Filtering by Strings eq ne and startswith](4. Development & Deployment/REST API Mastery/2. Reading Data and OData Queries/019 Filtering by Strings eq ne and startswith.md)

Canonical documentation for [019 Filtering by Strings eq ne and startswith](4. Development & Deployment/REST API Mastery/2. Reading Data and OData Queries/019 Filtering by Strings eq ne and startswith.md). This document defines concepts, terminology, and standard usage.

## Purpose
String filtering provides a mechanism for restricting a dataset based on the lexical properties of its attributes. By applying logical predicates to string data, systems can isolate specific records, exclude irrelevant data, or facilitate "search-as-you-type" functionality. This topic addresses the fundamental operations of equality, inequality, and prefix matching, which form the baseline for most data retrieval protocols.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logical definitions of `eq` (Equal), `ne` (Not Equal), and `startswith` (Prefix Match).
* Evaluation behavior regarding case sensitivity and nullability.
* Theoretical constraints of string-based predicates.

**Out of scope:**
* Specific programming language syntax (e.g., Python, JavaScript).
* Database-specific dialects (e.g., T-SQL, NoSQL).
* Advanced pattern matching such as Regular Expressions (Regex) or Globbing.
* Full-text search indexing algorithms.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Predicate** | A logical expression that evaluates to true or false for a given record. |
| **Operand** | The data value (attribute) or literal value being compared. |
| **Operator** | The functional symbol or keyword defining the relationship between operands. |
| **Literal** | A fixed string value provided by the consumer to filter against. |
| **Collation** | A set of rules determining how strings are compared and sorted (e.g., case sensitivity, accent marks). |
| **Prefix** | The initial sequence of characters in a string. |

## Core Concepts

### Equality (`eq`)
The `eq` operator evaluates whether the target attribute and the provided literal are identical in sequence, length, and character encoding. In a canonical model, `eq` implies a 1:1 mapping of character codes.

### Inequality (`ne`)
The `ne` operator is the logical inverse of `eq`. It evaluates to true if there is any discrepancy in the character sequence, length, or encoding between the attribute and the literal.

### Prefix Matching (`startswith`)
The `startswith` operator evaluates whether a string begins with a specific sequence of characters. If the literal is an empty string, the operation typically evaluates to true for all non-null strings, as an empty string is a theoretical prefix of all sequences.

### Case Sensitivity and Normalization
String filtering is heavily influenced by the underlying collation. 
* **Case-Sensitive:** "Apple" ≠ "apple".
* **Case-Insensitive:** "Apple" = "apple".
Canonical implementations should explicitly define their default sensitivity to avoid ambiguity.

## Standard Model

The standard model for string filtering follows a ternary logic (True, False, Unknown/Null) or binary logic depending on the system's handling of missing data.

1.  **Evaluation Order:** The system identifies the attribute, retrieves the stored value, and applies the operator against the literal.
2.  **Type Safety:** Both operands must be treated as string types. If a non-string type is compared, the system must either perform a deterministic cast or return an error.
3.  **Result Set:** Only records where the predicate evaluates to `True` are included in the output.

## Common Patterns

### Exact Match Lookup
Using `eq` to retrieve a specific record based on a unique identifier or a known category name.
* *Example:* `status eq 'active'`

### Exclusion Filtering
Using `ne` to remove "noise" or irrelevant categories from a broad dataset.
* *Example:* `type ne 'internal_test'`

### Incremental Search (Type-ahead)
Using `startswith` to provide real-time suggestions as a user inputs text. This is more performant than "contains" because it can leverage standard B-tree indexing.
* *Example:* `city startswith 'San'` (Returns San Francisco, San Diego, etc.)

## Anti-Patterns

### Using `ne` for High-Cardinality Exclusion
Relying on `ne` to filter out the majority of a dataset. This is often inefficient as it forces a full scan of the data rather than utilizing index-based lookups.

### Over-reliance on `startswith` for Substring Search
Attempting to use `startswith` when the user intent is a general search. `startswith` will fail to find "The Apple" if the filter is `startswith 'Apple'`.

### Ignoring Encoding/Normalization
Comparing strings without accounting for Unicode normalization (e.g., `n` with a tilde vs. `n` followed by a combining tilde). This leads to "invisible" mismatches where strings look identical but fail the `eq` test.

## Edge Cases

| Scenario | Expected Behavior |
|----------|-------------------|
| **Null Values** | Most systems treat `null eq 'value'` as False or Unknown. `null ne 'value'` behavior varies; some systems exclude nulls from `ne` results unless explicitly handled. |
| **Empty Strings** | An empty string `''` is a valid string. `'' eq ''` is True. `'' startswith ''` is True. |
| **Trailing Spaces** | In some standards, trailing spaces are ignored; in others, they are significant. Canonical filtering should treat spaces as significant characters. |
| **Literal longer than Attribute** | If the literal in a `startswith` operation is longer than the attribute being checked, the result is always False. |

## Related Topics
* **020 Filtering by Strings: contains and endswith** - Extension of string matching logic.
* **021 Logical Operators: and, or, not** - Combining multiple string filters.
* **045 Collation and Internationalization** - Deep dive into character comparison rules.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |