# 006 Instant Cloud Flows Button Flows

Canonical documentation for 006 Instant Cloud Flows Button Flows. This document defines concepts, terminology, and standard usage.

## Purpose
Instant Cloud Flows (specifically Button Flows) address the need for human-initiated, on-demand automation. While many automated processes rely on data-driven triggers (e.g., a record being created) or temporal triggers (e.g., a scheduled task), Button Flows provide a mechanism for users to execute complex logic at a specific moment of their choosing. This bridges the gap between manual human decision-making and automated backend execution.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The architecture of user-initiated manual triggers.
* Runtime input collection and validation logic.
* Execution context and identity delegation.
* The lifecycle of an on-demand automation request.

**Out of scope:**
* Specific UI/UX design for buttons in proprietary software.
* Vendor-specific licensing models for flow execution.
* Physical hardware buttons (IoT) unless functioning as a digital trigger proxy.

## Definitions
| Term | Definition |
|------|------------|
| **Manual Trigger** | An entry point for a workflow that requires a deliberate human action to initiate. |
| **Input Parameter** | A data field defined at design-time that must be populated by the user at runtime before execution. |
| **Execution Context** | The environment and metadata (user ID, timestamp, location) associated with the specific instance of a flow run. |
| **Payload** | The structured data package sent from the trigger interface to the automation engine. |
| **Synchronous Initiation** | A trigger model where the user waits for the system to acknowledge the start of the process. |
| **Identity Delegation** | The process by which the flow executes using either the permissions of the initiator or a predefined service principal. |

## Core Concepts

### 1. Manual Invocation
Unlike automated flows that "listen" for events, Button Flows remain dormant until an explicit signal is received. This signal is typically generated via a digital interface (mobile app, web portal, or integrated command).

### 2. Runtime Input Collection
A defining characteristic of Button Flows is the ability to request information from the user at the moment of execution. This allows the automation to be dynamic, using user-provided variables to branch logic or populate data fields in downstream systems.

### 3. Contextual Awareness
Button Flows can be "Global" (available anywhere) or "Contextual" (bound to a specific record or entity). Contextual flows automatically inherit the unique identifier of the record from which they were launched, reducing the need for manual data entry.

## Standard Model
The standard model for a Button Flow follows a linear progression:

1.  **Trigger Definition:** The developer defines the required inputs (text, boolean, file, etc.).
2.  **User Initiation:** The user selects the flow and provides the requested inputs.
3.  **Payload Construction:** The system bundles the user inputs with system metadata (Initiator ID, Timestamp).
4.  **Orchestration:** The automation engine receives the payload and executes the defined logic.
5.  **Feedback Loop:** The system provides a notification to the user regarding the success or failure of the initiation.

## Common Patterns

### The "On-Demand Report" Pattern
A user triggers a flow to aggregate data from multiple sources and email a PDF. The user provides the "Start Date" and "End Date" as runtime inputs.

### The "Manual Override" Pattern
In a system governed by automated logic, a Button Flow is used by administrators to force a state change or bypass standard validation rules for a specific record.

### The "Data Enrichment" Pattern
A user triggers a flow from within a CRM record to fetch external data (e.g., credit scores or social media profiles) and update the local record immediately.

## Anti-Patterns

### Over-Inputting
Requiring more than 5–7 input fields during a button trigger. This degrades the "instant" nature of the flow and increases the likelihood of user error. If extensive data is needed, a dedicated form or application is preferred.

### Polling via Button
Using a manual button to check for updates that could be handled by a webhook or an automated data-change trigger. This is an inefficient use of human resources.

### Lack of Feedback
Failing to provide the user with a confirmation that the flow has started or completed. Without a feedback loop, users often trigger the flow multiple times, leading to race conditions or duplicate data.

## Edge Cases

### Connectivity Loss during Input
If a user loses network connectivity after opening the input prompt but before submission, the execution context may expire, requiring the user to restart the process.

### Permission Mismatch
A scenario where a user has the permission to trigger a flow but lacks the permissions for the underlying connectors (e.g., trying to post to a database they cannot access). Canonical models must define whether the flow runs as "User" or "System."

### Concurrent Executions
When a button is pressed multiple times in rapid succession. Systems must implement "concurrency control" or "idempotency keys" to ensure that the same manual intent does not result in unintended duplicate actions.

## Related Topics
* **001 Trigger Architectures:** The broader study of how workflows begin.
* **015 Identity and Access Management (IAM):** Governing who can see and press specific buttons.
* **022 Input Validation Frameworks:** Ensuring data provided at the button prompt is sanitized and valid.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |