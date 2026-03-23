# 035 Start and Wait for Approval Action

Canonical documentation for 035 Start and Wait for Approval Action. This document defines concepts, terminology, and standard usage.

## Purpose
The **035 Start and Wait for Approval Action** is a fundamental workflow pattern designed to integrate human decision-making into automated processes. Its primary purpose is to pause the execution of a computational sequence until a designated human actor (or group of actors) provides a formal response to a specific request.

This action addresses the "Human-in-the-Loop" (HITL) requirement in automation, where business logic requires subjective judgment, verification, or authorization that cannot be reliably performed by algorithmic means alone. By combining the initiation of the request and the suspension of the process into a single atomic operation, it ensures state consistency and process integrity.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The logical lifecycle of a synchronous-asynchronous approval request.
* State management during the "Wait" phase.
* Standardized response types and outcome handling.
* Theoretical boundaries of human-automated handoffs.

**Out of scope:**
* Specific vendor-specific UI/UX for approval buttons.
* Database schema implementations for storing approval history.
* Specific notification protocols (e.g., SMTP vs. Webhook).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Approver** | The human entity or role assigned the responsibility of reviewing and responding to the request. |
| **Outcome** | The final decision rendered by the approver (e.g., Approved, Rejected, Abstained). |
| **Wait State** | A period of process suspension where the workflow engine persists the current state and yields resources while awaiting an external signal. |
| **Timeout** | A predefined temporal boundary after which the action ceases waiting and triggers a failure or alternative logic path. |
| **Escalation** | A secondary logic path triggered when an approval remains pending beyond a specific threshold. |
| **Reassignment** | The act of transferring the approval authority from one Approver to another during an active Wait State. |

## Core Concepts
The 035 action operates on three core pillars:

1.  **State Persistence:** Unlike a simple "fire-and-forget" notification, the 035 action must capture the entire context of the workflow at the moment of invocation. This allows the process to resume exactly where it left off once the approval is received.
2.  **Synchronous Logic, Asynchronous Execution:** To the workflow designer, the action appears synchronous (the next step does not run until this one finishes). However, the underlying execution is asynchronous, as the system must handle a delay that could span minutes, days, or weeks.
3.  **Outcome-Driven Branching:** The action is inherently tied to conditional logic. The result of the 035 action is not merely "complete," but a specific value that dictates the subsequent path of the process.

## Standard Model
The standard model for the 035 Start and Wait for Approval Action follows a five-stage lifecycle:

1.  **Initialization:** The system generates a unique Approval ID and packages the necessary metadata (context, attachments, links).
2.  **Dispatch:** The system sends a notification to the Approver(s) via the configured communication channel.
3.  **Suspension:** The workflow engine moves the process instance into a "Waiting" or "Paused" state, releasing active compute resources.
4.  **Signal Reception:** An external trigger (the Approver's response) provides the Outcome and any associated comments back to the engine.
5.  **Resumption:** The engine validates the response, hydrates the process state, and moves to the next sequential action based on the Outcome.

## Common Patterns
*   **Single Approver:** A 1:1 relationship where one response determines the outcome.
*   **Consensus (Everyone must approve):** Requires a unanimous "Approve" outcome from a list of multiple Approvers. A single "Reject" typically terminates the process.
*   **First to Respond:** Multiple Approvers are notified, but the first response received determines the outcome for the entire group, and remaining requests are canceled.
*   **Sequential Approval:** A chain of 035 actions where the completion of one triggers the start of the next.

## Anti-Patterns
*   **Infinite Wait:** Failing to configure a Timeout, leading to "zombie" processes that consume tracking overhead indefinitely.
*   **Hardcoded Approvers:** Embedding specific user identifiers within the action rather than using dynamic roles or lookups, leading to process failure when personnel change.
*   **Opaque Requests:** Providing insufficient context in the approval request, forcing the Approver to leave the approval interface to find information, which increases latency and error rates.
*   **High-Frequency Approvals:** Using the 035 action for tasks that occur hundreds of times per hour for a single human, leading to "approval fatigue" and reduced scrutiny.

## Edge Cases
*   **Approver Unavailability:** If an Approver leaves the organization or is on leave while a process is in a Wait State, the process may stall. Systems should implement reassignment logic.
*   **Concurrent Responses:** In "First to Respond" patterns, if two approvers submit simultaneously, the system must implement a concurrency lock to accept the first and gracefully reject the second.
*   **System Restart/Upgrade:** If the workflow engine undergoes maintenance during a Wait State, the 035 action must be resilient enough to recover its "Waiting" status upon system restoration.
*   **Outcome Ambiguity:** When an Approver provides a response that does not map to defined outcomes (e.g., a "Request for Information" instead of "Approve/Reject").

## Related Topics
*   **State Machines:** For complex approval cycles that require moving backward and forward between stages.
*   **Notification Services:** The underlying transport layer for approval requests.
*   **Identity and Access Management (IAM):** For verifying the authenticity of the Approver.
*   **Audit Logging:** For maintaining a legal record of who approved what and when.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |