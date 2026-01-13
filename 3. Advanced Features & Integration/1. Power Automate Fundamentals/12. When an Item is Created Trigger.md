# 012 When an Item is Created Trigger

Canonical documentation for 012 When an Item is Created Trigger. This document defines concepts, terminology, and standard usage.

## Purpose
The **012 When an Item is Created Trigger** serves as the foundational entry point for event-driven architectures and automated workflows. Its primary purpose is to detect the transition of a data entity from a state of non-existence to a state of persistence within a system of record. 

By providing a reactive mechanism rather than a proactive polling mechanism, this trigger addresses the problem of latency and resource inefficiency in data synchronization, business process automation, and real-time notification systems. It ensures that downstream consumers are informed immediately upon the successful initialization of a discrete data unit.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   The lifecycle of a "Creation Event."
*   Payload requirements and metadata standards for initialization triggers.
*   Theoretical boundaries between "Creation" and "Update" events.
*   Subscriber notification patterns.

**Out of scope:**
*   Specific vendor implementations (e.g., Power Automate, Zapier, AWS Lambda).
*   UI/UX configurations for setting up triggers.
*   Physical database transaction logs or low-level disk I/O.

## Definitions
| Term | Definition |
|------|------------|
| **Item** | A discrete, identifiable unit of data (record, object, or document) within a data store. |
| **Trigger** | A mechanism that monitors a data store for a specific state change and initiates a subsequent action. |
| **Payload** | The data packet transmitted by the trigger, typically containing the item's unique identifier and its initial attributes. |
| **Idempotency** | The property of a process where multiple identical requests have the same effect as a single request. |
| **Eventual Consistency** | A theoretical guarantee that, provided no new updates are made to an item, all accesses to that item will eventually return the last updated value. |
| **System of Record (SoR)** | The authoritative data source for a given data element or piece of information. |

## Core Concepts

### Event-Driven Initiation
The 012 Trigger operates on the principle of "Push" logic. Unlike polling, where a client periodically asks the server if new items exist, the 012 Trigger relies on the System of Record to broadcast the existence of the new item to registered subscribers.

### Atomic Persistence
A valid 012 Trigger event must only fire *after* the item has been successfully committed to the data store. If a transaction is rolled back, the trigger must not execute. This ensures data integrity between the trigger source and the subscriber.

### Immutable Identity
Upon creation, an item is assigned a Unique Identifier (UID). This UID is a core component of the trigger payload, allowing downstream systems to reference the item back to the System of Record.

## Standard Model

The standard model for the 012 Trigger follows a linear progression:

1.  **Request:** A user or system submits a request to create a new item.
2.  **Validation:** The System of Record validates the schema and constraints.
3.  **Commitment:** The item is persisted in the database/storage layer.
4.  **Event Generation:** The system generates a "Created" event containing the item's initial state.
5.  **Dispatch:** The trigger engine identifies subscribers and dispatches the payload.
6.  **Execution:** Downstream workflows or services consume the payload and perform secondary logic.

## Common Patterns

### The Enrichment Pattern
The trigger initiates a process that fetches additional data from external sources to populate secondary fields in the newly created item.

### The Notification Pattern
The trigger sends an alert (email, SMS, webhook) to a stakeholder or system to signal that a new entity requires attention.

### The Synchronization Pattern
The trigger acts as a catalyst to replicate the new item across disparate systems to maintain data parity.

### The Validation/Correction Pattern
The trigger fires a logic sequence that checks the new item against complex business rules that cannot be enforced at the database schema level, potentially flagging or deleting the item if it fails.

## Anti-Patterns

*   **The Infinite Loop:** Configuring a 012 Trigger to create a new item in the same data store that fires the trigger, leading to recursive execution.
*   **The "God" Trigger:** Attaching excessive, synchronous logic to a single creation event, causing performance degradation in the System of Record.
*   **Assuming Immediate Availability:** Designing downstream logic that assumes the item is globally available across all read-replicas immediately after the trigger fires (ignoring eventual consistency).
*   **Payload Overload:** Including massive binary blobs or unnecessary metadata in the trigger payload instead of providing a reference URI or ID.

## Edge Cases

*   **Bulk Creation:** When 1,000 items are created in a single transaction, the system must determine if it fires 1,000 individual triggers or a single "Bulk Created" event.
*   **Race Conditions:** A 012 Trigger fires, but a subsequent "Update" or "Delete" trigger for the same item arrives at the subscriber before the "Created" payload due to network jitter.
*   **Partial Commits:** In distributed systems, an item might be created in one shard but fail in another; the trigger must account for the "Source of Truth" status.
*   **Ghost Items:** Items created by system processes (like migrations or backups) that should not trigger standard business workflows.

## Related Topics
*   **013 When an Item is Updated Trigger:** The logical successor to the creation event.
*   **014 When an Item is Deleted Trigger:** The lifecycle termination event.
*   **Webhook Architecture:** The common transport mechanism for these triggers.
*   **Idempotency Keys:** Essential for handling duplicate trigger executions.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |