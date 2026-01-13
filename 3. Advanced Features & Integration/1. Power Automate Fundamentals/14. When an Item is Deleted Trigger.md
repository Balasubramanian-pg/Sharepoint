# 014 When an Item is Deleted Trigger

Canonical documentation for 014 When an Item is Deleted Trigger. This document defines concepts, terminology, and standard usage.

## Purpose
The **When an Item is Deleted Trigger** exists to provide a reactive mechanism for systems to respond to the permanent or logical removal of a discrete data entity from a structured data store. Its primary purpose is to facilitate downstream synchronization, maintain referential integrity across distributed systems, and initiate archival or auditing workflows that must occur immediately following the destruction of a record.

This trigger addresses the "State Transition Gap"—the moment a system moves from a state of "Existence" to "Non-existence"—ensuring that the loss of data in a primary system does not result in orphaned records or inconsistent states in secondary systems.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Event Signaling:** The mechanism by which a system broadcasts the removal of an item.
* **Payload Requirements:** The minimum data necessary to process a deletion event.
* **State Management:** Handling the transition from active data to tombstoned or purged data.
* **Trigger Timing:** The distinction between pre-commit and post-commit deletion signals.

**Out of scope:**
* **Vendor-specific API syntax:** (e.g., specific JSON schemas for AWS EventBridge, Power Automate, or Webhooks).
* **UI/UX Design:** How a "Delete" button is rendered or confirmed by a user.
* **Storage Mechanics:** The physical bit-level erasure of data on disk.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Item** | The smallest discrete unit of data (record, object, or file) that can be uniquely identified and deleted. |
| **Trigger** | An event-driven signal that initiates a secondary process or workflow based on a specific state change. |
| **Hard Delete** | The permanent physical removal of a record from the database, making it unrecoverable by standard queries. |
| **Soft Delete** | A logical deletion where a record is marked as "deleted" (often via a flag) but remains in the database for archival or recovery purposes. |
| **Tombstone** | A minimal record or marker left behind after a deletion to inform distributed systems that an item no longer exists. |
| **Payload** | The data packet sent by the trigger, typically containing the unique identifier of the deleted item and metadata about the deletion. |
| **Idempotency** | The property of a process where multiple identical deletion signals result in the same system state as a single signal. |

## Core Concepts
### The Data Vacuum Problem
The fundamental challenge of a deletion trigger is that the subject of the trigger (the Item) no longer exists in the primary store at the time of execution. Therefore, the trigger must provide a "snapshot" or a "tombstone" of the item’s identity to allow downstream systems to identify which record to act upon.

### Event Timing
*   **Synchronous (Pre-deletion):** The trigger fires before the data is removed. This allows for validation or cancellation of the delete action.
*   **Asynchronous (Post-deletion):** The trigger fires after the data is removed. This is the standard model for "When an Item is Deleted," as it ensures the trigger only fires for successful operations.

### Scope of Influence
A deletion trigger may be **Local** (affecting the immediate database) or **Global** (propagating through a message bus to external microservices).

## Standard Model
The standard model for a Deletion Trigger follows the **Event-Notification Pattern**. 

1.  **Originating Action:** A user or system issues a `DELETE` command.
2.  **Commitment:** The data store validates the request and removes the record.
3.  **Signal Generation:** Upon successful commit, the system generates a deletion event.
4.  **Payload Distribution:** The system broadcasts a payload containing:
    *   `Unique_ID`: The primary key of the deleted item.
    *   `Timestamp`: When the deletion occurred.
    *   `Actor_ID`: Who or what initiated the deletion.
    *   `Context_Metadata`: (Optional) The parent container or category of the item.
5.  **Downstream Consumption:** Subscribers receive the ID and perform cleanup (e.g., deleting related files, removing search index entries).

## Common Patterns
*   **The Cascading Cleanup:** Using the trigger to remove child records in a separate database that does not support native foreign key constraints.
*   **Audit Logging:** Capturing the deletion event in a permanent, immutable ledger for compliance.
*   **Cache Invalidation:** Using the trigger to purge the deleted item from a Content Delivery Network (CDN) or an in-memory cache (e.g., Redis).
*   **Synchronization Mirroring:** Ensuring that if an item is deleted in a System of Record (SoR), it is also removed from all downstream "Read Models."

## Anti-Patterns
*   **Heavy Payload Dependency:** Including the entire object's data in the deletion payload. This increases network overhead and may violate privacy regulations (e.g., GDPR "Right to be Forgotten") if the payload is stored in logs.
*   **Circular Deletion:** Setting up triggers that cause System A to delete from System B, which then triggers a delete back in System A, potentially causing infinite loops.
*   **Post-Delete Retrieval:** Designing a workflow that attempts to "Fetch Item Details" using the ID provided in the trigger. Since the item is deleted, this fetch will fail.
*   **Ignoring Idempotency:** Failing to handle cases where the same deletion trigger is delivered twice, leading to errors in downstream systems that expect the item to still exist during the second execution.

## Edge Cases
*   **Bulk Deletion:** When 1,000,000 items are deleted simultaneously. Does the system fire 1,000,000 individual triggers (potentially DDOSing the subscriber) or one batch trigger?
*   **Recursive Deletion:** If a folder is deleted, does the trigger fire only for the folder, or for every individual item inside the folder?
*   **Undelete/Restore:** If an item is restored from a "Recycle Bin," the system must ensure that the original deletion trigger's effects are either reversible or that a "Created" trigger is fired to re-sync the data.
*   **Race Conditions:** A "Delete" trigger arriving at a subscriber before the "Create" trigger for the same item due to network latency or out-of-order delivery.

## Related Topics
*   **011 When an Item is Created Trigger:** The inverse event.
*   **012 When an Item is Modified Trigger:** The update event.
*   **Event-Driven Architecture (EDA):** The overarching architectural style.
*   **Referential Integrity:** The consistency requirements between coupled data stores.
*   **Tombstone Patterns:** Strategies for persisting deletion knowledge in distributed systems.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |