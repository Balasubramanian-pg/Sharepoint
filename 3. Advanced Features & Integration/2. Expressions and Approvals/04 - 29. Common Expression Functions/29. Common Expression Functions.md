# 029 Common Expression Functions

Canonical documentation for 029 Common Expression Functions. This document defines concepts, terminology, and standard usage.

## Purpose
Common Expression Functions provide a standardized library of operations used to transform data, evaluate logic, and perform calculations within an expression language or domain-specific language (DSL). The purpose of these functions is to abstract complex logic into reusable, predictable, and declarative units. By defining a common set of functions, systems ensure interoperability, reduce the need for custom imperative code, and provide a consistent interface for users to interact with data models.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative. While specific syntax may vary between engines, the underlying logic and functional expectations remain constant.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Functional Categories:** Definition of standard categories (String, Numeric, Logical, Temporal, and Collection).
* **Behavioral Standards:** Expectations regarding input handling, output consistency, and determinism.
* **Type Signatures:** The theoretical structure of function inputs and returns.

**Out of scope:**
* **Syntax Specification:** Specific delimiters (e.g., `()` vs `[]`) or casing (e.g., `toUpperCase` vs `UPPER`).
* **Performance Optimization:** Engine-specific execution plans or indexing strategies.
* **Custom Extensions:** Proprietary functions unique to a single vendor or platform.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Expression** | A combination of one or more constants, variables, and functions that the engine evaluates to produce a value. |
| **Arity** | The number of arguments or operands that a function takes. |
| **Determinism** | A property where a function, given the same set of input values, will always produce the same output. |
| **Type Coercion** | The automatic or implicit conversion of a value from one data type to another during function evaluation. |
| **Null-Safety** | The behavior of a function when encountering a null or missing value, typically resulting in a null return or a default value. |
| **Variadic Function** | A function that accepts a variable number of arguments. |

## Core Concepts

### Functional Purity
In the context of common expressions, functions are ideally "pure." They should not produce side effects (such as modifying a database record or updating a global variable) and should rely solely on their input arguments to produce a result.

### Evaluation Context
Functions operate within a context that provides the necessary data (variables, constants, and metadata). The context determines the scope of available data and the specific behavior of environment-aware functions (e.g., "Current User" or "System Time").

### Type Safety and Validation
Every function has an expected input type (e.g., String, Integer, Boolean). Canonical functions must define how they handle type mismatches—whether through strict rejection (errors) or implicit coercion (casting).

## Standard Model

The standard model for expression functions categorizes them by the data types they manipulate:

### 1. String Functions
Operations focused on text manipulation.
* **Transformation:** Changing case, trimming whitespace, or padding.
* **Extraction:** Substring retrieval, character indexing, or regex matching.
* **Combination:** Concatenation or interpolation of multiple strings.

### 2. Numeric Functions
Mathematical operations for quantitative data.
* **Arithmetic:** Basic operations (addition, subtraction, multiplication, division).
* **Rounding:** Floor, ceiling, and precision-based rounding.
* **Analysis:** Absolute values, square roots, and logarithmic functions.

### 3. Logical and Conditional Functions
Functions that govern flow and decision-making.
* **Boolean Logic:** AND, OR, NOT, and XOR operations.
* **Comparison:** Equality, inequality, and range checks (Greater Than, Less Than).
* **Branching:** Ternary logic (If-Then-Else) and switch/case equivalents.

### 4. Temporal Functions
Handling of dates, times, and durations.
* **Extraction:** Isolating the year, month, day, or hour from a timestamp.
* **Arithmetic:** Adding or subtracting intervals (e.g., adding 30 days to a date).
* **Formatting:** Converting temporal objects into standardized string representations (ISO-8601).

### 5. Collection Functions
Operations on arrays, lists, or sets.
* **Filtering:** Returning a subset of a collection based on a predicate.
* **Mapping:** Applying a function to every element in a collection.
* **Aggregation:** Reducing a collection to a single value (Sum, Average, Count, Min, Max).

## Common Patterns

### Function Chaining (Piping)
A pattern where the output of one function is passed directly as the input to the next. This promotes readability by following a linear transformation path rather than deeply nested parentheses.

### Coalescing
The practice of using a function to return the first non-null value from a list of arguments. This is the standard pattern for providing default values in data-sparse environments.

### Predicate Mapping
Using a logical function as an argument for a collection function (e.g., passing a "Greater Than" check into a "Filter" function) to perform complex data selection.

## Anti-Patterns

### Deep Nesting (The Onion Pattern)
Nesting functions so deeply that the logic becomes unreadable and difficult to debug. 
* *Correction:* Use intermediate variables or function chaining where supported.

### Over-Coercion
Relying on implicit type conversion (e.g., treating the string "123" as an integer automatically). This leads to unpredictable results when the input format changes slightly.
* *Correction:* Use explicit casting functions to ensure type intent is clear.

### Side-Effect Reliance
Attempting to use expression functions to trigger external actions (like sending an email) within a logic block intended for data transformation.
* *Correction:* Separate data transformation logic from action-oriented workflow logic.

## Edge Cases

### Null and Undefined Handling
The most common edge case is the "Null Propagation" vs. "Null Suppression" conflict. Standard functions must define if `Function(null)` returns `null`, an error, or a default value.

### Division by Zero
In numeric functions, dividing by zero must have a defined behavior: either returning a specific "Infinity" constant, a Null value, or throwing a terminal evaluation error.

### Timezone Shifts
Temporal functions often fail when crossing Daylight Saving Time (DST) boundaries or when the evaluation context's timezone differs from the data's timezone. Canonical functions should ideally operate on UTC to avoid ambiguity.

### Floating Point Precision
Numeric functions involving decimals may encounter rounding errors inherent to binary floating-point representation (e.g., 0.1 + 0.2 != 0.3). High-precision financial functions are required for these specific use cases.

## Related Topics
* **012 Data Types and Schema:** Defines the structures that functions operate upon.
* **045 Evaluation Engines:** The runtime environments that execute these functions.
* **078 Domain Specific Languages (DSL):** The syntax wrappers for expression functions.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |