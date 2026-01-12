# 041 Loops in Power Automate

Canonical documentation for 041 Loops in Power Automate. This document defines concepts, terminology, and standard usage.

## Purpose
Loops in Power Automate provide the control flow mechanisms required to perform repetitive tasks. They address the need to process collections of data (arrays) or to repeat a sequence of actions until a specific logical state is achieved. By automating iterative logic, loops eliminate the need for manual intervention in multi-item workflows and enable complex state-based automation.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative regarding the logic and behavior of iterative structures within the Power Automate engine.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Iterative Control Structures:** Definition and behavior of "Apply to each" and "Do until" mechanisms.
* **Execution Models:** Sequential versus parallel execution logic.
* **Flow Control:** Termination conditions, limits, and iteration constraints.
* **Data Context:** Handling of item-level scope within iterations.

**Out of scope:**
* **Connector-Specific Implementation:** Specific configurations for third-party APIs (e.g., how to specifically loop through "Jira" tickets).
* **Expression Syntax:** Detailed documentation of the Logic Apps Expression Language (WDL), though its use in loops is referenced.
* **Licensing:** Impact of licensing tiers on loop performance or limits.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Iteration** | A single execution of the block of actions contained within a loop. |
| **Collection** | An array or set of data objects provided as input to an iterative structure. |
| **Concurrency** | The ability of the engine to execute multiple iterations of a loop simultaneously. |
| **Sequential Execution** | A mode where each iteration must complete before the next begins (Concurrency = 1). |
| **Termination Condition** | A boolean expression that determines when a "Do until" loop should cease execution. |
| **Iteration Limit** | The maximum number of times a loop is permitted to run before the engine forces a termination. |
| **Scope Context** | The data environment within a loop that allows access to the "current item" being processed. |

## Core Concepts

### 1. Collection-Based Iteration (Apply to Each)
This structure is used when the number of repetitions is defined by the size of an input array. The loop executes once for every item in the collection. The engine automatically handles the indexing and retrieval of individual items within the loop's scope.

### 2. Condition-Based Iteration (Do Until)
This structure repeats a block of actions until a specific condition evaluates to `true`. Unlike collection-based loops, the number of iterations is dynamic and depends on the side effects of the actions within the loop or external state changes.

### 3. Concurrency and Parallelism
Power Automate allows for "Apply to each" loops to run iterations in parallel. This significantly reduces total execution time for independent tasks but introduces complexity regarding variable updates and resource contention.

### 4. Limits and Thresholds
Loops are governed by engine-level constraints to prevent infinite execution and resource exhaustion. These include:
* **Count Limits:** Maximum number of iterations allowed.
* **Timeout Limits:** Maximum duration a loop can run.
* **Throughput Limits:** Constraints on the number of actions executed per interval.

## Standard Model

The standard model for implementing loops follows a "Filter-Process-Verify" lifecycle:

1.  **Pre-Loop Optimization:** Data should be filtered at the source (e.g., OData filters) to ensure the loop only processes necessary items.
2.  **Initialization:** Variables used to track state across iterations must be initialized outside the loop scope.
3.  **Execution:**
    *   Use **Apply to each** for known datasets.
    *   Use **Do until** for polling or state-change scenarios.
4.  **Concurrency Management:** By default, "Apply to each" may run sequentially or with limited parallelism. For high-performance requirements, concurrency should be increased only if the actions inside are thread-safe (i.e., do not rely on shared variables).
5.  **Error Handling:** Implementation of "Configure Run After" logic within or after the loop to handle partial failures in a batch.

## Common Patterns

### The Batch Processor
Dividing a large dataset into smaller chunks (batches) and using a loop to process each chunk. This is often used to circumvent API limits or engine timeouts.

### The Polling Pattern
Using a "Do until" loop to check the status of an asynchronous process (e.g., a long-running report generation) with a "Delay" action inside the loop to manage frequency.

### The Singleton Update
Using a loop to find a specific item in an array and updating a variable, then using a termination strategy to effectively "break" or ignore subsequent items.

## Anti-Patterns

### The "God Loop"
Placing excessive logic, nested loops, and complex conditionals inside a single loop. This leads to unmaintainable flows and high failure rates.
* *Correction:* Offload complex logic to child flows.

### Variable Contention in Parallelism
Updating a global variable (e.g., "Increment variable") inside an "Apply to each" loop while concurrency is enabled. This results in race conditions and inaccurate data.
* *Correction:* Use sequential execution if variables must be updated, or use the "Select" and "Join" actions for data transformation.

### Unbounded "Do Until"
Setting a "Do until" loop with a condition that may never be met and a high iteration limit.
* *Correction:* Always define a reasonable timeout and a maximum iteration count as a fail-safe.

### Row-by-Row Processing
Using a loop to perform an operation that could be done via a bulk operation or a declarative action (like "Filter array" or "Select").
* *Correction:* Use data operations to transform arrays before looping.

## Edge Cases

*   **Empty Collections:** If an "Apply to each" receives an empty array, it will skip execution entirely and report success. This can cause downstream failures if subsequent actions expect data.
*   **Null Inputs:** Providing a `null` value instead of an empty array to a loop will typically result in a runtime error.
*   **Self-Modifying Collections:** Attempting to modify the collection being iterated over (e.g., deleting items from the source list during the loop) can lead to skipped items or inconsistent indexing.
*   **Nesting Limits:** Power Automate enforces a maximum depth for nested loops (typically 8 levels). Exceeding this requires architectural redesign.

## Related Topics
*   **022 Variables in Power Automate:** Understanding global vs. local state.
*   **035 Data Operations:** Using Select, Filter, and Join to minimize loop usage.
*   **050 Error Handling:** Using Scopes and Run After configurations within iterative structures.
*   **088 Child Flows:** Offloading loop logic for performance and reusability.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |