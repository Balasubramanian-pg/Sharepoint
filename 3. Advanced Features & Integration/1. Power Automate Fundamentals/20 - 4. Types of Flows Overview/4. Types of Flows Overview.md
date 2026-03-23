# 004 Types of Flows Overview

Canonical documentation for 004 Types of Flows Overview. This document defines concepts, terminology, and standard usage.

## Purpose
The concept of "Flows" addresses the requirement to structure, visualize, and execute a sequence of operations that transform an initial state or input into a desired output or end state. By categorizing flows into distinct types, architects and systems designers can apply appropriate governance, error-handling strategies, and performance optimizations suited to the specific nature of the data or control movement. This documentation provides a taxonomy for these execution paths to ensure consistency across complex systems.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Classification of flow architectures (e.g., synchronous vs. asynchronous).
* Structural patterns of logic execution.
* Theoretical boundaries between automated and manual intervention points.
* Data and control flow characteristics.

**Out of scope:**
* Specific vendor implementations (e.g., Power Automate, Salesforce Flows, Apache Airflow).
* Programming language-specific syntax.
* Hardware-level instruction pipelining.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Flow** | A directed sequence of operations or tasks designed to achieve a specific outcome. |
| **Statefulness** | The property of a flow that requires the system to remember preceding events or user interactions to inform current execution. |
| **Determinism** | A characteristic where a flow, given the same initial conditions and inputs, will always produce the same output. |
| **Orchestration** | A centralized approach to flow management where a single controller directs the sequence of activities. |
| **Choreography** | A decentralized approach where individual components react to events and interact without a central coordinator. |
| **Idempotency** | The property of a flow or operation where multiple identical executions have the same effect as a single execution. |

## Core Concepts

### 1. Directionality and Topology
Flows are fundamentally directed. They move from a trigger (entry point) toward a resolution (exit point). The topology may be linear, branching (conditional), or cyclical (iterative), depending on the logic requirements.

### 2. Temporal Coupling
Flows are categorized by how tightly the components are bound in time. 
* **Synchronous flows** require the caller and receiver to be available simultaneously.
* **Asynchronous flows** decouple the request from the response, allowing for background processing and increased system resilience.

### 3. Data vs. Control Flow
* **Data Flow:** Focuses on the transformation and movement of information between storage or processing nodes.
* **Control Flow:** Focuses on the order of execution and the logic gates (if/then/else) that dictate which path is taken.

## Standard Model

The standard model for flow classification is based on the **Execution Context** and **Interaction Pattern**.

### The Execution Context Model
1.  **Automated Flows:** Triggered by system events (e.g., a database update, a timer, or an API call) and require no human intervention.
2.  **Human-in-the-Loop (HITL) Flows:** Require manual input or approval at specific checkpoints to proceed.
3.  **Scheduled Flows:** Executed at predefined intervals or specific timestamps, often used for batch processing.

### The Interaction Pattern Model
*   **Request-Response:** A bidirectional flow where the initiator waits for a result.
*   **Fire-and-Forget:** A unidirectional flow where the initiator triggers an action and immediately moves to the next task without waiting for a confirmation of completion.

## Common Patterns

### Sequential Processing
The simplest form where Task A must complete before Task B begins. This is used when Task B is dependent on the output of Task A.

### Fan-Out / Fan-In
*   **Fan-Out:** A single trigger initiates multiple parallel paths to process data concurrently.
*   **Fan-In:** Multiple parallel paths converge into a single join point, usually requiring all paths to complete before proceeding.

### Long-Running Flows
Flows that may take hours, days, or weeks to complete (e.g., an insurance claim process). These require persistent state management to survive system restarts.

## Anti-Patterns

### The "God Flow"
A single, monolithic flow that attempts to handle every possible business scenario. This leads to high complexity, difficult debugging, and significant downtime during updates.

### Circular Dependencies
Designing flows that call each other in a loop without a clear termination condition, leading to resource exhaustion.

### Hard-Coded Logic Gates
Embedding volatile business rules directly into the flow structure rather than using an externalized rules engine or configuration layer.

### Lack of Idempotency in Retries
Designing a flow that performs non-idempotent actions (like charging a credit card) without checking if the action was already completed during a previous failed attempt.

## Edge Cases

### Race Conditions in Parallel Paths
When two parallel branches of a flow attempt to update the same resource simultaneously, the final state depends on which branch finishes last.

### Partial Completion (Atomic Failure)
In a multi-step flow, if step 4 of 5 fails, the system may be left in an inconsistent state. Canonical flows must define "Compensating Transactions" to roll back or neutralize the effects of the first three steps.

### Ghost Triggers
Scenarios where a flow is initiated by a transient system error or a duplicate event notification, requiring deduplication logic at the entry point.

## Related Topics
*   **001 Event-Driven Architecture:** The underlying philosophy for asynchronous flows.
*   **008 Error Handling and Retry Policies:** Standardized methods for managing flow interruptions.
*   **012 State Management:** Deep dive into persisting flow data across sessions.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |