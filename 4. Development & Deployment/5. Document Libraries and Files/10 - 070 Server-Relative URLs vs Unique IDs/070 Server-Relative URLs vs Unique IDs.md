# 070 Server Relative URLs vs Unique IDs

Canonical documentation for 070 Server Relative URLs vs Unique IDs. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of this topic is to define the two primary methodologies for addressing and retrieving resources within a networked environment: path-based addressing (Server Relative URLs) and identity-based addressing (Unique IDs). 

In digital systems, resources must be referenced in a way that balances human readability, system performance, and long-term persistence. This documentation addresses the fundamental tension between referencing a resource by its location within a hierarchy versus referencing it by an immutable identifier.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Theoretical foundations of location-based vs. identity-based referencing.
* Persistence and durability characteristics of both methods.
* Impact on system architecture, caching, and resource migration.
* Logical mapping between identifiers and physical locations.

**Out of scope:**
* Specific vendor implementations (e.g., SharePoint DocID, AWS S3 Object Keys, WordPress Permalinks).
* Syntax-specific rules for URL encoding or UUID versioning.
* Physical storage layer protocols (e.g., block storage vs. file storage).

## Definitions
| Term | Definition |
|------|------------|
| **Server Relative URL** | A path-based reference that identifies a resource relative to the root of the host environment (e.g., `/folder/subfolder/item.ext`). |
| **Unique ID (UID)** | An immutable, non-hierarchical identifier assigned to a specific resource that remains constant regardless of the resource's location (e.g., a GUID or Hash). |
| **Persistence** | The quality of a reference remaining valid over time, even if the underlying resource is moved or renamed. |
| **Resolution** | The process of translating a reference (URL or ID) into a physical resource stream. |
| **Namespace** | A container or context in which a URL or ID is guaranteed to be unique. |
| **Fragility** | The susceptibility of a reference to break due to changes in the environment's structure. |

## Core Concepts

### Location vs. Identity
The fundamental distinction lies in **what** is being referenced. 
*   **Server Relative URLs** reference a **location**. If the resource moves, the reference breaks unless a redirect is implemented.
*   **Unique IDs** reference an **identity**. The resource can exist anywhere within the system; as long as the ID is known, the system can locate it.

### The Resolution Layer
Unique IDs require a resolution layer (a database or lookup table) to map the ID to a physical path. Server Relative URLs are often "self-resolving" within the context of a file system or web server, as the path itself provides the instructions for retrieval.

### Human Readability vs. Machine Efficiency
URLs are typically human-readable and provide semantic context (e.g., `/reports/2023/financials.pdf`). Unique IDs are optimized for machine processing and database indexing, offering no inherent context to a human observer.

## Standard Model

The standard model for resource addressing suggests that the choice between URLs and IDs should be governed by the **Lifecycle of the Resource**:

1.  **Hierarchical Navigation:** Use Server Relative URLs when the primary interaction is through a directory structure or when SEO and human-readability are prioritized.
2.  **Relational Integrity:** Use Unique IDs when resources are linked across different systems or when the resource is likely to be moved, renamed, or reorganized.
3.  **The Hybrid Approach:** In modern systems, the standard model often employs a "Permalink" strategy where a Unique ID is used as the primary key in the backend, while a Server Relative URL (often containing a "slug") is presented to the user.

## Common Patterns

### The "ID-in-Path" Pattern
A hybrid approach where the Unique ID is embedded within a URL (e.g., `/articles/550e8400/how-to-code`). This provides the persistence of an ID with the SEO benefits of a path.

### Content Addressable Storage (CAS)
A pattern where the Unique ID is derived from the content itself (usually via a cryptographic hash). In this model, the ID and the content are inextricably linked; if the content changes, the ID changes.

### Redirection Mapping
Using a central registry to map legacy Server Relative URLs to new locations or Unique IDs to ensure backward compatibility during system migrations.

## Anti-Patterns

### Hardcoded Relative Paths in Distributed Systems
Relying on `/assets/image.png` in a system where assets may be distributed across multiple nodes or CDNs. This leads to "broken link" cascades when the directory structure is refactored.

### Using IDs for Public-Facing Navigation
Forcing users to navigate via strings like `?id=12345` without providing a human-readable alias. This degrades user experience and reduces the discoverability of content.

### Changing IDs on Resource Update
Treating a Unique ID as a version-specific identifier rather than a resource-specific identifier. If an ID changes every time a file is edited, it ceases to function as a persistent reference.

## Edge Cases

### Collision in Merged Namespaces
When two systems are merged, "Unique" IDs may collide if they were not generated using a sufficiently random or globally unique standard (e.g., using integer increments instead of UUIDs).

### Circular References
In systems using path-based URLs, moving a parent folder into a child folder (where possible) or creating symbolic link loops can break resolution logic.

### Orphaned IDs
When a resource is deleted but the Unique ID remains in the resolution table or is still referenced by external systems, leading to "ghost" references that consume metadata overhead.

## Related Topics
*   **012 URI/URL/URN Standards:** The technical syntax of resource identifiers.
*   **045 RESTful API Design:** How identifiers are exposed via web services.
*   **088 Persistence and Durability:** The broader concept of data longevity.
*   **102 Global Unique Identifiers (GUIDs):** The mathematics and generation of unique IDs.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |