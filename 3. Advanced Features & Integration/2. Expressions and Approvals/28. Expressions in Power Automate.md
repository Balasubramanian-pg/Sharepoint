# 028 Expressions in Power Automate

Canonical documentation for 028 Expressions in Power Automate. This document defines concepts, terminology, and standard usage.

## Purpose
Expressions in Power Automate serve as the logic engine for data transformation and flow control. They address the requirement for dynamic data manipulation that exceeds the capabilities of static "drag-and-drop" mapping. Expressions allow for the evaluation of logic, mathematical computation, string manipulation, and collection filtering at runtime, ensuring that automated workflows can adapt to varying input data structures and business rules.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative regarding the logic engine (Workflow Definition Language) underlying the Power Automate service.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The syntax and structure of the expression language.
* Core function categories (String, Math, Logical, Collection, Conversion).
* Data type handling and casting within expressions.
* Evaluation logic and runtime behavior.

**Out of scope:**
* Specific third-party connector API limitations.
* UI-specific "Expression Builder" walkthroughs.
* Power Apps (Canvas App) expressions (Power Fx), except where they overlap with cloud flow logic.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **WDL** | Workflow Definition Language; the underlying JSON-based schema that defines the expression syntax. |
| **Function** | A named logic block that accepts arguments and returns a single value (e.g., `concat()`, `int()`). |
| **Dynamic Content** | References to outputs from previous steps, represented in expressions as `outputs()` or `body()`. |
| **Casting** | The process of explicitly converting a value from one data type to another (e.g., String to Integer). |
| **Null-coalescing** | The practice of handling null values by providing a default fallback (e.g., using the `coalesce()` function). |
| **Token** | A placeholder within a string that is replaced by an expression's result at runtime. |

## Core Concepts

### 1. Syntax Structure
Expressions follow a functional programming syntax: `functionName(argument1, argument2, ...)`. Expressions can be nested, where the output of an inner function serves as an argument for an outer function.

### 2. Data Types
The expression engine recognizes and enforces specific data types:
*   **String:** Textual data.
*   **Integer/Float:** Numeric data.
*   **Boolean:** True/False values.
*   **Array:** Ordered lists of items.
*   **Object:** Key-value pairs (JSON).
*   **Null:** The absence of a value.

### 3. Evaluation Context
Expressions are evaluated at the moment the specific action containing them is executed. They have access to the "State" of the workflow up to that point, including trigger outputs and results from all preceding actions.

## Standard Model

The standard model for expression usage follows the **Input-Transform-Output (ITO)** pattern:

1.  **Input:** Reference a piece of dynamic data (e.g., `triggerOutputs()?['body/Title']`).
2.  **Transform:** Wrap the reference in one or more functions to modify the data (e.g., `toUpper(...)`).
3.  **Output:** The resulting value is passed to the action parameter for processing.

### Accessor Methods
*   **Dot Notation:** Used to access properties of an object (e.g., `body('ActionName').property`).
*   **Bracket Notation:** Used for properties with special characters or for dynamic property access (e.g., `body('ActionName')?['complex-property']`).
*   **Question Mark (?):** The "Safe Navigation Operator," used to prevent flow failure if a property or key does not exist (e.g., `item()?['OptionalField']`).

## Common Patterns

### Defensive Null Handling
Using `coalesce()` to ensure a workflow does not fail when an optional field is empty.
*   *Pattern:* `coalesce(outputs('Get_Item')?['body/Description'], 'No description provided')`

### Type Conversion for Math
Ensuring string-based inputs from APIs are treated as numbers for calculations.
*   *Pattern:* `add(int(variables('Count')), 1)`

### Date Standardization
Converting ISO 8601 strings into specific regional formats or performing offsets.
*   *Pattern:* `formatDateTime(utcNow(), 'yyyy-MM-dd')`

### Collection Filtering
Using expressions within "Filter Array" or "Select" actions to extract specific subsets of data without looping.

## Anti-Patterns

*   **Deep Nesting (Spaghetti Expressions):** Creating a single expression that performs five or more transformations. This is difficult to debug and maintain. *Correction: Use Compose actions to break logic into steps.*
*   **Hardcoding Environment-Specific Values:** Including IDs or URLs directly in expressions. *Correction: Use Environment Variables or configuration files.*
*   **Ignoring the Safe Navigation Operator:** Accessing properties as `body('Action')['Property']` instead of `body('Action')?['Property']`, leading to "Property Not Found" runtime errors.
*   **Redundant Casting:** Casting a value to a string that is already a string, which adds unnecessary overhead and reduces readability.

## Edge Cases

*   **Empty Strings vs. Null:** The expression engine treats `''` (empty string) and `null` differently. Functions like `empty()` will return `true` for both, but math functions will fail on both.
*   **Large Integer Precision:** When handling extremely large integers (e.g., Snowflake IDs), the engine may encounter precision issues if not handled as strings.
*   **Time Zone Offsets:** `utcNow()` always returns UTC. Failure to account for Daylight Savings Time when manually adding hours via `addHours()` is a common source of logic errors.
*   **Escaping Single Quotes:** In string literals, single quotes must be escaped by using two single quotes (e.g., `'It''s a beautiful day'`).

## Related Topics
*   **012 Data Types in Cloud Flows:** Understanding the underlying schema.
*   **045 Error Handling and Retries:** How expressions interact with "Configure Run After" settings.
*   **089 JSON Schema Validation:** Ensuring inputs for expressions meet expected structures.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |