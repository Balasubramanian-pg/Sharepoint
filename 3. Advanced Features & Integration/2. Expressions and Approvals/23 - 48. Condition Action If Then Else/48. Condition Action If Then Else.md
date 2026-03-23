# 048 Condition Action If Then Else

Canonical documentation for 048 Condition Action If Then Else. This document defines concepts, terminology, and standard usage.

## Purpose
The **048 Condition Action If Then Else** pattern exists to facilitate non-linear execution paths within a system. It addresses the requirement for deterministic decision-making where a system must evaluate a specific state or input against a set of criteria to determine which subsequent action or set of actions to perform. 

By providing a structured framework for branching logic, this pattern ensures that processes can handle variability, exceptions, and diverse data states without requiring manual intervention.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logical evaluation of Boolean predicates.
* Structural requirements for primary, alternative, and fallback execution branches.
* Deterministic flow control principles.
* Order of operations in conditional evaluation.

**Out of scope:**
* Specific syntax for programming languages (e.g., C++, Python, JavaScript).
* Graphical user interface (GUI) design for rule builders.
* Performance benchmarks for specific hardware architectures.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Predicate** | A logical expression that evaluates to either True or False (Boolean). |
| **Condition** | The specific criteria or state being tested within the Predicate. |
| **Then-Branch** | The sequence of actions executed if the Predicate evaluates to True. |
| **Else-Branch** | The sequence of actions executed if the Predicate evaluates to False. |
| **Branching** | The act of diverting the execution flow into one of several mutually exclusive paths. |
| **Determinism** | The property where a given input and state always produce the same output path. |
| **Short-Circuiting** | A behavior where the evaluation of a complex predicate stops as soon as the outcome is determined. |

## Core Concepts
The 048 Condition Action If Then Else pattern is built upon three fundamental pillars:

### 1. Binary Evaluation
At its core, the pattern relies on the law of the excluded middle. A condition must be resolvable to a binary state. Even in systems supporting "truthy" or "falsy" values, the logic engine must ultimately map these to a binary choice to select a branch.

### 2. Mutual Exclusivity
In a standard If-Then-Else structure, the branches are mutually exclusive. Execution of the "Then-Branch" precludes the execution of the "Else-Branch" for a single evaluation cycle.

### 3. Sequential Integrity
The condition must be evaluated *prior* to the action. The state of the system at the moment of evaluation dictates the path; changes to the state occurring during the action phase do not retroactively change the branch selection for that specific cycle.

## Standard Model
The standard model for 048 Condition Action If Then Else follows a linear evaluation-to-execution pipeline:

1.  **Input Acquisition:** The system gathers the necessary data points required by the Predicate.
2.  **Predicate Evaluation:** The system applies logical operators to the input.
3.  **Gatekeeping:** 
    *   If **True**: Divert to the "Then" path.
    *   If **False**: Divert to the "Else" path (or terminate if no Else is defined).
4.  **Action Execution:** The system performs the operations defined within the selected branch.
5.  **Convergence:** The execution paths typically rejoin at a common post-conditional anchor point.

## Common Patterns

### The Guard Clause
A pattern where the "If" condition checks for invalid or "edge" data at the beginning of a process, using the "Then" branch to exit or error out, leaving the remainder of the logic for the "Else" (or implicit continuation).

### Nested Conditionals
The placement of an If-Then-Else structure within another If-Then-Else branch. This is used for multi-layered decision trees where the second condition is only relevant if the first condition is met.

### Default Fallback (The "Else" Safety)
Using the "Else" branch as a catch-all to ensure the system remains in a known state if the primary condition is not met, preventing "hanging" processes.

## Anti-Patterns

### The Arrow Anti-Pattern
Excessive nesting of If-Then-Else blocks that creates a code structure resembling an arrowhead. This reduces maintainability and increases cognitive load, making it difficult to trace the logic flow.

### Side-Effect Predicates
Including actions that change the system state within the Predicate evaluation itself. Predicates should be "read-only" to ensure that evaluating the condition does not inadvertently alter the outcome of the action.

### Overlapping Predicates
Defining multiple conditions in a sequence where more than one could be true, but only the first is executed. This leads to "shadowed" logic that is unreachable and difficult to debug.

## Edge Cases

### Null or Missing Data
When a Predicate references a variable that is undefined or null. A robust implementation must define whether this results in a "False" evaluation, a system error, or a diversion to a specific exception-handling branch.

### Type Coercion
Scenarios where the input data type does not match the expected type of the Predicate (e.g., comparing a string "1" to an integer 1). The 048 model requires strict definition of how types are cast or compared to maintain determinism.

### Race Conditions
In multi-threaded environments, the state of the condition might change between the moment of evaluation and the moment of action. The standard model assumes state atomicity during the evaluation-action transition.

## Related Topics
* **049 Switch Case Logic:** For handling multiple discrete values without nested Ifs.
* **050 Boolean Algebra:** The mathematical foundation for complex predicates.
* **102 Error Handling and Exceptions:** For managing failures within action branches.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |