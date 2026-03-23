# 056 Flow Run History

Canonical documentation for 056 Flow Run History. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Flow Run History is to provide a persistent, immutable record of the execution of automated workflows. It serves as the primary mechanism for observability, auditing, and forensic analysis within automated systems. By capturing the state, transitions, and outcomes of a flow at a specific point in time, Flow Run History enables operators to verify system integrity, troubleshoot failures, and meet compliance requirements.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Data structures representing execution instances.
* Lifecycle of execution metadata.
* Persistence and retention principles.
* Relationship between flow definitions and execution instances.

**Out of scope:**
* Specific vendor database schemas or UI layouts.
* Real-time telemetry (streaming logs) prior to persistence.
* Specific authentication mechanisms for accessing history.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Flow** | A defined sequence of operations or logic intended to achieve a specific outcome. |
| **Run (Instance)** | A single, discrete execution of a Flow. |
| **State** | The condition of a Run at a specific point in time (e.g., Queued, Running, Succeeded, Failed). |
| **Payload** | The data passed into (Input) or produced by (Output) a Flow Run or its individual steps. |
| **Metadata** | Contextual information about the Run, such as Trigger Source, Duration, and User Identity. |
| **Retention Policy** | The set of rules governing how long Run History is preserved before deletion or archiving. |
| **Correlation ID** | A unique identifier used to track a single request or transaction across multiple flows or services. |

## Core Concepts

### Immutability
Once a Flow Run has reached a terminal state (Success, Failure, or Cancelled), its history must be treated as immutable. Altering the record of what occurred undermines the integrity of the audit trail.

### Traceability
Flow Run History must provide a clear "thread" from the triggering event to the final output. This includes the ability to see which version of a Flow definition was active at the time of execution.

### Granularity
History exists at multiple levels:
1.  **Flow Level:** High-level status, start/end times, and overall result.
2.  **Step Level:** Detailed breakdown of individual actions within the flow, including their specific inputs, outputs, and durations.
3.  **Log Level:** Low-level diagnostic messages emitted during execution.

## Standard Model

The standard model for Flow Run History follows a hierarchical structure:

1.  **Execution Header:**
    *   `RunID`: Unique identifier.
    *   `FlowID` & `Version`: Reference to the logic being executed.
    *   `Status`: Current or final state.
    *   `Timestamps`: Created, Started, and Completed.
    *   `Trigger Context`: Who or what initiated the run.

2.  **Execution Body (Steps):**
    *   A collection of step records, each containing a `StepID`, `Status`, `Start/End Time`, and `Error Details` if applicable.
    *   **Input/Output Mapping:** References to the data consumed and produced by each step.

3.  **Contextual Metadata:**
    *   Environment variables, system configurations, and Correlation IDs.

## Common Patterns

### The Parent-Child Pattern
In complex systems, a "Parent" flow may trigger one or more "Child" flows. The Run History should maintain bidirectional references (ParentID in the child record, and a list of ChildIDs in the parent record) to allow for nested navigation.

### The Snapshot Pattern
To ensure accuracy during debugging, the Run History often includes a "snapshot" of the Flow definition as it existed at the moment of execution, preventing confusion if the Flow is updated later.

### Externalized Artifacts
For flows involving large data payloads (e.g., file processing), the Run History stores metadata and pointers (URIs) to the data rather than embedding the raw data directly into the history record.

## Anti-Patterns

*   **Sensitive Data Leakage:** Storing unencrypted credentials, PII (Personally Identifiable Information), or secrets within the Run History payloads.
*   **Infinite Retention:** Failing to define a retention policy, leading to degraded system performance and excessive storage costs.
*   **Log Overwhelming:** Capturing excessive low-level debug information in the primary history record, which obscures critical state transition data.
*   **Mutable History:** Allowing users or processes to modify the status or timestamps of a completed run.

## Edge Cases

*   **Zombies/Orphaned Runs:** A run that is marked as "Running" but the underlying compute resource has crashed. Standard models must include a "heartbeat" or a timeout mechanism to transition these to a "Timed Out" or "Unknown" state.
*   **Circular References:** In recursive flow patterns, history must be capped or depth-limited to prevent infinite history generation.
*   **Partial Success:** Flows that complete with "Warnings" or "Soft Failures" require a nuanced status beyond a simple binary Success/Failure.
*   **Clock Skew:** When flows run across distributed systems, timestamps in the history may appear out of order if not normalized to a standard (e.g., UTC).

## Related Topics
*   **012 Workflow Orchestration:** The logic that generates run history.
*   **088 Audit Logging:** The broader category of security-focused event logging.
*   **104 Data Retention & Archiving:** The management of historical data over time.
*   **210 Observability Frameworks:** The systems used to visualize and alert on run history.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |