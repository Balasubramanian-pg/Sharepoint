# 048 Setting Multi User Person Fields

Canonical documentation for 048 Setting Multi User Person Fields. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Multi-User Person Fields is to facilitate the association of multiple human entities—or their digital representations—with a single record or data object. This topic addresses the complexities of managing non-scalar identity data, ensuring that systems can accurately represent shared responsibility, collaborative ownership, and group-based notification triggers without compromising data integrity or identity resolution.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Data structures for storing multiple identity references.
* Logic for adding, removing, and updating user sets.
* Validation requirements for identity integrity.
* Theoretical boundaries of multi-user cardinality.

**Out of scope:**
* Specific UI/UX implementations of "People Pickers."
* Vendor-specific API syntax (e.g., Microsoft Graph, Salesforce Apex).
* Authentication protocols (OAuth2, SAML).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Identity Principal** | A unique entity (user or service) recognized by the system that can be assigned to a field. |
| **Multi-User Field** | A data attribute capable of storing a collection of references to Identity Principals. |
| **Identity Resolution** | The process of verifying that a provided identifier corresponds to a valid, active account in the Identity Provider (IdP). |
| **Cardinality** | The numerical constraints placed on the number of users that can be assigned to a single field. |
| **Junction Representation** | A normalized data model where associations are stored in a separate relational structure rather than an inline array. |

## Core Concepts
### Identity Reference vs. Identity Metadata
A Multi-User Person Field should ideally store a **Reference** (such as a GUID or Unique ID) rather than **Metadata** (such as a Display Name or Email). Storing metadata directly leads to data staleness when a user’s attributes change in the primary Identity Provider.

### Atomic vs. Differential Updates
When setting multi-user fields, systems typically employ one of two logic patterns:
1.  **Atomic (Full Replace):** The entire collection is overwritten with a new set of identifiers.
2.  **Differential (Delta):** Only the specific additions or removals are processed, preserving the existing state of unmentioned users.

### Validation and Constraints
Setting these fields requires validation against the current state of the directory. This includes checking for account existence, account status (active/disabled), and whether the principal type (User vs. Group) is permitted for that specific field.

## Standard Model
The standard model for Multi-User Person Fields follows a **Collection-Based Reference Model**.

1.  **Data Type:** The field is defined as a collection (Array, List, or Set) of Unique Identifiers.
2.  **Uniqueness:** The collection must enforce uniqueness; a single Identity Principal cannot be represented multiple times within the same field instance.
3.  **Resolution:** Upon "Setting" the field, the system must perform a look-up to resolve the provided identifiers against the authoritative Identity Source.
4.  **Persistence:**
    *   *Relational:* Use a join table (Many-to-Many) to link the Object ID to the User ID.
    *   *Document-based:* Use an array of IDs within the document, ideally indexed for searchability.

## Common Patterns
### The "Observer" Pattern
Used when multiple users need to be notified of changes but do not have primary ownership. The field acts as a distribution list for system-generated events.

### The "Shared Responsibility" Pattern
Used for approval workflows where any one of the assigned users (or all of them) must perform an action. The field defines the pool of authorized actors.

### The "Append-Only" Log
A pattern where users can be added to a field over time (e.g., "Contributors"), but historical entries are preserved for audit purposes, often coupled with a timestamp of when they were added to the field.

## Anti-Patterns
*   **String-Based Storage:** Storing users as a comma-separated string of names (e.g., "John Doe; Jane Smith"). This breaks identity resolution and makes renaming impossible.
*   **Unbounded Cardinality:** Allowing an infinite number of users in a single field, which can lead to payload bloat and performance degradation during serialization.
*   **Implicit Permissions:** Assuming that being added to a Multi-User Field automatically grants system-level security permissions without a formal underlying Access Control List (ACL) update.
*   **Hard-Coding IDs:** Hard-coding specific user identifiers in application logic rather than referencing them dynamically.

## Edge Cases
*   **Deactivated Users:** How the system handles a field containing a user who has been deleted or disabled in the IdP. Standard practice suggests retaining the ID for historical integrity but flagging the record as "Inactive."
*   **Cross-Tenant Identities:** In multi-tenant environments, setting a user from an external directory requires a "Guest" or "Foreign Key" reference that may not follow standard internal ID formats.
*   **Group Expansion:** If a "Group" is added to a Multi-User Person Field, the system must determine if it stores the Group ID or expands the group to store individual User IDs at the moment of assignment.
*   **Empty Sets:** Distinguishing between a "Null" state (field never set) and an "Empty Array" (field cleared by a user).

## Related Topics
*   **012 Identity Resolution Services:** How IDs are mapped to human-readable data.
*   **089 Access Control Lists (ACL):** How multi-user fields interact with security trimming.
*   **104 Audit Logging:** Tracking changes to multi-user assignments over time.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |