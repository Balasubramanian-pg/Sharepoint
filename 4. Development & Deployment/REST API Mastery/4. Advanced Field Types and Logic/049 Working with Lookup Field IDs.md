# [049 Working with Lookup Field IDs](4. Development & Deployment/REST API Mastery/4. Advanced Field Types and Logic/049 Working with Lookup Field IDs.md)

Canonical documentation for [049 Working with Lookup Field IDs](4. Development & Deployment/REST API Mastery/4. Advanced Field Types and Logic/049 Working with Lookup Field IDs.md). This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Lookup Field IDs is to establish and maintain relational integrity between disparate entities within a data ecosystem. This topic addresses the problem of data redundancy and inconsistency by providing a mechanism to reference a single "source of truth" record from multiple locations without duplicating the underlying data. It ensures that relationships remain stable even when the descriptive attributes of the referenced entity change.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The theoretical framework of relational referencing via identifiers.
* The distinction between internal identifiers and display values.
* Lifecycle management of references (creation, updates, and deletion).
* Referential integrity constraints.

**Out of scope:**
* Specific vendor implementations (e.g., Salesforce Lookups, SQL Foreign Keys, SharePoint Lookup columns).
* User interface design for lookup selectors.
* Performance tuning for specific database engines.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Lookup Field** | A data attribute in a source entity that stores a reference to a record in a target entity. |
| **Identifier (ID)** | A unique, immutable value used to distinguish a specific record within an entity. |
| **Source Entity** | The entity containing the lookup field (the "child" or "referencing" entity). |
| **Target Entity** | The entity being referenced by the lookup field (the "parent" or "referenced" entity). |
| **Referential Integrity** | A state where every lookup ID in a source entity corresponds to a valid, existing ID in the target entity. |
| **Surrogate Key** | A system-generated, non-meaningful ID used specifically for lookups (e.g., a GUID or UUID). |
| **Natural Key** | An ID derived from real-world data (e.g., an SSN or SKU) used as a lookup reference. |

## Core Concepts
### The Reference Abstraction
A lookup field ID acts as a pointer. It abstracts the relationship so that the source entity does not need to know the details of the target entity, only its unique identifier. This allows the target entity's attributes (e.g., Name, Address, Status) to change without breaking the connection.

### Cardinality
Lookup IDs define the nature of the relationship:
*   **One-to-Many:** The most common pattern where one target record is referenced by many source records.
*   **One-to-One:** A constraint where a lookup ID must be unique within the source entity.

### Resolution and Dereferencing
Resolution is the process of retrieving the full record or specific attributes of the target entity using the stored ID. Dereferencing occurs when the system follows the ID to provide the "Display Value" to the end-user or consuming application.

## Standard Model
The standard model for working with Lookup Field IDs follows a strict separation between the **Storage Layer** and the **Presentation Layer**:

1.  **Storage Layer:** Only the immutable ID is stored in the source entity. This ID should ideally be a surrogate key (GUID/UUID) to prevent breakage if business logic changes.
2.  **Logic Layer:** The system enforces referential integrity, preventing the deletion of a target record if active lookup IDs point to it (Restrict Delete) or automatically clearing/deleting the source (Cascade).
3.  **Presentation Layer:** The ID is masked by a "Display Value" (usually a Name or Title attribute from the target entity) to ensure human readability.

## Common Patterns
### The GUID/UUID Pattern
Using globally unique identifiers as the lookup ID. This is the preferred pattern for distributed systems to avoid ID collisions and ensure immutability.

### Polymorphic Lookups
A pattern where a single lookup field can store IDs from multiple different target entities. This requires an additional "Type" attribute to tell the system which entity the ID belongs to.

### Self-Referencing Lookups
A pattern where the source and target entity are the same. This is commonly used for hierarchical data, such as an "Employee" record referencing a "Manager" record within the same table.

## Anti-Patterns
*   **Hardcoding IDs:** Embedding specific ID values directly into application code or logic, which leads to failure when moving data between environments (e.g., Dev to Prod).
*   **Using Mutable Data as IDs:** Using a field like "Email Address" or "Company Name" as the lookup ID. If the name changes, the link is broken or requires an expensive "cascade update."
*   **Storing Display Values:** Storing the "Name" of the target record instead of its ID in the lookup field. This leads to data corruption if two records have the same name or if a record is renamed.
*   **Dangling References:** Allowing a lookup ID to persist after the target record has been deleted, leading to null pointer errors or orphaned data.

## Edge Cases
*   **Circular References:** Entity A references Entity B, which in turn references Entity A. This can create infinite loops during data serialization or deletion.
*   **Soft Deletes:** When a target record is marked as "deleted" but remains in the database. The lookup ID remains valid at the database level but must be treated as invalid at the application level.
*   **Late-Bound References:** Scenarios where a lookup ID is assigned before the target record is actually created (common in asynchronous message processing).
*   **Multi-Tenant ID Collisions:** In systems where IDs are sequential integers, moving records between tenants can result in different records sharing the same ID, necessitating a re-mapping strategy.

## Related Topics
*   **012 Data Normalization:** The process of organizing data to minimize redundancy, which necessitates the use of lookups.
*   **088 Identity and Access Management:** How IDs are secured and scoped.
*   **104 API Design Patterns:** How lookup IDs are represented in REST/GraphQL interfaces (e.g., HATEOAS).
*   **210 Database Schema Design:** The physical implementation of relational constraints.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |