# 037 Handling Approval Outcomes

Canonical documentation for 037 Handling Approval Outcomes. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Handling Approval Outcomes is to define the systematic transition of a process from a decision state to its subsequent operational state. This topic addresses the critical gap between the moment a decision is rendered by an authority and the execution of the resulting actions. It ensures that outcomes are processed with integrity, consistency, and traceability, preventing "orphaned" decisions or inconsistent system states.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **State Transition Logic:** The mechanics of moving an entity from a "Pending" state to a terminal or intermediate state.
* **Post-Decision Orchestration:** The triggering of downstream events based on the specific outcome.
* **Integrity and Validation:** Ensuring the outcome is valid, authorized, and recorded.
* **Communication Protocols:** Standardized methods for notifying stakeholders of the result.

**Out of scope:**
* **Decision-Making Logic:** The criteria or algorithms used to arrive at a decision (e.g., business rules, AI models).
* **User Interface Design:** The specific buttons or forms used to capture an approval.
* **Identity Management:** The underlying technology used to authenticate the approver.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Outcome** | The final status assigned to a request (e.g., Approved, Rejected, Deferred). |
| **Terminal State** | A state from which no further transitions are possible within the current workflow context. |
| **Intermediate State** | A state requiring further action or input before reaching a terminal state (e.g., "Request Changes"). |
| **Outcome Handler** | The logic or component responsible for executing the side effects of a decision. |
| **Idempotency** | The property where an outcome can be processed multiple times without changing the result beyond the initial application. |
| **Audit Trail** | A chronological record of the decision, the decider, the timestamp, and the resulting actions. |

## Core Concepts

### Determinism
Every approval outcome must lead to a predictable and repeatable set of actions. Given the same input and state, the outcome handler must produce the same system changes.

### Atomicity
The processing of an outcome should be atomic. If an approval triggers three downstream actions (e.g., updating a database, sending an email, and starting a shipment), either all three must succeed, or the system must roll back to the pre-decision state to avoid partial processing.

### Authorization Persistence
The outcome record must capture not just the "what" (the decision) but the "who" and "why" (the authority and justification). This context must be persisted alongside the state change to satisfy compliance and governance requirements.

## Standard Model

The standard model for handling approval outcomes follows a linear progression:

1.  **Outcome Capture:** The system receives a decision signal (Approve, Reject, etc.) and associated metadata (comments, timestamps).
2.  **Validation:** The system verifies that the decision-maker has the requisite authority and that the request is still in a state eligible for that decision.
3.  **State Transition:** The primary entity's status is updated in the system of record.
4.  **Downstream Execution:** The Outcome Handler triggers secondary processes (e.g., API calls, webhooks, or internal service activations).
5.  **Notification Dispatch:** Stakeholders are informed of the outcome through defined communication channels.
6.  **Finalization:** The transaction is committed to the audit log, and the workflow instance is closed or moved to the next stage.

## Common Patterns

### The "Request Changes" Loop
Instead of a binary Approve/Reject, the outcome is "Incomplete." The handler reverts the entity to an editable state and notifies the initiator, effectively restarting a portion of the lifecycle.

### Conditional Branching
The outcome handler evaluates the decision and routes the process to different sub-workflows. For example, an "Approved" outcome for a high-value item may trigger an additional "Executive Review" rather than a terminal "Finalized" state.

### Asynchronous Processing
For outcomes that trigger heavy computational tasks or third-party integrations, the handler acknowledges the decision immediately but processes the side effects in the background to ensure system responsiveness.

## Anti-Patterns

*   **Silent Failures:** Processing an outcome (e.g., marking a request as "Approved") but failing to trigger the downstream action without alerting administrators.
*   **Hard-Coded Side Effects:** Embedding specific business logic (like "if approved, email Bob") directly into the database trigger rather than using a dedicated orchestration layer.
*   **Mutable Audit Logs:** Allowing the record of an approval outcome to be modified or deleted after the fact.
*   **Optimistic State Updates:** Updating the status to "Approved" before validating that the downstream systems are capable of accepting the change.

## Edge Cases

*   **Authority Revocation:** A decision is submitted, but before the outcome is processed, the approver's permissions are revoked. The handler must decide whether to honor the "stale" authority or void the outcome.
*   **Race Conditions:** Two approvers submit conflicting decisions (e.g., Approve and Reject) at the exact same millisecond. The system must implement locking mechanisms to ensure only the first processed decision is valid.
*   **Expired Tokens:** An outcome is submitted via a link or token that has expired during the time the user spent reviewing the document.
*   **Dependency Failure:** An approval is granted, but the downstream system required to fulfill the request is offline. The handler must manage retries or move the request to a "Pending Fulfillment" error state.

## Related Topics
*   **012 Workflow State Machines:** The underlying architecture that governs transitions.
*   **045 Audit Logging Standards:** Requirements for recording the outcome history.
*   **088 Notification Services:** The mechanisms used to alert stakeholders of outcomes.
*   **102 Identity and Access Management (IAM):** Verification of the decider's authority.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |