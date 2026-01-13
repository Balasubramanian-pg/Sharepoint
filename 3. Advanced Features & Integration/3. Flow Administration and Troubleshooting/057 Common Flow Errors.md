# 057 Common Flow Errors

Canonical documentation for 057 Common Flow Errors. This document defines concepts, terminology, and standard usage.

## Purpose
The 057 Common Flow Errors framework exists to categorize, identify, and mitigate failures within automated logic sequences, state machines, and orchestration workflows. As systems move toward distributed architectures and low-code/procedural automation, the complexity of execution paths increases. This documentation provides a standardized taxonomy for understanding why flows fail, ensuring that architects and engineers can design resilient systems that handle exceptions gracefully rather than failing catastrophically.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logical failures within sequential or parallel execution paths.
* Data-driven execution errors and type mismatches.
* Connectivity and integration timeouts within a flow context.
* Resource exhaustion and governor limit breaches.
* State transition failures.

**Out of scope:**
* Specific vendor-specific error codes (e.g., Salesforce Apex exceptions, AWS Lambda specific exit codes).
* Hardware-level failures (e.g., bit rot, physical disk failure).
* User interface (UI) rendering bugs that do not affect the underlying logic flow.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Flow** | A sequence of operations or nodes designed to achieve a specific outcome based on input data and logic. |
| **Fault Path** | A secondary logic branch designed to execute only when a primary operation fails. |
| **Idempotency** | The property of a flow where multiple executions with the same input produce the same result without unintended side effects. |
| **Race Condition** | A failure occurring when the outcome of a flow depends on the sequence or timing of uncontrollable events. |
| **Deadlock** | A state where two or more flow instances are waiting for each other to release resources, preventing further progress. |
| **State Drift** | A condition where the actual state of a system diverges from the state expected by the flow logic. |

## Core Concepts
### The Deterministic Nature of Flows
A robust flow is expected to be deterministic: given the same state and the same input, it should produce the same output. Flow errors occur when non-deterministic factors (external API latency, changing data structures, or resource availability) interfere with the execution path.

### Error Propagation
Errors in a flow can be "bubbled up" or "trapped." Propagation refers to the movement of an error from a child element or sub-flow to the parent container. If an error is not trapped by a defined handler, it results in a terminal state for the entire execution.

### Transactionality and Atomicity
Flows often interact with databases or external systems. A core concept in flow error management is the "Atomic" unit—ensuring that if one part of a flow fails, all preceding parts are rolled back to prevent partial data states (the "Half-Baked Data" problem).

## Standard Model
The standard model for 057 Common Flow Errors categorizes failures into four primary domains:

1.  **Input/Data Errors:** Failures caused by null values, incorrect types, or schema mismatches that the flow logic is not equipped to handle.
2.  **Logic/Path Errors:** Failures where the flow reaches a "dead end" or an infinite loop because the branching logic did not account for a specific permutation of data.
3.  **Integration/External Errors:** Failures resulting from the unavailability, latency, or malformed responses of external services called during the flow.
4.  **Environment/Resource Errors:** Failures caused by the execution engine itself, such as memory limits, execution time limits, or concurrent thread exhaustion.

## Common Patterns
### The Try-Catch-Compensate Pattern
In this pattern, a flow attempts an action (Try). If an error occurs, it moves to a Fault Path (Catch). The flow then executes a "Compensating Transaction" to undo any partial changes made before the failure.

### The Exponential Backoff Retry
For transient errors (like a temporary network outage), the flow is designed to pause and retry the failed operation at increasing intervals (e.g., 1s, 2s, 4s, 8s) before finally declaring a terminal error.

### Dead Letter Queuing (DLQ)
When a flow instance fails irrecoverably, the state and input data are moved to a "Dead Letter" repository. This allows for manual inspection and re-injection into the flow once the underlying issue is resolved.

## Anti-Patterns
### The "Silent Failure"
Designing a flow that traps an error but does not log it or notify a supervisor. This leads to "Zombie Processes" where the system appears healthy, but business logic is not being completed.

### Hardcoded Logic Paths
Creating flows that rely on specific IDs or hardcoded strings. When the environment changes (e.g., moving from Sandbox to Production), the flow fails because the hardcoded dependencies do not exist in the new context.

### The Infinite Loop (Recursive Triggering)
A flow that updates a record, which then triggers the same flow to run again, leading to immediate resource exhaustion and system instability.

## Edge Cases
### Partial Commit on Timeout
A scenario where a flow initiates a multi-step database update. If the execution engine hits a timeout limit *during* the commit, some records may be updated while others are not, leading to data corruption.

### The "Thundering Herd"
When a system recovers from an outage and thousands of queued flows attempt to execute simultaneously, causing a secondary failure due to immediate resource exhaustion.

### Version Mismatch during Long-Running Flows
In flows that take days or weeks to complete (e.g., approval processes), the underlying flow definition may be updated while an instance is still "in flight." The instance may fail if it attempts to move to a node that no longer exists in the new version.

## Related Topics
*   **058 State Machine Persistence:** How state is saved between flow steps.
*   **042 Idempotency Standards:** Ensuring repeated executions are safe.
*   **109 Observability and Telemetry:** Standardized logging for flow execution.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |