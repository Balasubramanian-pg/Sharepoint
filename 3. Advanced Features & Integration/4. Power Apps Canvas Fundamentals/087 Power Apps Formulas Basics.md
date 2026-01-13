# 087 Power Apps Formulas Basics

Canonical documentation for 087 Power Apps Formulas Basics. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Power Apps formulas is to provide a declarative, functional logic layer that governs the behavior, data transformation, and visual state of low-code applications. This logic layer bridges the gap between static user interface elements and dynamic data sources, allowing for complex operations to be expressed through expressions rather than traditional imperative code. It addresses the need for a high-level abstraction that enables rapid application development while maintaining the power to perform sophisticated data manipulation.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative, focusing on the underlying logic engine (Power Fx) principles.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Syntax and Grammar:** The fundamental structure of expressions and functions.
* **Data Flow:** How data is passed, transformed, and reacted to within the formula engine.
* **Evaluation Models:** The distinction between declarative (automatic) and imperative (event-driven) logic.
* **Type System:** How the engine handles data types and coercion.

**Out of scope:**
* **UI/UX Design:** Visual styling of components (except where driven by formulas).
* **Connector-Specific Logic:** The internal workings of third-party APIs or specific database schemas.
* **Environment Administration:** Deployment, licensing, or governance of the hosting platform.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Declarative Logic** | Logic that describes *what* the result should be, allowing the engine to automatically update values when dependencies change. |
| **Imperative Logic** | Logic that describes *how* to perform a task through a sequence of specific actions or commands, usually triggered by an event. |
| **Delegation** | The process by which the formula engine passes data processing (filtering, sorting) to the underlying data source rather than processing it locally. |
| **Signal** | A dynamic value that the engine monitors for changes (e.g., a sensor reading, a timer, or a user's current location). |
| **Context** | The local state or scope within a specific screen or component, distinct from global application state. |
| **Collection** | A specialized local data structure used to store and manipulate tables of data within the application's memory. |
| **Strong Typing** | A characteristic where every value and expression has a specific data type determined at authoring time. |

## Core Concepts

### 1. Functional and Declarative Nature
The core of the formula engine is functional. Most formulas are "pure," meaning they take inputs and return an output without modifying the state of the application. The engine uses a declarative model similar to a spreadsheet: when a dependency changes, all dependent formulas are automatically recalculated by the system.

### 2. The Dependency Graph
The engine maintains an internal directed acyclic graph (DAG) of all formulas. This graph tracks which properties depend on which data points. This ensures that the application remains performant by only recalculating the specific nodes affected by a change in data or user input.

### 3. Data Types and Coercion
The system supports standard types (Text, Number, Boolean, Date) and complex types (Records, Tables). While the system is strongly typed, it performs "implicit coercion" in specific scenarios (e.g., treating a single-column table as a list of values) to simplify the authoring experience.

### 4. Delegation and Data Limits
Because the engine is designed to work with large-scale cloud data, it distinguishes between operations that can be "delegated" to a server and those that must be performed locally. Local processing is subject to "data rows limits," which necessitates a deep understanding of delegable functions to ensure application scalability.

## Standard Model

The standard model for Power Apps formulas follows the **Power Fx** specification. It utilizes a "Formula Bar" interface where logic is bound to specific properties of objects.

1.  **Expression Evaluation:** Formulas are evaluated in real-time. If a Label's `Text` property is set to `User().FullName`, the engine binds that property to the identity provider's signal.
2.  **Event-Driven Actions:** Imperative logic is reserved for specific "Behavior" properties (e.g., `OnSelect`, `OnVisible`). These properties allow for sequential operations using the semi-colon (`;`) operator.
3.  **Immutable Data Patterns:** While variables exist, the standard model encourages the use of data streams and direct filtering of sources over the manual caching of data into local variables.

## Common Patterns

### The Filter-Sort-Search Pattern
The most common pattern for data retrieval involves nesting functions to refine a dataset:
`Sort(Filter(DataSource, Condition), Column, Direction)`
This pattern is optimized for delegation to ensure only the necessary records are retrieved from the server.

### The Contextual State Pattern
Using local variables (`UpdateContext`) to manage UI states that are only relevant to the current screen, such as toggling the visibility of a modal or tracking a step in a multi-stage form.

### The Lookup and Patch Pattern
A standard approach for CRUD (Create, Read, Update, Delete) operations where `LookUp` identifies a specific record and `Patch` applies changes to that record in the data source.

## Anti-Patterns

*   **The "God" Formula:** Placing excessive, complex logic within a single property, making it difficult to debug and maintain.
*   **Variable Overuse:** Using global variables (`Set`) to store data that could be retrieved directly from a data source or a declarative formula. This breaks the dependency graph and leads to stale data.
*   **Non-Delegable Filtering:** Using functions or operators (e.g., `IsNumeric`, `Last`) within a filter criteria that the underlying data source cannot process, resulting in incomplete data sets.
*   **Nested Lookups in Galleries:** Performing a `LookUp` for every row in a list/gallery, which creates an "N+1" query problem and severely degrades performance.

## Edge Cases

*   **Blank vs. Empty String:** The engine distinguishes between a `Blank` (null/no value) and an empty string (`""`). Formulas must explicitly handle `IsBlank` or `Coalesce` to avoid logic errors in calculations.
*   **Circular References:** The engine prevents a property from depending on itself, either directly or indirectly. If detected, the engine will cease evaluation of the affected nodes.
*   **Asynchronous Timing:** When multiple imperative actions are triggered (e.g., `Patch` followed by `Navigate`), the engine does not always guarantee the completion of the first before the second begins unless specific patterns are followed.
*   **Type Mismatch in Collections:** Adding a record to a collection that has a different schema than existing records can cause the collection to become "untyped" or error out.

## Related Topics
*   **088 Data Source Delegation:** Deep dive into server-side vs. client-side processing.
*   **089 Power Fx Grammar:** Technical specification of the expression language.
*   **090 Application Lifecycle Management (ALM):** How formulas are versioned and deployed.
*   **091 Component Framework:** Extending formulas with custom code components.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |