# 022 Update Item Action

Canonical documentation for 022 Update Item Action. This document defines concepts, terminology, and standard usage.

## Purpose
The **022 Update Item Action** exists to facilitate the modification of existing data entities within a system of record. Its primary purpose is to synchronize the digital representation of an entity with its evolving real-world state or intended configuration. This action addresses the problem of data obsolescence by providing a structured mechanism to alter specific attributes of an item without necessitating the destruction and recreation of the entity’s unique identity.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **State Transition Logic:** The theoretical movement from a current state to a target state.
* **Data Integrity Constraints:** Rules governing the validity of an update.
* **Concurrency Control:** Managing simultaneous update requests.
* **Partial vs. Full Updates:** The logic of attribute replacement.

**Out of scope:**
* **Specific vendor implementations:** (e.g., SQL `UPDATE` syntax, Salesforce Apex triggers, or RESTful `PATCH` methods).
* **UI/UX Design:** How the update action is presented to an end-user.
* **Hardware-level persistence:** The physical storage of data on disk.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Item** | A discrete, uniquely identifiable data entity within a system. |
| **Attribute** | A specific property or field belonging to an Item. |
| **Delta** | The set of changes representing the difference between the current state and the proposed state. |
| **Idempotency** | The property where an action can be performed multiple times with the same result as a single execution. |
| **Optimistic Locking** | A strategy where an update is only processed if the item has not been modified since it was last read. |
| **Pessimistic Locking** | A strategy where an item is "locked" to prevent other updates until the current transaction is complete. |
| **Atomic Update** | An update operation that either completes entirely or fails entirely, leaving no partial changes. |

## Core Concepts

### 1. Identity Preservation
The fundamental requirement of the 022 Update Item Action is the preservation of the Item’s unique identifier. While attributes change, the identity remains constant to maintain relational integrity across the system.

### 2. State Validation
Before an update is committed, the proposed state must be validated against:
*   **Schema Constraints:** Data types, lengths, and formats.
*   **Business Logic:** Rules that govern whether a transition is permissible (e.g., an order cannot be updated to "Shipped" if it is currently "Cancelled").

### 3. Change Detection
Systems must determine what has changed. This involves comparing the incoming payload with the existing record to identify the "Delta." This minimizes unnecessary processing and audit log noise.

## Standard Model

The standard model for the 022 Update Item Action follows a five-stage pipeline:

1.  **Request Ingestion:** The system receives the identifier and the proposed changes.
2.  **Pre-flight Verification:** The system checks for the existence of the Item and verifies the requester's permissions.
3.  **Conflict Resolution:** The system applies concurrency controls (e.g., checking version numbers) to ensure the update is based on the most recent data.
4.  **Transformation and Persistence:** The system merges the Delta with the existing Item and commits the new state to the data store.
5.  **Post-condition Execution:** The system triggers downstream effects, such as cache invalidation, notifications, or audit logging.

## Common Patterns

### Partial Update (Patch)
Only the attributes provided in the request are modified. All other attributes remain in their current state. This reduces bandwidth and prevents accidental overwriting of unrelated fields.

### Full Replacement (Put)
The entire Item is replaced by the provided payload. Attributes not included in the request are typically nullified or set to defaults.

### Upsert (Update or Insert)
A hybrid pattern where the system attempts to update an Item if it exists, or creates a new Item if the identifier is not found.

### Versioned Update
Each update creates a new version of the Item, allowing for historical tracking and the ability to "roll back" to a previous state.

## Anti-Patterns

*   **Over-posting:** Accepting and persisting attributes that the requester should not have the authority to change (e.g., an end-user updating their own "Account Balance").
*   **Blind Updates:** Overwriting data without checking for concurrent changes, leading to the "Lost Update" problem.
*   **Side-Effect Overload:** Attaching too many synchronous downstream processes to a single update action, causing performance degradation and potential timeouts.
*   **Identity Modification:** Attempting to change the primary unique identifier of an Item during an update action.

## Edge Cases

*   **Null vs. Omitted:** Distinguishing between a request to set an attribute to `null` (empty) versus a request that simply omits the attribute to keep its current value.
*   **Circular Dependencies:** An update to Item A triggers an update to Item B, which in turn attempts to update Item A, potentially creating an infinite loop.
*   **Large Attribute Sets:** Updating an Item with thousands of attributes where the Delta is minimal, requiring efficient diffing algorithms.
*   **Non-Existent Items:** Handling requests to update an Item that has been deleted or never existed (typically resulting in a "Not Found" error).

## Related Topics

*   **021 Create Item Action:** The precursor to the update action.
*   **023 Delete Item Action:** The terminal action in an Item's lifecycle.
*   **Audit Logging Standards:** The methodology for recording the history of updates.
*   **Concurrency Control Models:** Deep dive into locking mechanisms.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |