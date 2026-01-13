# 038 Variables in Flows

Canonical documentation for 038 Variables in Flows. This document defines concepts, terminology, and standard usage.

## Purpose
Variables in Flows exist to provide a mechanism for state management and data mobility within an automated sequence of operations. They address the need to capture, store, transform, and pass information between disparate steps or nodes that may not share a direct interface. By decoupling data from specific execution steps, variables allow for dynamic logic, conditional branching, and the persistence of information throughout the lifecycle of a flow execution.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The lifecycle of data within a flow execution.
* Mechanisms for data storage, retrieval, and mutation.
* Scoping rules and visibility of data across flow segments.
* Abstract data typing and structure within flow contexts.

**Out of scope:**
* Specific vendor syntax (e.g., Salesforce Flow, Power Automate, Zapier).
* Physical database storage implementation.
* Network protocols used for data transmission between external services.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Variable** | A named container used to store a value that can be referenced or updated during flow execution. |
| **Scope** | The boundary within which a variable is accessible and its value is maintained. |
| **Data Type** | A classification that identifies the kind of data a variable can hold (e.g., String, Integer, Boolean, Collection). |
| **Initialization** | The process of assigning an initial value to a variable at the start of its lifecycle. |
| **Mutation** | The act of changing the value or state of an existing variable. |
| **Collection** | A specialized variable type designed to hold multiple items of the same or different data types. |
| **Input/Output Variable** | Variables specifically designated to receive data from or pass data to external systems or parent/child flows. |

## Core Concepts

### State Persistence
Flows are often stateless by nature until variables are introduced. Variables provide the "memory" required to track progress. State persistence ensures that a value generated in the first step of a flow is available for use in the final step, regardless of how many intermediate transformations occur.

### Data Binding
Data binding is the process of linking a variable to a specific input or output field of a flow component. This allows components to interact with the variable's value dynamically rather than relying on hardcoded parameters.

### Scope and Visibility
Variables operate within defined boundaries:
*   **Local Scope:** Variables accessible only within a specific block or loop.
*   **Flow Scope:** Variables accessible throughout the entire duration of a single flow execution.
*   **Global/Environment Scope:** Variables that persist across different flows or execution instances (often read-only during execution).

### Immutability vs. Mutability
In some flow models, variables are immutable (once set, they cannot be changed), requiring the creation of a new variable for every transformation. In most standard models, variables are mutable, allowing their values to be overwritten as the flow progresses.

## Standard Model

The standard model for Variables in Flows follows a four-stage lifecycle:

1.  **Declaration:** Defining the variable name and data type.
2.  **Assignment:** Populating the variable with data (either via a static value, a formula, or an output from a preceding step).
3.  **Transformation:** Modifying the variable through logic, arithmetic, or string manipulation.
4.  **Resolution:** Utilizing the variable's final value to perform an action (e.g., updating a record, sending a notification) or returning it as an output.

In this model, the **Flow Context** acts as the orchestrator, managing the memory allocation for these variables and ensuring they are cleared once the execution reaches a terminal state.

## Common Patterns

### The Accumulator
A numeric variable used within a loop to sum values or count iterations. The variable is initialized to zero and incremented with each pass.

### The Flag (Boolean Toggle)
A boolean variable used to track whether a specific condition has been met during the flow. This flag is later used to determine which branch of logic to execute.

### The Collection Filter
A pattern where a large set of data is stored in a collection variable, and a second "filtered" collection variable is populated with a subset of those items based on specific criteria.

### The Buffer
A temporary variable used to hold data from an external source before it is validated or transformed for final commitment to a system of record.

## Anti-Patterns

### The God Variable
Creating a single, complex object variable that holds all data for the entire flow. This makes debugging difficult and increases the risk of accidental data overwrites.

### Hardcoding Context
Storing environment-specific values (like URLs or IDs) in local variables instead of using environment-level variables. This prevents the flow from being portable across different environments (e.g., Sandbox to Production).

### Shadowing
Defining variables with identical or confusingly similar names in different scopes, leading to logic errors where the flow references the wrong data point.

### Uninitialized Reference
Attempting to read or transform a variable before it has been assigned a value, often resulting in "Null Pointer" errors or flow crashes.

## Edge Cases

### Race Conditions in Parallel Branches
When a flow splits into parallel paths that both attempt to mutate the same variable simultaneously. Without proper locking mechanisms, the final value of the variable becomes unpredictable.

### Type Coercion Ambiguity
Scenarios where a variable is assigned a value that does not strictly match its data type (e.g., a string "100" being assigned to an integer variable). The flow's behavior depends on whether the engine performs implicit casting or throws an error.

### Recursive Overflow
In flows that allow recursion (a flow calling itself), variables can consume excessive memory if the exit condition is not met, leading to a stack overflow or execution timeout.

### Large Object Serialization
When a variable stores an exceptionally large data object (e.g., a 50MB JSON blob), the overhead of passing this variable between steps can significantly degrade flow performance.

## Related Topics
*   **012 Flow Control Logic:** How variables influence branching and loops.
*   **045 Data Transformation Engines:** The logic used to mutate variable values.
*   **089 Error Handling and Fault Paths:** Managing variables when execution fails.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |