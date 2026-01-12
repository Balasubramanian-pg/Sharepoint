# Document Properties and Metadata

Canonical documentation for Document Properties and Metadata. This document defines concepts, terminology, and standard usage.

## Purpose

Document Properties and Metadata is a crucial aspect of information management, as it enables the effective organization, retrieval, and maintenance of documents within an organization. The primary problem space addressed by this topic is the need for a standardized approach to managing document properties and metadata, ensuring that documents are accurately described, easily searchable, and properly preserved. This standardization facilitates collaboration, reduces errors, and improves the overall efficiency of document-related workflows.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

Clarify what is in scope and out of scope for this topic.

**In scope:**
* Document metadata standards (e.g., Dublin Core, XMP)
* Document property management (e.g., author, creation date, file format)
* Metadata schema design and implementation

**Out of scope:**
* Tool-specific implementations (e.g., Microsoft Office, Adobe Creative Cloud)
* Vendor-specific behavior (e.g., proprietary metadata formats)
* Document content management systems (although they may utilize document properties and metadata)

## Definitions

Provide precise definitions for key terms used throughout the documentation.

| Term | Definition |
|------|------------|
| Document | A self-contained piece of information, such as a text file, image, or video. |
| Metadata | Information that describes or provides context about a document, such as author, creation date, or file format. |
| Document Properties | Attributes or characteristics of a document, such as file size, modification date, or permissions. |
| Schema | A predefined structure or organization for metadata, defining the relationships between different metadata elements. |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

Explain the fundamental ideas that make up the topic.

### Document Metadata
Document metadata is information that describes or provides context about a document. It can include attributes such as author, creation date, file format, and keywords. Metadata can be embedded within the document itself or stored separately in a database or repository.

### Document Properties
Document properties refer to the attributes or characteristics of a document, such as file size, modification date, or permissions. These properties are often used to manage and organize documents within a system or repository.

## Standard Model

Describe the generally accepted or recommended model for this topic.

The standard model for document properties and metadata involves the use of a metadata schema to define the structure and organization of metadata elements. This schema should be based on widely accepted standards, such as Dublin Core or XMP, to ensure interoperability and consistency across different systems and applications.

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified.

## Common Patterns

Document recurring patterns or approaches associated with this topic.

* Using a centralized metadata repository to store and manage document metadata
* Embedding metadata within documents using standards-based formats (e.g., XMP, EXIF)
* Utilizing automated tools and workflows to extract and populate metadata

## Anti-Patterns

Describe common mistakes or discouraged practices.

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues.

* Using proprietary or non-standard metadata formats, which can limit interoperability and reuse
* Failing to establish a consistent metadata schema or standard, leading to data inconsistencies and errors
* Over-reliance on manual metadata entry, which can be time-consuming and prone to errors

## Edge Cases

Explain unusual, ambiguous, or boundary scenarios related to the topic.

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions.

* Handling documents with multiple or conflicting metadata sources (e.g., embedded metadata vs. repository metadata)
* Managing documents with dynamic or changing metadata (e.g., updating author information or permissions)
* Dealing with documents that lack metadata or have incomplete metadata (e.g., scanned documents or legacy files)

## Related Topics

Link to adjacent or dependent topics.

* Information Architecture
* Content Management
* Data Governance

## References

List authoritative external references, specifications, or papers.

* Dublin Core Metadata Initiative (DCMI)
* Extensible Metadata Platform (XMP) specification
* ISO 15836:2009 - Information and documentation -- The Dublin Core metadata element set

## Change Log

Document notable changes to this topic over time.

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-02-01 | Added section on edge cases and updated references |
| 1.2 | 2026-03-15 | Revised standard model section to include more details on metadata schema design |