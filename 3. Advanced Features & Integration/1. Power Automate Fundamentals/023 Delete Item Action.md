# 023 Delete Item Action

Canonical documentation for 023 Delete Item Action. This document defines concepts, terminology, and standard usage.

## Purpose
The **023 Delete Item Action** represents the terminal phase of a resource's lifecycle within a system. Its primary purpose is to remove a specific entity from an active state, ensuring data hygiene, resource optimization, and compliance with data retention or privacy requirements (such as the "Right to Erasure"). This action addresses the problem of data obsolescence and the necessity of maintaining referential integrity when a discrete unit of information is no longer required or valid.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   The logical process of resource removal.
*   State transitions associated with deletion.
*   Impact on referential integrity and relational hierarchies.
*   Standard behaviors for confirmation and idempotency.

**Out of scope:**
*   Specific UI/UX component designs (e.g., "Red Trash Can" icons).
*   Vendor-specific API syntax (e.g., SQL `DELETE` vs. NoSQL `remove`).
*   Physical hardware destruction protocols.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Item** | The discrete unit of data or resource targeted for removal. |
| **Hard Delete** | The permanent, irreversible removal of data from the storage medium. |
| **Soft Delete** | A logical deletion where the item is marked as "deleted" or "inactive" but remains in the database, typically hidden from standard queries. |
| **Cascading Delete** | A secondary action where the deletion of a parent item triggers the automatic deletion of all associated child items. |
| **Idempotency** | The property where performing the delete action multiple times on the same item results in the same system state as the initial action. |
| **Referential Integrity** | The consistency requirement that ensures relationships between items remain valid during and after a deletion. |

## Core Concepts

### Resource Lifecycle Termination
The Delete Item Action is the final state in the CRUD (Create, Read, Update, Delete) lifecycle. It signifies that the item is no longer relevant to the system's operational context.

### Irreversibility vs. Recoverability
A core concept of deletion is the tension between the finality of the action and the need for error recovery. Systems must define whether a deletion is a destructive "Hard Delete" or a recoverable "Soft Delete" based on the criticality of the data.

### Authorization and Permissions
Deletion is a high-privilege action. The system must verify that the actor (user or service) possesses the explicit authority to terminate the resource before the action is processed.

## Standard Model

The standard model for a Delete Item Action follows a structured sequence to ensure data safety:

1.  **Identification:** The system uniquely identifies the target item via a persistent identifier (UID/UUID).
2.  **Validation:** The system checks for dependencies. If the item is required by another active process, the action may be blocked.
3.  **Authorization:** The system verifies the actor's permissions.
4.  **Execution:**
    *   *Soft Delete:* Update the item status to "Deleted" and record the timestamp.
    *   *Hard Delete:* Remove the record from the primary storage index.
5.  **Propagation:** If configured, the system executes cascading deletes or notifies downstream services via webhooks or event buses.
6.  **Confirmation:** The system returns a success state, regardless of whether the item existed (to maintain idempotency).

## Common Patterns

### The Archive Pattern
Instead of immediate removal, the item is moved to a "cold" storage or an archive table. This satisfies data retention policies while removing the item from the "hot" operational path.

### The Two-Step Verification (Soft-to-Hard)
Items are first "Soft Deleted" (placed in a "Trash" or "Recycle Bin" state). After a predefined duration (e.g., 30 days), a background process executes a "Hard Delete."

### Tombstoning
In distributed systems, a "tombstone" record is created to synchronize the deletion across multiple nodes, preventing the item from being "re-animated" by older data syncs.

## Anti-Patterns

*   **Silent Failure:** Failing to delete an item due to a constraint but returning a success message.
*   **Dangling References:** Deleting a parent item while leaving orphaned child items that point to a non-existent ID, breaking system integrity.
*   **Non-Idempotent Deletion:** Returning an error (e.g., 404 Not Found) on a second delete request in a way that breaks automated retry logic.
*   **Lack of Audit Trail:** Removing data without logging who initiated the deletion and when, which is critical for security and compliance.

## Edge Cases

*   **Concurrent Deletion:** Two actors attempt to delete the same item simultaneously. The system must handle the race condition so that only one request performs the logic while the other is gracefully handled.
*   **Circular Dependencies:** Item A cannot be deleted because Item B depends on it, but Item B is also slated for deletion.
*   **Partial Failure in Cascades:** In a cascading delete, the parent is removed but a network failure prevents the children from being removed. This requires transactional integrity (Atomicity).
*   **Deletion of System-Critical Items:** Attempting to delete a "Root" user or a "Default" configuration that the system requires to function.

## Related Topics

*   **012 Resource Lifecycle Management:** The broader context of item birth, growth, and death.
*   **045 Data Retention Policy:** The legal and organizational rules governing how long data must be kept before deletion.
*   **088 Transactional Integrity:** The mechanisms (ACID) that ensure deletions are processed reliably.
*   **102 Soft Delete Implementation:** Specific strategies for logical deletion.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |