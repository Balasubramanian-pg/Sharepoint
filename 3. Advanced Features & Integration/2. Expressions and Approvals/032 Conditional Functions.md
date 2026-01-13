# 032 Conditional Functions

Canonical documentation for 032 Conditional Functions. This document defines concepts, terminology, and standard usage.

## Purpose
Conditional functions provide a mechanism for executing logic-based branching within expressions. They allow systems to return different outputs based on whether a specified criterion (predicate) evaluates to true or false. 

The primary purpose of these functions is to enable declarative decision-making within data transformations, queries, and calculations without requiring procedural control flow structures. They bridge the gap between static data retrieval and dynamic logic by allowing the output to be a function of the input state.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Functional representations of logic (e.g., Ternary, Case, Coalescence).
* Evaluation strategies (Eager vs. Lazy).
* Predicate-based branching.
* Handling of null or undefined states through specialized functions.

**Out of scope:**
* Procedural control flow statements (e.g., `if...else` blocks, `for` loops).
* Specific syntax for individual programming languages or SQL dialects.
* Hardware-level branch prediction.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Predicate** | A Boolean expression that evaluates to either True or False, serving as the trigger for a conditional branch. |
| **Branch** | One of the potential output paths or values selected based on the evaluation of a predicate. |
| **Short-circuiting** | An evaluation strategy where the function stops evaluating arguments as soon as the final outcome is determined. |
| **Default/Fallback** | The value returned when no specified predicates are met. |
| **Arity** | The number of arguments a conditional function accepts (e.g., Ternary functions have an arity of three). |
| **Null-Coalescence** | A specific type of conditional function that selects the first non-null value from a list of arguments. |

## Core Concepts

### 1. Predicate Evaluation
At the heart of every conditional function is the predicate. The system must evaluate an expression to a Boolean state. In systems with "truthy" or "falsy" values, the conditional function must follow a deterministic mapping to resolve these to a binary choice.

### 2. Result Mapping
Conditional functions map the result of a predicate to a specific return value. Unlike procedural logic, which may execute a series of actions, a conditional function is strictly an expression that resolves to a single value or object.

### 3. Determinism
A canonical conditional function should be deterministic: given the same input state and predicates, it must always return the same output.

## Standard Model

The standard model for conditional functions categorizes them into three primary archetypes:

### The Binary Choice (Ternary)
The most basic form, requiring three components:
1.  **Condition:** The predicate to test.
2.  **Consequent:** The value returned if the predicate is true.
3.  **Alternative:** The value returned if the predicate is false.

### The Multi-Branch (Search/Switch)
A structure that evaluates multiple predicates sequentially:
*   **Searched:** Evaluates a series of independent Boolean expressions and returns the result for the first "True" encounter.
*   **Simple:** Compares a single base expression against multiple potential values (equality-based).

### The Existential (Null-Handling)
Functions specifically designed to handle the absence of data. These functions check for `NULL`, `NaN`, or `Undefined` states and provide a substitute value, often used for data normalization.

## Common Patterns

### Defaulting
Using a conditional function (often Coalesce) to ensure a variable has a valid value before it is used in downstream calculations.
*   *Pattern:* `Value = Function(Input, Default_Value)`

### Range Mapping
Using multi-branch logic to categorize continuous data into discrete buckets.
*   *Pattern:* `If(x > 90, 'A', If(x > 80, 'B', 'C'))`

### Guard Clauses
Using a conditional function to prevent errors, such as division by zero, by checking the denominator before the operation.
*   *Pattern:* `Result = If(Denominator == 0, Null, Numerator / Denominator)`

## Anti-Patterns

### Deep Nesting
Nesting binary conditional functions (e.g., `IF(IF(IF(...)))`) instead of using a multi-branch structure (e.g., `CASE` or `SWITCH`). This reduces readability and increases the likelihood of logic errors.

### Type Inconsistency
Returning different data types from different branches (e.g., Branch A returns a String, Branch B returns an Integer). This can lead to unpredictable behavior in statically typed or strictly schema-bound systems.

### Side-Effect Dependency
Relying on a conditional function to trigger a side effect (like writing to a log) during the evaluation of a branch. Conditional functions should be pure and return values only.

## Edge Cases

### Lazy vs. Eager Evaluation
*   **Eager Evaluation:** The system evaluates all possible branch results before checking the predicate. This can lead to performance hits or errors (e.g., a "Division by Zero" error in a branch that wasn't even selected).
*   **Lazy Evaluation:** The system only evaluates the branch that corresponds to the predicate's result. This is the preferred behavior for complex or resource-intensive operations.

### Three-Valued Logic
In systems that support `NULL`, a predicate may evaluate to `True`, `False`, or `Unknown`. Canonical conditional functions must define how `Unknown` is handled—typically by treating it as `False` and triggering the alternative/default branch.

### Empty Argument Lists
Behavior of variadic conditional functions (like Coalesce) when no arguments are provided. Standard behavior should return a null-state or throw a definition error.

## Related Topics
*   **012 Boolean Algebra:** The mathematical foundation for predicates.
*   **045 Data Normalization:** The process of using conditional functions to clean datasets.
*   **088 Control Flow:** The procedural counterpart to functional conditions.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |