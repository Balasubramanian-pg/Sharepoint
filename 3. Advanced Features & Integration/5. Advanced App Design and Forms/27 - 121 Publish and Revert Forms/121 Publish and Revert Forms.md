# 121 Publish and Revert Forms

Canonical documentation for 121 Publish and Revert Forms. This document defines concepts, terminology, and standard usage.

## Purpose
The 121 Publish and Revert Forms framework exists to manage the lifecycle of digital data collection interfaces. It addresses the critical need for version control, deployment integrity, and risk mitigation in environments where form schemas and logic are subject to frequent iteration. By separating the "design-time" state from the "run-time" state, this topic ensures that end-users interact with validated, stable interfaces while allowing administrators to develop improvements without disrupting active data collection.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Lifecycle states of a form (Draft, Published, Archived).
* Versioning mechanics and snapshotting.
* Rollback (Revert) logic and data integrity.
* Propagation of changes from staging to production environments.
* Impact of schema changes on existing submission data.

**Out of scope:**
* Specific UI/UX design principles for form fields.
* Vendor-specific database migration scripts.
* Third-party integration authentication protocols.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Draft** | An ephemeral, mutable state of a form used for development and testing; not accessible to end-users. |
| **Published** | The immutable, active state of a form version that is accessible for data entry. |
| **Revert** | The process of restoring a previously published version of a form to the active state. |
| **Schema** | The underlying structural definition of the form, including fields, data types, and validation rules. |
| **Snapshot** | A point-in-time capture of a form’s configuration, logic, and metadata. |
| **Atomic Deployment** | A publishing method where the transition from one version to another occurs instantaneously to prevent partial state errors. |

## Core Concepts

### State Separation
The fundamental principle of 121 Publish and Revert is the strict separation between the **Working Copy** and the **Active Instance**. Changes made to a form must never affect the live user experience until an explicit "Publish" action is triggered.

### Immutability of Published Versions
Once a form is published, that specific version should be treated as immutable. Any subsequent changes must result in a new version number or a new draft. This ensures that data collected under "Version A" can always be mapped back to the exact schema that existed at the time of submission.

### The Reversion Trigger
Reversion is not merely "undoing" an edit; it is the promotion of a historical snapshot to the current active status. This is a governance-heavy action that requires consideration of how existing data captured by the "faulty" version will be handled.

## Standard Model

The standard model for 121 Publish and Revert follows a linear progression with a circular recovery path:

1.  **Initialization:** A new form is created in a **Draft** state.
2.  **Validation:** The draft undergoes schema and logic validation.
3.  **Publication:** The draft is snapshotted, assigned a version ID, and moved to the **Published** state. The previous Published version (if any) is moved to **Archived**.
4.  **Observation:** The live form collects data.
5.  **Reversion (Optional):** If a defect is found, the system identifies the last known stable **Archived** version and re-promotes it to **Published**, effectively demoting the defective version.

## Common Patterns

### The "Blue-Green" Form Swap
In high-traffic environments, the system maintains two identical environments. The new form version is published to the "Green" environment; once verified, traffic is routed away from the "Blue" (old) environment. This allows for near-instant reversion by simply re-routing traffic.

### Semantic Versioning
Forms often utilize a Major.Minor.Patch versioning system. 
*   **Major:** Breaking schema changes (e.g., deleting a field).
*   **Minor:** Non-breaking additions (e.g., adding an optional field).
*   **Patch:** Cosmetic changes (e.g., updating a label or tooltip).

### Forward-Only Migration
A pattern where reverting a form does not delete the "bad" version but instead creates a new version that is a functional clone of a previous stable version. This maintains a clean, linear audit trail.

## Anti-Patterns

*   **Live-Editing:** Modifying the schema of a form while it is actively collecting data without a versioning increment. This leads to "Schema Drift" and data corruption.
*   **Implicit Publishing:** Systems that automatically save and deploy changes to the live environment without an explicit administrative "Publish" action.
*   **Orphaned Data:** Reverting to an older form version without a strategy for the data collected during the period the "newer" version was live.
*   **Hard-Coding Versions:** Referencing specific form version IDs in external APIs or integrations, which breaks the reversion process.

## Edge Cases

### Mid-Submission Session Updates
If a user is filling out a form while a "Publish" or "Revert" action occurs, the system must decide whether to allow the user to finish on the old schema or force a refresh. The standard approach is to allow the current session to complete using the schema cached at the start of the session.

### Destructive Reversion
When reverting to a version that lacks fields present in the "current" version, the system must handle the "excess" data. Canonical practice dictates that data is never deleted from the database, even if the form interface no longer displays the fields.

### Dependency Breaking
Forms that rely on external data lookups (e.g., a list of current employees) may fail upon reversion if the external data structure has changed in a way that the older form version cannot interpret.

## Related Topics
*   **Form Schema Versioning:** The technical specification of how JSON/XML schemas evolve.
*   **Data Integrity and Normalization:** How form data is stored in relational vs. non-relational systems.
*   **Audit Logging:** The record-keeping of who published or reverted a form and why.
*   **Change Management:** The organizational process surrounding the technical act of publishing.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |