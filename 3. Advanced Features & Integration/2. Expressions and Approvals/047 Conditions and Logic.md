# 047 Conditions and Logic

Canonical documentation for 047 Conditions and Logic. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of 047 Conditions and Logic is to provide a formal framework for decision-making within a system. It addresses the requirement for systems to exhibit dynamic behavior by evaluating state, inputs, and environmental variables to determine specific execution paths or outcomes. This topic establishes the rules for how predicates are formed, how logical operations are composed, and how branching is governed to ensure predictability and determinism.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Boolean Evaluation:** The fundamental mechanics of true/false determination.
* **Logical Operators:** The composition of complex expressions using AND, OR, NOT, and XOR.
* **Control Flow Structures:** Abstract branching mechanisms such as conditional execution and multi-way selection.
* **Evaluation Strategies:** Concepts such as short-circuiting and eager vs. lazy evaluation.
* **Truthiness and Falsiness:** The mapping of non-boolean types to boolean outcomes.

**Out of scope:**
* **Specific Syntax:** Implementation details of specific programming languages (e.g., C# `if` vs. Python `if`).
* **Hardware Logic Gates:** Physical implementation of transistors and circuits.
* **Business Rules Engines:** Specific third-party software products for managing logic.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Predicate** | An expression that returns a Boolean value (True or False) based on the evaluation of its inputs. |
| **Operand** | The data or expression acted upon by a logical operator. |
| **Short-circuiting** | An evaluation strategy where the second argument is executed or evaluated only if the first argument does not suffice to determine the value of the expression. |
| **Branching** | The act of diverting the flow of execution based on the result of a conditional evaluation. |
| **Truthiness** | A heuristic used to interpret non-boolean values as boolean for the purpose of logical evaluation. |
| **Deterministic Logic** | A logical structure that, given the same initial state and inputs, will always produce the same outcome. |
| **Unary Operator** | An operator that takes a single operand (e.g., NOT). |
| **Binary Operator** | An operator that takes two operands (e.g., AND, OR). |

## Core Concepts

### Boolean Algebra
At the foundation of all conditions is Boolean Algebra. Every logical expression must eventually resolve to a binary state. The primary operations—conjunction (AND), disjunction (OR), and negation (NOT)—form the basis for all complex decision-making.

### Evaluation Context
Logic does not exist in a vacuum. The "Context" refers to the set of variables, constants, and stateful information available to the evaluator at the moment a condition is checked. A condition's result is a function of its logic and its context.

### Predicate Composition
Complex logic is built by nesting or chaining predicates. The integrity of the system depends on the "Atomicity" of these predicates; each individual check should ideally be independent and side-effect-free.

## Standard Model

The standard model for 047 Conditions and Logic follows a hierarchical evaluation structure:

1.  **Input Acquisition:** The system gathers necessary data from the context.
2.  **Predicate Evaluation:** Individual conditions are tested against the data.
3.  **Logical Composition:** Individual results are combined using logical operators.
4.  **Branch Selection:** Based on the final Boolean result, the system selects the appropriate path (e.g., If-True path vs. If-False path).
5.  **Execution:** The system performs the action associated with the selected branch.

### The Ternary Model
A common abstraction for simple logic is the Ternary operation: `Condition ? Result_A : Result_B`. This represents the most concise form of the standard model.

## Common Patterns

### Guard Clauses
A pattern where "edge cases" or "invalid states" are evaluated at the beginning of a logic block to trigger an early exit. This reduces the complexity of the "happy path" logic.

### Decision Tables
A tabular representation used when multiple conditions combine to produce various outcomes. This is used to map complex input combinations to specific actions without deep nesting.

### Pattern Matching
An advanced form of branching where an expression is tested against a series of templates (patterns). It combines condition checking and variable binding into a single step.

### Fallback Logic
The "Default" or "Else" pattern, ensuring that a system remains deterministic by providing a guaranteed execution path if no specific conditions are met.

## Anti-Patterns

### The Arrow Anti-Pattern (Deep Nesting)
The accumulation of multiple nested `if` statements that move the code horizontally. This increases cognitive load and makes the logic difficult to audit or test.

### Side-Effect Predicates
Including logic within a condition that modifies the system state (e.g., an `if` statement that also writes to a database). This makes debugging nearly impossible as the act of checking a condition changes the environment.

### Magic Values
Hard-coding specific strings or numbers directly into logical expressions (e.g., `if (status == 4)`). This obscures the intent of the logic.

### Double Negatives
Using complex negative logic (e.g., `if (!isNotReady)`) which increases the likelihood of human error during maintenance.

## Edge Cases

### Three-Valued Logic (Null/Unknown)
In many systems, a condition may result in `True`, `False`, or `Unknown` (Null). Standard logic must define how `Unknown` interacts with operators (e.g., `Unknown AND False` is `False`, but `Unknown AND True` is `Unknown`).

### Floating-Point Comparison
Comparing two non-integer numbers for equality is a common failure point due to precision limits. Standard logic dictates using a "tolerance" or "epsilon" range rather than direct equality.

### Short-Circuit Side Effects
If a system relies on a function call within a logical expression, and that expression is short-circuited, the function will never run. This can lead to missing state updates if the developer assumes the function always executes.

### Race Conditions in Logic
In concurrent systems, the state being evaluated may change between the time the condition is checked and the time the resulting action is taken (Time-of-check to time-of-use).

## Related Topics
* **046 State Management:** Logic is often driven by the current state of the system.
* **048 Event-Driven Architecture:** Conditions often act as filters for which events trigger which handlers.
* **050 Error Handling:** Logic used to determine if a system state constitutes a failure.
* **092 Validation and Sanitization:** The use of logic to ensure input integrity.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |