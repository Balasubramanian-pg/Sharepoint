# 050 Multi Lookup Field Implementation

Canonical documentation for 050 Multi Lookup Field Implementation. This document defines concepts, terminology, and standard usage.

## Purpose
The 050 Multi Lookup Field Implementation exists to facilitate complex relational data modeling where a single attribute of an entity must reference multiple distinct records from another entity. This pattern addresses the limitation of standard single-value lookup fields, which only support 1:1 or N:1 relationships. 

By standardizing the multi-lookup approach, systems can maintain data normalization while providing a streamlined user experience for associating entities without requiring the manual creation of intermediary junction records by the end-user.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logical data structures for many-to-many (N:N) relationships.
* Referential integrity requirements for multi-value associations.
* Abstract UI/UX patterns for selection and display.
* Validation logic for collection-based attributes.

**Out of scope:**
* Specific vendor implementations (e.g., Salesforce Multi-Select Picklists, Microsoft Dataverse Activity Parties).
* Front-end framework-specific components (e.g., React-Select, Angular Material Chips).
* Physical database optimization (e.g., indexing strategies for specific SQL engines).

## Definitions
| Term | Definition |
|------|------------|
| **Source Entity** | The primary record containing the multi-lookup field. |
| **Target Entity** | The entity type being referenced by the multi-lookup field. |
| **Junction Table** | An underlying relational table that maps IDs from the Source Entity to IDs from the Target Entity. |
| **Cardinality** | The numerical relationship between entities; in this context, specifically Many-to-Many (N:N). |
| **Referential Integrity** | The property ensuring that every reference in a multi-lookup field points to a valid, existing record in the target entity. |
| **Selection Set** | The collection of records currently associated with the source record via the multi-lookup field. |

## Core Concepts
### 1. Relational Abstraction
The 050 Multi Lookup Field acts as an abstraction layer over a Many-to-Many relationship. While the database may store these associations in a separate junction table to maintain normalization, the implementation presents these associations as a single "field" or "attribute" on the source record.

### 2. Bidirectional vs. Unidirectional Visibility
While the data relationship is inherently bidirectional, the 050 implementation focuses on the "Source-to-Target" perspective. A standard implementation must define whether the Target Entity is aware of its association with the Source Entity (bidirectional) or if the association is only visible from the Source (unidirectional).

### 3. Collection Constraints
Unlike single lookups, multi-lookups require definitions for:
* **Minimum Selection:** The minimum number of records required (often 0 or 1).
* **Maximum Selection:** The upper limit of records allowed to prevent performance degradation or logic errors.
* **Uniqueness:** Ensuring the same target record cannot be selected multiple times within a single field instance.

## Standard Model
The standard model for a 050 Multi Lookup Field Implementation follows a three-tier architecture:

1.  **The Interface Layer:** A UI component that allows users to search, select, and remove multiple entities. It displays selected items as "tags," "chips," or a "sub-grid."
2.  **The Logic Layer:** A controller that validates the selection set against business rules (e.g., "User cannot select more than 5 reviewers").
3.  **The Persistence Layer:** A normalized structure, typically a junction table, consisting of:
    *   `Source_Record_ID`
    *   `Target_Record_ID`
    *   `Metadata` (Optional: e.g., Sort Order, Date Added)

## Common Patterns
### The "Tagging" Pattern
Used for categorizing records where the target entities are descriptors (e.g., assigning multiple "Skills" to an "Employee" record).

### The "Participant" Pattern
Used for assigning multiple actors to a process (e.g., assigning multiple "Stakeholders" to a "Project").

### The "Filtered Lookup" Pattern
A multi-lookup field where the available target records are dynamically filtered based on another value in the source record (e.g., selecting "Products" filtered by the "Manufacturer" already selected on the form).

## Anti-Patterns
*   **Comma-Separated Strings:** Storing multiple IDs in a single string field (e.g., "ID1, ID2, ID3"). This violates First Normal Form (1NF), prevents efficient querying, and breaks referential integrity.
*   **Hardcoded Column Limits:** Creating a fixed number of lookup columns (e.g., `Lookup_1`, `Lookup_2`, `Lookup_3`) on the source table. This limits scalability and complicates reporting.
*   **Overloading Single Lookups:** Using a single lookup field to point to a "Group" record as a proxy for multiple individuals, unless the group itself is a meaningful entity in the business logic.

## Edge Cases
*   **Target Record Deletion:** What happens to the multi-lookup reference when a selected target record is deleted? The implementation must define a "Cascade Link Removal" or "Restrict Delete" policy.
*   **Circular References:** A scenario where Entity A references Entity B in a multi-lookup, and Entity B references Entity A, potentially creating infinite loops in recursive logic or tree-view visualizations.
*   **Large Selection Sets:** When a multi-lookup contains thousands of references, standard UI components may fail. Implementations must define a threshold where the UI switches from a "Tag" view to a "Paginated Grid" view.
*   **Cross-Tenant/Cross-Database References:** Handling multi-lookups where the target records reside in a different data partition or external system.

## Related Topics
*   **010 Data Normalization Standards:** The foundational principles for relational data.
*   **042 Referential Integrity Protocols:** Standards for maintaining links between entities.
*   **075 Search and Filter Architecture:** How target entities are queried for selection within the lookup interface.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |