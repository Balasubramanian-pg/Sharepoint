# Sites and Site Collections

Canonical documentation for Sites and Site Collections. This document defines concepts, terminology, and standard usage.

## Purpose

The purpose of this documentation is to provide a comprehensive understanding of Sites and Site Collections, addressing the problem space of content management, collaboration, and information architecture. It aims to establish a common language and framework for designing, implementing, and managing Sites and Site Collections, ensuring consistency and interoperability across different platforms and systems. This documentation exists to guide developers, administrators, and users in creating and maintaining effective Sites and Site Collections, promoting best practices and minimizing errors.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

The scope of this topic includes the concepts, terminology, and standard usage related to Sites and Site Collections. The following aspects are in scope:

**In scope:**
* Site structure and hierarchy
* Site collection management
* Content types and metadata
* Security and permissions
* Site templates and customization

**Out of scope:**
* Tool-specific implementations (e.g., SharePoint, Drupal, WordPress)
* Vendor-specific behavior and proprietary features
* Low-level technical details (e.g., database schema, API implementation)

## Definitions

The following terms are defined for use throughout this documentation:

| Term | Definition |
|------|------------|
| Site | A self-contained collection of related content, including pages, documents, and other digital assets. |
| Site Collection | A hierarchical grouping of Sites, often sharing common characteristics, such as security, branding, or functionality. |
| Content Type | A predefined template or structure for creating and managing specific types of content, such as documents, images, or news articles. |
| Metadata | Descriptive information about content, such as author, date created, or keywords. |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

The fundamental ideas that make up the topic of Sites and Site Collections include:

### Site Hierarchy
A Site hierarchy refers to the organizational structure of Sites within a Site Collection, including parent-child relationships and navigation.

### Content Management
Content management involves the creation, editing, publishing, and maintenance of content within a Site or Site Collection, including versioning, approval workflows, and search.

### Security and Permissions
Security and permissions refer to the access control mechanisms that govern who can view, edit, or manage content within a Site or Site Collection, including authentication, authorization, and role-based access control.

## Standard Model

The standard model for Sites and Site Collections involves a hierarchical structure, with Site Collections containing multiple Sites, each with its own content, security, and settings. The standard model includes:

* A clear separation of concerns between Site Collections and Sites
* Consistent naming conventions and taxonomy
* Standardized content types and metadata
* Role-based access control and security

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified.

## Common Patterns

Recurring patterns or approaches associated with Sites and Site Collections include:

* Using Site templates to create new Sites with pre-defined structure and content
* Implementing content approval workflows to ensure quality and consistency
* Utilizing metadata and taxonomy to facilitate search and discovery
* Creating custom content types to support specific business needs

## Anti-Patterns

Common mistakes or discouraged practices related to Sites and Site Collections include:

* Creating overly complex or deep Site hierarchies
* Using inconsistent or unclear naming conventions
* Failing to implement proper security and permissions
* Ignoring content versioning and change management

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues.

## Edge Cases

Unusual, ambiguous, or boundary scenarios related to Sites and Site Collections include:

* Handling multiple Site Collections with overlapping or conflicting security settings
* Managing content that spans multiple Sites or Site Collections
* Dealing with legacy or migrated content that does not conform to standard models or patterns

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions.

## Related Topics

Adjacent or dependent topics related to Sites and Site Collections include:

* Information Architecture
* Content Strategy
* Search and Discovery
* Collaboration and Workflow

## References

Authoritative external references, specifications, or papers related to Sites and Site Collections include:

* [ISO 15489:2016](https://www.iso.org/standard/62542.html) - Information and documentation -- Records management
* [W3C Web Content Accessibility Guidelines (WCAG 2.1)](https://www.w3.org/TR/WCAG21/) - Web content accessibility guidelines

## Change Log

Notable changes to this topic over time include:

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-02-15 | Added section on anti-patterns and edge cases |
| 1.2 | 2026-03-20 | Updated definitions and standard model to reflect industry best practices |