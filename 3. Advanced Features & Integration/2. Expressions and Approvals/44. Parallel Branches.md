# 044 Parallel Branches

Canonical documentation for 044 Parallel Branches. This document defines concepts, terminology, and standard usage.

## Purpose
The 044 Parallel Branches pattern exists to facilitate the concurrent execution of multiple distinct logic paths within a single process or workflow. Its primary objective is to optimize execution time by performing independent operations simultaneously rather than sequentially. This pattern addresses the requirement for complex state transitions where multiple conditions or actions must be satisfied or completed before a process can advance to a subsequent unified state.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logical structures for forking and joining execution paths.
* Synchronization mechanisms and barrier logic.
* State management across concurrent execution contexts.
* Error propagation and termination behavior in parallel environments.

**Out of scope:**
* Specific vendor implementations (e.g., AWS Step Functions "Parallel" state, GitHub Actions "Matrix", or Jenkins "Parallel" blocks).
* Low-level CPU multi-threading or hardware-level parallelism.
* Network-level load balancing.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Branch** | A discrete sequence of operations or states that executes independently of other sequences within the same parent process. |
| **Fork (Fan-out)** | The transition point where a single execution path splits into two or more parallel branches. |
| **Join (Fan-in)** | The synchronization point where multiple parallel branches converge back into a single execution path. |
| **Barrier** | A synchronization primitive that prevents the process from advancing until a defined set of branches has reached a specific state. |
| **Race Condition** | A flaw where the output or state depends on the uncontrollable sequence or timing of parallel branch completion. |
| **Partial Success** | A state where some branches complete successfully while others fail, requiring a defined resolution strategy. |

## Core Concepts

### Concurrency vs. Parallelism
In the context of 044 Parallel Branches, "Parallelism" refers to the logical simultaneous execution of branches. While the underlying infrastructure may execute these branches concurrently (interleaved) or in true parallel (simultaneous hardware execution), the logical model treats them as happening at the same time.

### State Isolation
Each branch should ideally operate within its own isolated scope. While branches may read from a shared parent state, writing to shared variables without locking mechanisms introduces non-deterministic behavior.

### Synchronization and Convergence
The lifecycle of parallel branches is governed by the Join. A Join acts as a "wait" state. The process cannot proceed to the next step until the criteria of the Join (e.g., "All complete," "Any one completes," or "N of M complete") are met.

## Standard Model

The standard model for 044 Parallel Branches follows a **Fork-Join** architecture:

1.  **Entry Point:** A single transition enters the Parallel construct.
2.  **Distribution:** The process state is cloned or partitioned for each branch.
3.  **Independent Execution:** Each branch executes its defined logic. Branches do not communicate with each other during execution.
4.  **Synchronization Barrier:** All branches (or a required subset) must reach the end of their logic.
5.  **State Recomposition:** The results from each branch are merged back into the primary process state.
6.  **Exit Point:** A single transition exits the Parallel construct.

## Common Patterns

### Static Branching
The number of branches and the logic within them are defined at design time. This is used when the parallel tasks are heterogeneous (e.g., "Process Payment" and "Update Inventory" happening simultaneously).

### Dynamic Branching (Parallel Map)
The number of branches is determined at runtime based on an input collection. The same logic is applied to each item in the collection in parallel.

### Race (First-of-N)
Multiple branches are started, but only the result of the first branch to complete is used. All other branches are typically terminated or ignored. This is common in high-availability lookups.

### Optional Completion
A model where the process continues if a "critical path" branch completes, even if "non-critical" branches are still processing.

## Anti-Patterns

### The Longest Pole
Designing parallel branches where one branch takes significantly longer than all others, negating the time-saving benefits of parallelism and leaving resources idle.

### Shared Resource Contention
Multiple branches attempting to modify the same external resource (e.g., a database row or a file) without an external orchestration or locking mechanism, leading to data corruption.

### Deep Nesting
Nesting parallel branches within parallel branches to an excessive degree. This increases cognitive load, complicates debugging, and can lead to resource exhaustion.

### Silent Failures
Configuring a Join that proceeds on "Any" completion without properly handling or logging the failures of the branches that did not finish.

## Edge Cases

### Empty Input Sets
In dynamic branching, if the input collection is empty, the system must define whether the Parallel block is skipped, returns an error, or immediately proceeds to the Join.

### Infinite Loops in a Single Branch
If one branch enters an infinite loop or hangs, the Join barrier may never be reached. Canonical implementations must include a "Timeout" mechanism at the Parallel block level.

### Partial Failure and Rollback
If three branches succeed and one fails, the system must determine if the successful branches should be "undone" (Saga pattern) or if the partial state is acceptable.

### Branch Interdependency
If Branch B requires an output from Branch A, they are not truly parallel. Attempting to force them into a parallel structure usually results in deadlocks or race conditions.

## Related Topics
* **012 State Machines:** The foundational logic for transitions.
* **088 Error Handling and Retries:** Strategies for managing branch-level failures.
* **102 Distributed Locking:** Mechanisms for managing shared resources across branches.
* **Saga Pattern:** Managing long-running transactions across parallel distributed systems.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |