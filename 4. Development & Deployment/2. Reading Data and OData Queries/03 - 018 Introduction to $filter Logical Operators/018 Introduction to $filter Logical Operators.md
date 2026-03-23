# 018 Introduction to $filter Logical Operators

Canonical documentation for 018 Introduction to $filter Logical Operators. This document defines concepts, terminology, and standard usage.

## Purpose
The `$filter` logical operators provide a standardized mechanism for combining multiple criteria into a single boolean expression. This topic exists to define the formal logic used to include or exclude data points within a collection based on complex, multi-faceted conditions. It addresses the need for precise data subsetting beyond simple equality checks, allowing for the construction of sophisticated queries that reflect real-world business logic.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Formal definitions of logical conjunction, disjunction, and negation within a filtering context.
* Evaluation order and operator precedence.
* The behavior of boolean logic when applied to data predicates.
* Structural requirements for nested logical expressions.

**Out of scope:**
* Specific syntax for OData, MongoDB, SQL, or GraphQL (though concepts apply to all).
* Performance optimization for specific database engines.
* Arithmetic or string-manipulation operators (except where they serve as operands).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Predicate** | A statement or expression that evaluates to a boolean value (True or False) based on the properties of a data object. |
| **Operand** | The input values or expressions upon which a logical operator acts. |
| **Conjunction (AND)** | A logical operation that results in True if and only if all its operands are True. |
| **Disjunction (OR)** | A logical operation that results in True if at least one of its operands is True. |
| **Negation (NOT)** | A unary operation that inverts the boolean value of its operand. |
| **Short-circuiting** | An evaluation strategy where the second argument is executed or evaluated only if the first argument does not suffice to determine the value of the expression. |
| **Three-valued Logic** | A logic system (often used in databases) that includes True, False, and Unknown (Null). |

## Core Concepts

### Boolean Evaluation
At its core, the `$filter` logical operator set treats every sub-expression as a boolean provider. The filter engine iterates through a dataset and applies the logical tree to each record. If the final result of the logical tree is `True`, the record is included in the result set.

### Operator Precedence
In complex expressions involving multiple operators, a standard hierarchy determines the order of evaluation:
1. **Grouping** (Parentheses)
2. **Negation** (NOT)
3. **Conjunction** (AND)
4. **Disjunction** (OR)

### Arity
Logical operators in filtering typically follow standard arity:
* **Unary:** `NOT` (requires one operand).
* **Binary/Variadic:** `AND`, `OR` (requires two or more operands).

## Standard Model

The standard model for logical filtering follows a tree-based structure where logical operators serve as nodes and predicates serve as leaves.

1.  **Expression Tree:** A filter is represented as a directed acyclic graph (DAG).
2.  **Leaf Nodes:** These are comparison expressions (e.g., `Price gt 50`).
3.  **Branch Nodes:** These are logical operators (e.g., `AND`) that join leaf nodes or other branch nodes.
4.  **Root Evaluation:** The final output of the root node determines the inclusion of the data entity.

### Truth Tables (Standard Binary)
| A | B | A AND B | A OR B |
|---|---|---|---|
| T | T | T | T |
| T | F | F | T |
| F | T | F | T |
| F | F | F | F |

## Common Patterns

### Range Filtering
Combining two predicates with an `AND` operator to define a boundary.
* *Pattern:* `(Property >= Min) AND (Property <= Max)`

### Exclusive Selection
Using `OR` to select entities that meet one of several distinct criteria.
* *Pattern:* `(Status == 'Active') OR (Status == 'Pending')`

### Exclusion Logic
Using `NOT` in conjunction with `AND` to filter out specific subsets.
* *Pattern:* `(Category == 'Electronics') AND NOT (Brand == 'Generic')`

### Membership Simulation
Using nested `OR` statements to simulate "In-list" functionality when a dedicated `IN` operator is unavailable.

## Anti-Patterns

### Redundant Logic
Including predicates that are logically covered by other parts of the expression.
* *Example:* `(Age > 18) AND (Age > 21)`. The first predicate is redundant.

### Deep Nesting
Creating excessively deep logical trees that are difficult to parse and maintain. This often indicates a need for computed properties or simplified data modeling.

### Negating Disjunctions (De Morgan's Law Misuse)
Failing to correctly apply De Morgan's laws when refactoring logic. 
* *Incorrect:* `NOT (A OR B)` is often mistakenly treated as `NOT A OR NOT B`, whereas it is logically equivalent to `NOT A AND NOT B`.

## Edge Cases

### Null/Unknown Values
In systems supporting three-valued logic, if a predicate refers to a missing or `Null` value, the result may be `Unknown`. 
* `True AND Unknown` = `Unknown`
* `False AND Unknown` = `False`
* `True OR Unknown` = `True`
* `False OR Unknown` = `Unknown`

### Empty Logical Arrays
If an implementation allows a variadic `AND` or `OR` with an empty set of operands:
* An empty `AND` typically defaults to `True` (identity element).
* An empty `OR` typically defaults to `False` (identity element).

### Type Mismatch
When a predicate compares incompatible types (e.g., `String gt Integer`), the logical operator receives an error or a `False` value depending on the implementation's strictness.

## Related Topics
* **019 Comparison Operators:** Detailed definitions of `eq`, `ne`, `gt`, `lt`.
* **025 Functional Operators:** Usage of functions like `contains` or `startswith` within logical blocks.
* **042 Query Optimization:** How logical structures impact index utilization.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |