# 013 When an Item is Modified Trigger

Canonical documentation for 013 When an Item is Modified Trigger. This document defines concepts, terminology, and standard usage.

## Purpose
The "When an Item is Modified" trigger exists to facilitate reactive programming and automated workflows within data-driven systems. Its primary purpose is to detect state changes in an existing discrete data entity (an "item") and initiate a subsequent set of actions or logic. This mechanism addresses the need for real-time or near-real-time data synchronization, audit logging, and business process automation without requiring constant manual oversight or inefficient bulk polling.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   The logic of change detection for existing records.
*   The lifecycle of a modification event from detection to execution.
*   Data integrity and state management during the trigger process.
*   Filtering and conditional execution based on specific attribute changes.

**Out of scope:**
*   "On Create" or "On Delete" triggers (unless a modification is functionally treated as a soft-delete).
*   Specific vendor-specific syntax (e.g., Power Automate expressions, SQL Trigger syntax, or Zapier webhooks).
*   Hardware-level interrupt triggers.

## Definitions
| Term | Definition |
|------|------------|
| Item | A single, unique entry or record within a data source (e.g., a row in a table, a document in a collection). |
| Trigger | A mechanism that monitors a data source and executes a predefined set of instructions when a specific event occurs. |
| Modification | An update operation that alters one or more attributes of an existing item without changing its unique identifier. |
| Payload | The data packet transmitted by the trigger, typically containing the current state of the item and, optionally, the previous state. |
| Delta | The specific difference between the pre-modification state and the post-modification state. |
| Idempotency | The property of a process where multiple identical modifications result in the same state, and the trigger logic can be executed multiple times without unintended side effects. |

## Core Concepts
### Event-Driven Architecture (EDA)
The "When an Item is Modified" trigger is a cornerstone of Event-Driven Architecture. It shifts the system from a "pull" model (where the consumer asks for updates) to a "push" model (where the producer notifies the consumer of updates).

### Change Detection Mechanisms
1.  **Polling:** The system periodically checks the data source for items with a "Last Modified" timestamp greater than the last check.
2.  **Webhooks/Callbacks:** The data source actively pushes a notification to a listener URL the moment a write operation occurs.
3.  **Transaction Log Sniffing:** The trigger monitors the database's internal transaction logs to identify updates at the engine level.

### State Awareness
A robust modification trigger is often "state-aware." It understands not just that a change happened, but *what* changed. This involves comparing the **Version N-1** (Previous) with **Version N** (Current).

## Standard Model
The standard model for a modification trigger follows a linear progression:

1.  **Persistence Event:** A user or system performs an update operation on an existing item.
2.  **Detection:** The monitoring agent identifies the change based on a unique identifier (ID) and a modification marker (timestamp, version number, or checksum).
3.  **Evaluation:** The system determines if the modification meets specific criteria (e.g., "Did the 'Status' field change?").
4.  **Payload Generation:** The system constructs a data object containing the item's attributes.
5.  **Dispatch:** The trigger fires, sending the payload to the subscriber or workflow engine.

## Common Patterns
*   **The Filtered Update:** The trigger is configured to fire only when specific fields are modified (e.g., "Only trigger if 'Price' changes"), reducing unnecessary executions.
*   **The Audit Trail:** Every modification trigger captures the user ID, timestamp, and delta, writing them to a secondary "History" table for compliance.
*   **Synchronization/Mirroring:** Using the trigger to update a secondary system (e.g., updating a CRM record when a Billing record is modified) to ensure data consistency across platforms.
*   **State Machine Transition:** Using the trigger to move a business process to the next stage (e.g., when "Approval Status" is modified to "Approved," trigger the "Shipment" workflow).

## Anti-Patterns
*   **The Infinite Loop (Recursive Triggering):** A trigger that updates the same item it is monitoring, causing it to fire again, leading to system exhaustion or stack overflow.
*   **The "God" Trigger:** A single modification trigger that contains logic for every possible field change, leading to complex, unmaintainable code and high latency.
*   **Polling Overload:** Setting a polling interval so frequent (e.g., every 1 second) that it degrades the performance of the source data system.
*   **Ignoring Idempotency:** Designing the triggered action such that if the trigger fires twice for the same modification (due to network retry), it creates duplicate or corrupted data.

## Edge Cases
*   **No-Op Updates:** A user hits "Save" without changing any data. The system must decide if this constitutes a "modification" based on whether the "Last Modified" timestamp updated or if the actual data values remained static.
*   **Bulk Updates:** When 10,000 items are modified in a single transaction. The system must handle whether to fire 10,000 individual triggers or one batch trigger.
*   **Race Conditions:** Two modifications occurring milliseconds apart. The trigger must ensure it processes the updates in the correct chronological order to avoid "lost updates."
*   **Soft Deletes:** When a modification sets an `is_deleted` flag to `true`. The system must determine if this should be handled by the "Modified" trigger or a specialized "Deleted" trigger.

## Related Topics
*   **012 When an Item is Created Trigger:** The precursor to modification, handling the initial entry of data.
*   **Change Data Capture (CDC):** The low-level architectural pattern for tracking changes in databases.
*   **Webhook Management:** The infrastructure used to transport trigger payloads.
*   **Optimistic Concurrency Control:** A method to prevent conflicting modifications.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |