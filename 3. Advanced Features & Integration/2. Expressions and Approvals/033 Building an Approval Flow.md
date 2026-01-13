# 033 Building an Approval Flow

Canonical documentation for 033 Building an Approval Flow. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of an approval flow is to establish a structured, repeatable, and auditable process for authorizing actions, expenditures, or changes within an organization. Approval flows serve as a governance mechanism to mitigate risk, ensure compliance with internal policies, and maintain quality control by requiring verification from designated stakeholders before a process proceeds to its next state.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logical structure and sequencing of approval steps.
* Roles and responsibilities within a flow.
* State management and transition logic.
* Auditability and notification requirements.
* Handling of exceptions and escalations.

**Out of scope:**
* Specific vendor software implementations (e.g., Power Automate, Jira Service Management, ServiceNow).
* User interface (UI) design for approval buttons or forms.
* Specific business rules for a particular industry (e.g., specific FDA or GDPR compliance steps).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Requestor** | The entity (human or system) that initiates the approval process by submitting a payload for review. |
| **Approver** | A designated individual or group with the authority to grant or deny a request. |
| **Delegate** | An authorized substitute who acts on behalf of an Approver when they are unavailable. |
| **State** | The current status of a request within the flow (e.g., Pending, Approved, Rejected, More Info Requested). |
| **Transition** | The movement from one state to another based on a decision or trigger. |
| **Escalation** | A mechanism to redirect a request to a higher authority or different path if a decision is not made within a defined timeframe. |
| **Quorum** | The minimum number of approvers required to reach a decision in a multi-approver scenario. |
| **Audit Trail** | A chronological record of all actions, comments, and state changes within the flow. |

## Core Concepts
### 1. Deterministic Logic
An approval flow must be deterministic. Given a specific set of inputs (e.g., dollar amount, department, risk level), the flow must follow a predictable path. Ambiguity in the routing logic leads to governance failures.

### 2. State Persistence
The flow must maintain a persistent state. If the system hosting the flow restarts or fails, the request must remain in its last known state with all associated metadata intact.

### 3. Separation of Duties
A fundamental security concept where the Requestor and the Approver should ideally be different entities to prevent fraud or errors. In automated systems, this is enforced through identity management.

### 4. Transparency and Feedback
Requestors must have visibility into the current status of their request and who is currently responsible for the next action. Conversely, Approvers must have access to all necessary context to make an informed decision.

## Standard Model
The standard model for an approval flow follows a linear or branching lifecycle:

1.  **Submission:** The Requestor submits data. The system validates the data against entry criteria.
2.  **Routing:** The system identifies the necessary Approvers based on predefined business rules (e.g., "If amount > $5,000, route to VP").
3.  **Notification:** Approvers are alerted via asynchronous communication.
4.  **Evaluation:** The Approver reviews the context. They may:
    *   **Approve:** Move to the next step or final state.
    *   **Reject:** Terminate the flow or return to the Requestor for correction.
    *   **Request Clarification:** Move to a "More Info" state, pausing the timeline.
5.  **Finalization:** Once all steps are satisfied, the system executes the post-approval action (e.g., updating a database, releasing funds) and notifies the Requestor.

## Common Patterns
*   **Sequential Approval:** Steps occur in a strict order (A must approve before B is notified).
*   **Parallel Approval:** Multiple approvers are notified simultaneously. The flow proceeds based on:
    *   *Consensus:* Everyone must approve.
    *   *First to Respond:* The first decision (Approve/Reject) dictates the outcome.
    *   *Quorum:* A specific percentage or number must approve.
*   **Conditional Branching:** The path changes based on data within the request (e.g., different departments follow different approval chains).
*   **Auto-Approval:** Requests meeting specific low-risk criteria are moved to "Approved" automatically by the system logic.

## Anti-Patterns
*   **Hardcoding Approvers:** Embedding specific individual names in the flow logic rather than using roles or dynamic lookups. This leads to broken flows when personnel change.
*   **Infinite Loops:** Designing a flow where a "Request Clarification" or "Rejection" can loop indefinitely without a terminal state or escalation.
*   **Silent Failures:** A flow that stalls because an approver is inactive, with no timeout or escalation logic to move the process forward.
*   **Lack of Context:** Providing an Approver with a "Yes/No" choice without the supporting documentation or data required to make the decision.

## Edge Cases
*   **The "Out of Office" Scenario:** An approver is unavailable. The system must handle delegation or automated redirection to prevent the flow from stagnating.
*   **Circular Dependencies:** A scenario where the logic routes a request back to a previous approver in a way that creates a logical loop.
*   **Retroactive Changes:** If the underlying data of a request is changed *while* it is in the approval queue, the flow should typically be reset or re-validated.
*   **Self-Approval:** When the Requestor is also the designated Approver for a specific step. Standard models usually require an override or an escalation to a higher authority to maintain separation of duties.

## Related Topics
*   **012 Identity and Access Management (IAM):** Defining who has the authority to act as an Approver.
*   **045 Audit Logging and Compliance:** The technical requirements for storing the history of the flow.
*   **078 Notification Services:** The mechanisms used to alert stakeholders of pending actions.
*   **092 State Machine Design:** The underlying computational theory for managing transitions.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |