# 017 For a Selected Item Trigger

Canonical documentation for 017 For a Selected Item Trigger. This document defines concepts, terminology, and standard usage.

## Purpose
The **017 For a Selected Item Trigger** exists to facilitate human-in-the-loop automation. It addresses the requirement for non-deterministic, on-demand execution of workflows where a system cannot automatically predict when a process should run based on data changes alone. 

This trigger mechanism allows a user to bridge the gap between a static data repository and an active business process by manually designating a specific entity (or set of entities) as the subject of an automated sequence. It transforms a passive record into an active context for logic execution.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The conceptual framework of manual invocation within a data-centric UI.
* The transfer of contextual metadata from a host application to an execution engine.
* The lifecycle of a selection-based event.
* Input schema requirements for user-provided parameters at the time of triggering.

**Out of scope:**
* Specific vendor UI implementations (e.g., specific ribbon buttons in SharePoint or Salesforce).
* Programming language-specific SDKs.
* Hardware-level interrupt handling.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Selection Context** | The specific data record, file, or object instance currently highlighted or chosen by the user within the host application. |
| **Invocation Event** | The discrete action (e.g., a click or command) that signals the system to begin the trigger sequence. |
| **Contextual Metadata** | Information automatically passed by the trigger, such as the Unique Identifier (UID), timestamp, and user identity of the initiator. |
| **Input Schema** | A predefined set of fields presented to the user at the moment of triggering to collect additional runtime variables. |
| **Host Application** | The environment where the data resides and where the user performs the selection. |

## Core Concepts
### Manual Initiation
Unlike "On Create" or "On Update" triggers, the 017 trigger is strictly manual. It relies on human judgment to determine the appropriateness of the workflow execution.

### Contextual Awareness
The trigger must inherently "know" which item was selected. The primary payload of the trigger is the unique identifier of the selected item, which allows the subsequent workflow to query the host application for the full state of that item.

### Runtime Inputs
A core concept of this trigger is the ability to accept "just-in-time" data. Because the trigger is manual, it provides an opportunity for the user to provide parameters that influence the workflow's path (e.g., selecting a "Reason for Review" from a dropdown when triggering an approval).

## Standard Model
The standard model for a 017 For a Selected Item Trigger follows a four-stage lifecycle:

1.  **Selection State:** The user interacts with the Host Application and identifies one or more target records.
2.  **Invocation & Parameterization:** The user initiates the trigger. If an Input Schema is defined, the system prompts the user for additional data.
3.  **Payload Handshake:** The Host Application packages the Selection Context (ID, User Info) and the Runtime Inputs into a standardized message format (usually JSON).
4.  **Execution Handoff:** The message is sent to the workflow engine, which instantiates the process using the provided context.

## Common Patterns
*   **The Enrichment Pattern:** A user selects a "lead" or "contact" and triggers a workflow to fetch third-party data and update the record.
*   **The Gateway Pattern:** A user selects a document and triggers a "Submit for Approval" workflow, which moves the item through various state changes.
*   **The Bulk Action Pattern:** A user selects multiple items and triggers a single workflow instance that iterates through the collection to perform a standardized update.
*   **The Document Generation Pattern:** A user selects a data record and triggers a process that injects that data into a template to produce a PDF or report.

## Anti-Patterns
*   **Polling for Selection:** Attempting to simulate a selection trigger by constantly checking if a "Selected" flag has been toggled in a database. This is inefficient and introduces latency.
*   **Over-Parameterization:** Requiring users to input data that already exists within the Selection Context. This leads to data entry errors and user fatigue.
*   **Implicit State Assumptions:** Designing the trigger to assume the item is in a specific state (e.g., "Draft") without including a validation step at the start of the workflow.
*   **Heavy Payload Transfer:** Passing the entire object data in the trigger payload rather than just the ID. This can lead to "stale data" issues if the record changes between the trigger and the execution.

## Edge Cases
*   **Race Conditions:** If a user selects an item and triggers a workflow, but another user deletes the item before the workflow engine can fetch its details.
*   **Permissions Mismatch:** The user has permission to "select" and "trigger" in the Host Application, but the service account running the background workflow lacks permission to access the item's data.
*   **Multi-Select Ambiguity:** When a user selects multiple items, the system must determine if it should fire $N$ individual workflow instances or one instance with an array of $N$ IDs.
*   **UI Latency:** The user triggers the action, but due to network lag, clicks the trigger button multiple times, potentially spawning duplicate workflow instances.

## Related Topics
*   **018 On-Demand Button Trigger:** Similar to 017, but typically not bound to a specific data record (global context).
*   **Webhook Callbacks:** The underlying mechanism often used to transport the trigger payload.
*   **Identity Propagation:** The process of ensuring the workflow acts on behalf of the user who triggered it.
*   **State Machines:** Often used in conjunction with selection triggers to manage the lifecycle of the selected item.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |