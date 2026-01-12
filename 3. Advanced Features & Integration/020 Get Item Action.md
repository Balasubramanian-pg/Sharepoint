# 020 Get Item Action

Canonical documentation for 020 Get Item Action. This document defines concepts, terminology, and standard usage.

## Purpose
The **020 Get Item Action** exists to provide a standardized mechanism for retrieving a single, specific resource from a data persistence layer or service provider. Its primary purpose is to resolve a unique identifier into a complete or projected representation of an entity’s current state. 

This action addresses the requirement for precision in data retrieval, ensuring that consuming systems can access the most granular level of information available for a known entity without the overhead or ambiguity associated with collection-based queries.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   **Atomic Retrieval:** The logic governing the fetch of a single record by a unique key.
*   **State Representation:** The conceptual return of the entity's attributes.
*   **Existence Validation:** The theoretical handling of presence versus absence.
*   **Read-Only Constraints:** The non-mutative nature of the operation.

**Out of scope:**
*   **Collection Queries:** Filtering, sorting, or searching for multiple items (see *030 List Items*).
*   **Data Transformation:** Specific logic for mapping data between formats (e.g., JSON to XML).
*   **Vendor Implementations:** Specific syntax for SQL, NoSQL, or RESTful API frameworks.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Identifier (ID)** | A unique value used to distinguish a specific item from all others within a domain. |
| **Resource** | The conceptual entity or object being retrieved. |
| **Projection** | A subset of the resource's attributes returned instead of the full state. |
| **Idempotency** | The property where multiple identical requests have the same effect and return the same result (assuming no external state change). |
| **Null Response** | The standardized representation of a non-existent resource for a valid identifier. |

## Core Concepts
### Uniqueness
The 020 Get Item Action relies entirely on the uniqueness of the identifier. If an identifier resolves to more than one resource, the operation is considered malformed or the underlying data integrity is compromised.

### Idempotency and Side Effects
The Get Item Action is inherently **idempotent** and **safe**. Executing the action must not result in a change to the resource's state. While logging or telemetry may occur, the functional state of the system remains unchanged by the retrieval.

### Determinism
Given a static state of the data source, the 020 Get Item Action must be deterministic. Providing the same identifier must return the same resource representation.

## Standard Model
The standard model for a 020 Get Item Action follows a Request-Process-Response lifecycle:

1.  **Input Specification:** The consumer provides a unique identifier and, optionally, a projection mask (defining which fields to return).
2.  **Resolution:** The system locates the resource within the persistence layer using the identifier.
3.  **Authorization Check:** The system verifies that the requester has the necessary permissions to view the specific resource.
4.  **State Extraction:** The system captures the current attributes of the resource.
5.  **Output Delivery:** The system returns the resource representation or a standardized "Not Found" signal.

## Common Patterns
### Lazy Loading
The 020 Get Item Action is frequently used in "Lazy Loading" patterns where a summary list is retrieved first, and the full details of a specific item are only fetched via the 020 action when explicitly requested by a user or process.

### Caching (Read-Through)
To optimize performance, the result of a 020 Get Item Action is often stored in a high-speed cache. Subsequent requests for the same identifier are served from the cache until the data is invalidated or expires.

### Projection/Selection
In scenarios where resources are "heavy" (contain large amounts of data), the 020 action may allow the consumer to specify a "field mask" to retrieve only the necessary attributes, reducing bandwidth and processing time.

## Anti-Patterns
*   **Get-for-Update Side Effects:** Modifying a "Last Accessed" timestamp as a hard requirement within the retrieval logic, which violates the "safe" nature of the action.
*   **Over-Fetching:** Returning massive binary blobs or unnecessary nested relationships by default when they are rarely used by consumers.
*   **Identifier Guessing:** Implementing 020 actions without proper authorization, allowing attackers to iterate through identifiers (Insecure Direct Object Reference).
*   **Soft-Delete Ambiguity:** Returning a "Success" status for an item that has been logically deleted but remains in the database, without indicating its deleted status.

## Edge Cases
*   **Race Conditions:** An item is deleted or modified in the millisecond between the start of the 020 action and the delivery of the response.
*   **Composite Keys:** Handling identifiers that consist of multiple fields (e.g., a Tenant ID and a User ID) and ensuring the 020 action treats them as a single atomic key.
*   **Transient States:** Attempting to "Get" an item that is currently in the middle of a multi-step creation or transition process.
*   **Identifier Collisions:** How the action behaves if the underlying system contains duplicate identifiers (typically results in a 500-series error or "Internal System Error").

## Related Topics
*   **010 Create Item Action:** The precursor for generating the resource and identifier.
*   **030 List Items Action:** The retrieval of multiple resources, often used to find the ID for a 020 action.
*   **040 Update Item Action:** The modification of the state retrieved by a 020 action.
*   **Resource Identity Standards:** Documentation on UUID, ULID, and URI formatting.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |