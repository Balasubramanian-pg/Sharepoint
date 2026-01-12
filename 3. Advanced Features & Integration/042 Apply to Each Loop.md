# 042 Apply to Each Loop

Canonical documentation for 042 Apply to Each Loop. This document defines concepts, terminology, and standard usage.

## Purpose
The **Apply to Each Loop** is a control flow construct designed to perform a set of operations repeatedly across a collection of discrete data elements. Its primary purpose is to automate the processing of arrays, lists, or sets where the number of elements is dynamic or unknown at design time. 

By abstracting the iteration logic, the Apply to Each Loop ensures that the same logic is applied consistently to every member of a dataset, facilitating data transformation, state updates, and downstream service orchestration.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   **Iteration Logic:** The mechanics of traversing a collection.
*   **Execution Modes:** Sequential vs. parallel processing theories.
*   **Data Scoping:** How data is accessed within the loop context.
*   **Termination Criteria:** Standard conditions for loop completion.

**Out of scope:**
*   **Vendor-specific syntax:** (e.g., specific JSON schemas for Logic Apps or Python `for` loop syntax).
*   **Hardware-level optimization:** (e.g., CPU register management during iteration).
*   **Infinite loops:** These are generally handled by "While" or "Until" constructs, not "Apply to Each."

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Collection** | An ordered or unordered group of data elements (array, list, or set) serving as the loop input. |
| **Iteration** | A single execution of the loop's internal logic for one specific item in the collection. |
| **Iterator Variable** | A placeholder or reference representing the current item being processed in the active iteration. |
| **Concurrency** | The ability of the loop to execute multiple iterations simultaneously rather than one after another. |
| **Sequential Execution** | A processing mode where iteration $n+1$ begins only after iteration $n$ has successfully completed. |
| **Scope** | The logical boundary within which the iterator variable and internal actions are accessible. |

## Core Concepts

### The Input Collection
The loop requires a valid collection as its source. If the source is not a collection (e.g., a single string or an integer), the loop structure typically fails or requires a type-cast to a singleton list.

### The Loop Body
The body contains the sequence of actions or operations to be performed. Every action within the body has access to the "Current Item" context, allowing for dynamic processing based on the specific data of that iteration.

### Contextual Reference
Within an Apply to Each Loop, the system must maintain a pointer to the current index or item. This reference is transient and is updated or discarded upon the start of the next iteration.

## Standard Model

The standard model for an Apply to Each Loop follows a four-phase lifecycle:

1.  **Initialization:** The system evaluates the input collection and determines the total count of iterations required.
2.  **Context Binding:** The system selects the next available item from the collection and binds it to the iterator variable.
3.  **Execution:** The logic defined within the loop body is executed using the bound context.
4.  **Evaluation/Transition:** The system checks if more items remain in the collection. If yes, it returns to Phase 2. If no, it exits the loop and proceeds to the next step in the workflow.

## Common Patterns

### Data Transformation (Map)
The loop iterates through a collection, modifies each item, and produces a new collection of the same length but with altered data structures.

### Filtering (Select)
The loop evaluates each item against a condition. Only items meeting the criteria are passed to a secondary collection or action.

### Accumulation (Reduce)
The loop iterates through items to calculate a single aggregate value (e.g., summing prices, concatenating strings) stored in a variable initialized outside the loop scope.

### Batch Orchestration
The loop is used to trigger external processes (e.g., sending an email, calling an API) for every record retrieved from a database or trigger event.

## Anti-Patterns

### Deep Nesting
Placing multiple Apply to Each Loops inside one another (O(n²) complexity or higher). This leads to performance degradation and makes debugging difficult.
*   *Correction:* Flatten the data structure or use "Filter" operations before entering the loop.

### Side-Effect Reliance in Parallel Mode
Enabling concurrency while performing actions that rely on a specific order or modify a shared global variable.
*   *Correction:* Use sequential execution if order matters, or use atomic operations for shared state.

### Large Collection Processing without Throttling
Attempting to iterate over thousands of items in a single loop without considering timeout limits or API rate limits.
*   *Correction:* Implement pagination or batching strategies.

## Edge Cases

### Empty Collections
When the input collection contains zero items, the loop should complete successfully without executing the body. It should not trigger an error unless explicitly configured to require a minimum item count.

### Null Inputs
If the input source is `null` rather than an empty list, the loop may fail. Robust implementations should include a pre-check or coalesce the input to an empty array.

### Modification of Collection During Iteration
If the loop body adds or removes items from the collection it is currently iterating over, the behavior is often undefined (e.g., skipping items or infinite loops). 
*   *Standard approach:* Iteration should occur over a "snapshot" of the collection taken at the moment of initialization.

### Parallelism Bottlenecks
In high-concurrency modes, the loop may be limited by the downstream systems (e.g., an API that only allows 5 concurrent connections). The loop must be able to throttle its concurrency to match the weakest link in the chain.

## Related Topics
*   **041 Conditionals:** Often used inside loops to branch logic per item.
*   **043 Do Until Loop:** A related iteration construct based on a truth condition rather than a collection.
*   **015 Variables:** Used for accumulation and state management across iterations.
*   **088 Error Handling:** Defining how the loop behaves if a single iteration fails (e.g., "Continue on Error" vs. "Abort").

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |