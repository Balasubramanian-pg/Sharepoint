# 043 Do Until Loop

Canonical documentation for 043 Do Until Loop. This document defines concepts, terminology, and standard usage.

## Purpose
The **Do Until Loop** is a control flow statement designed to execute a block of code repeatedly as long as a specified condition evaluates to false. It exists to address scenarios where an operation must continue until a specific state change or "exit event" occurs. 

Unlike a "While" loop, which focuses on the continuation of a process based on a positive condition, the "Until" loop focuses on the termination of a process based on the achievement of a goal state. This structure is particularly useful for processes where the number of required iterations is unknown a priori and the logic is most naturally expressed as "keep going until this is finished."

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logical structure and evaluation order of "Until" semantics.
* Distinction between pre-test and post-test iterations.
* Termination criteria and state management within the loop body.
* Theoretical boundaries of indefinite iteration.

**Out of scope:**
* Specific syntax for programming languages (e.g., VBA, Perl, Ruby).
* Memory management or stack allocation details.
* Hardware-level interrupt handling.

## Definitions
| Term | Definition |
|------|------------|
| **Condition** | A boolean expression evaluated to determine whether the loop should terminate. |
| **Loop Body** | The sequence of instructions executed during each iteration of the loop. |
| **Iteration** | A single execution of the loop body. |
| **Termination Criterion** | The specific state where the condition evaluates to true, causing the loop to cease. |
| **Post-test Loop** | A loop where the condition is evaluated *after* the loop body has executed, ensuring at least one execution. |
| **Pre-test Loop** | A loop where the condition is evaluated *before* the loop body executes, potentially resulting in zero executions. |
| **Infinite Loop** | A failure state where the termination criterion is never met, causing indefinite execution. |

## Core Concepts
### The Inverse Logic Principle
The fundamental characteristic of the Do Until loop is its inverse relationship to the "While" loop. Where a While loop executes on `True`, the Until loop executes on `False`. The loop "breaks" or "terminates" the moment the condition becomes `True`.

### Evaluation Timing
The Do Until loop can be implemented in two primary modes:
1.  **Test-at-Bottom (Post-test):** The logic is "Do [Body] Until [Condition]." This guarantees the body runs at least once.
2.  **Test-at-Top (Pre-test):** The logic is "Until [Condition] Do [Body]." If the condition is true at the start, the body never executes.

### State Mutation
For a Do Until loop to be deterministic, the loop body or an external asynchronous event must mutate the state of the variables involved in the condition. Without state mutation, the loop remains in a perpetual state of non-termination.

## Standard Model
The standard model for a Do Until loop follows this logical flow:

1.  **Entry:** The control flow enters the loop structure.
2.  **Execution (Post-test):** The loop body executes.
3.  **Evaluation:** The condition is checked.
    *   If **False**: Control returns to the start of the loop body.
    *   If **True**: Control exits the loop structure (Termination).
4.  **Exit:** Execution continues with the next sequential instruction outside the loop.

In the **Pre-test** variation, the Evaluation (Step 3) occurs before the Execution (Step 2).

## Common Patterns
### 1. Polling/Waiting
Used to pause execution until an external resource becomes available or a flag is set.
*   *Pattern:* Do [Check Resource] Until [Resource Ready == True].

### 2. User Input Validation
Used to force the acquisition of valid data.
*   *Pattern:* Do [Prompt User] Until [Input Is Valid].

### 3. Convergence Algorithms
Used in mathematical contexts where a value is calculated repeatedly until the difference between iterations (delta) falls below a specific threshold.
*   *Pattern:* Do [Calculate] Until [Delta < Epsilon].

## Anti-Patterns
### 1. The "Never-True" Condition
Defining a termination criterion that is mathematically or logically impossible to achieve within the loop's scope, leading to a system hang or resource exhaustion.

### 2. Side-Effect Dependency
Relying on global state changes that are not guaranteed to occur, rather than mutating a local control variable. This makes the loop's behavior non-deterministic.

### 3. The "Off-by-One" Error
In post-test loops, failing to account for the fact that the body executes one final time even when the condition is met during that final execution.

### 4. Negation Confusion
Using a "While" logic inside an "Until" structure (e.g., `Until x != 0`), which creates double-negative logic that is difficult to audit and maintain.

## Edge Cases
### 1. Condition True at Initialization
In a **post-test** loop, the body will execute exactly once even if the condition is already true. In a **pre-test** loop, the body will not execute at all. This distinction is critical for operations involving null pointers or empty datasets.

### 2. External Termination (Break/Exit)
Most implementations allow for an "early exit" or "break" statement. While this technically terminates the loop, it bypasses the "Until" condition, potentially leaving the system in an inconsistent state if the loop was intended to reach a specific goal.

### 3. Overflow/Wrap-around
If the condition relies on a counter reaching a value, but the counter overflows and wraps around (e.g., an integer exceeding its maximum value), the "Until" condition may be skipped entirely, leading to an infinite loop.

## Related Topics
*   **042 While Loop:** The logical counterpart to the Do Until loop.
*   **040 Iteration Structures:** General theory of repetitive execution.
*   **045 Loop Invariants:** Logical assertions that remain true throughout the loop's execution.
*   **012 Boolean Logic:** The underlying truth-value system used for evaluations.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |