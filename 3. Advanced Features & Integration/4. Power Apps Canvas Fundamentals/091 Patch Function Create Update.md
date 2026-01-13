# 091 Patch Function Create Update

Canonical documentation for 091 Patch Function Create Update. This document defines concepts, terminology, and standard usage.

## Purpose
The 091 Patch Function Create Update (commonly referred to as an "Upsert" or "Merge" operation) exists to provide a unified mechanism for synchronizing state between a source and a destination. It addresses the complexity of conditional data persistence by abstracting the decision-making process—determining whether a record should be initialized as a new entry or modified as an existing one—into a single, atomic-style operation. This reduces the need for redundant "check-then-act" logic in client applications and ensures data integrity during high-frequency updates.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Identity Resolution:** The logic used to distinguish between new and existing records.
* **Delta Processing:** The handling of partial data sets versus full record replacements.
* **State Transition:** The theoretical movement from a null or existing state to a target state.
* **Idempotency:** The principles of ensuring repeated operations yield the same result.

**Out of scope:**
* Specific vendor implementations (e.g., Power Platform `Patch()`, SQL `MERGE`, or HTTP `PATCH` methods).
* Physical storage layer optimization or indexing strategies.
* Specific programming language syntax.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Patch** | A set of changes applied to a data object, typically containing only the fields to be modified. |
| **Identity Key** | A unique identifier (UUID, Primary Key, or Natural Key) used to determine if a record already exists in the destination. |
| **Upsert** | A portmanteau of "Update" and "Insert"; the functional outcome of the 091 pattern. |
| **Delta** | The specific difference between the current state and the desired state. |
| **Hydration** | The process of filling a partial patch object with existing data from the destination to form a complete record. |
| **Idempotency** | The property of an operation where multiple identical requests have the same effect as a single request. |

## Core Concepts

### 1. Identity Resolution
The foundation of the Create/Update function is the ability to resolve identity. The function must evaluate the provided data for a "Match Criterion." 
* If the criterion is met (e.g., a matching ID is found), the operation proceeds as an **Update**.
* If the criterion is not met (e.g., ID is null or not found), the operation proceeds as a **Create**.

### 2. Partial vs. Full Updates
The 091 pattern distinguishes between "Full State Replacement" (where missing fields are cleared) and "Partial Patching" (where missing fields in the request are ignored, preserving existing values in the destination).

### 3. Atomicity
In a standard model, the transition from "Check" to "Act" should be atomic. This prevents race conditions where two simultaneous "Create" operations occur because neither found an existing record at the start of the execution.

## Standard Model
The standard model for a 091 Patch Function follows a specific logical flow:

1.  **Input Validation:** Ensure the patch object conforms to the required schema.
2.  **Existence Check:** Query the destination using the Identity Key.
3.  **Branching Logic:**
    *   **Branch A (Exists):** Retrieve the existing record, merge the Delta (patch) into the existing fields, and apply validation rules for modifications.
    *   **Branch B (Not Found):** Initialize a new record, apply the Delta, and fill default values for required fields not present in the patch.
4.  **Persistence:** Commit the resulting object to the destination.
5.  **Return:** Return the finalized record (including system-generated fields like timestamps or auto-incrementing IDs).

## Common Patterns

### The "Delta-Only" Pattern
The caller sends only the fields that have changed. The function is responsible for merging these changes into the existing record. This minimizes bandwidth and reduces the risk of overwriting concurrent changes to unrelated fields.

### The "Defaulting" Pattern
Used primarily during the "Create" branch. The function maintains a template of default values. If the patch is missing non-nullable fields, the function populates them automatically to ensure the record is valid upon insertion.

### Optimistic Concurrency Pattern
The patch includes a version token or timestamp. If the destination record has a different token than the one provided in the patch, the update fails. This prevents "lost updates" in multi-user environments.

## Anti-Patterns

*   **Blind Upserts:** Creating records when an ID is provided but not found, without verifying if the ID should have been system-generated. This can lead to "Ghost Records" with invalid or orphaned identifiers.
*   **Null Ambiguity:** Treating a `null` value in a patch as "do not update" in some fields, but "set to null" in others. This creates unpredictable data states.
*   **Over-Patching:** Sending the entire record back to the server for every minor change, which increases the risk of overwriting changes made by other processes in the interim.
*   **Side-Effect Overload:** Embedding heavy business logic (like sending emails or triggering external APIs) directly inside the Create/Update function, making the core persistence layer slow and brittle.

## Edge Cases

*   **Collision on Natural Keys:** When a record is being "Created" but a unique constraint (other than the Primary Key) is violated (e.g., duplicate email address).
*   **Partial Failures:** In a bulk 091 operation, handling scenarios where 50 records are updated successfully but 2 fail validation.
*   **Soft Deletes:** How the function behaves when it attempts to "Update" a record that exists but is marked as "Deleted" or "Inactive."
*   **Identity Mutation:** Attempts to "Update" the Identity Key itself, which usually requires a delete-and-recreate flow rather than a standard patch.

## Related Topics
*   **042 Data Validation Framework:** Standards for validating data before persistence.
*   **115 Concurrency Control:** Detailed strategies for handling simultaneous data access.
*   **078 Identity Management:** Standards for UUID and Primary Key generation.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |