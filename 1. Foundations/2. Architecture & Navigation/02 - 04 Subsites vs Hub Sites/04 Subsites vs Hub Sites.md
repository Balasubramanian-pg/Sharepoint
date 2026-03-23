# Subsites vs Hub Sites

Canonical documentation for Subsites vs Hub Sites. This document defines concepts, terminology, and standard usage.

## Purpose

The distinction between subsites and hub sites is crucial in the context of information architecture, particularly within large, complex websites or intranets. This topic exists to clarify the differences, benefits, and appropriate uses of subsites and hub sites, addressing the problem space of organizing and structuring content in a way that is navigable, maintainable, and scalable. Effective differentiation and implementation of subsites and hub sites can significantly impact user experience, content management, and the overall efficiency of a website or intranet.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

Clarify what is in scope and out of scope for this topic.

**In scope:**
* Definitions and distinctions between subsites and hub sites
* Best practices for planning and implementing subsites and hub sites
* Discussion of the roles of subsites and hub sites in information architecture

**Out of scope:**
* Tool-specific implementations (e.g., SharePoint, Drupal)
* Vendor-specific behavior or proprietary solutions
* Detailed technical instructions for setting up subsites or hub sites in specific platforms

## Definitions

Provide precise definitions for key terms used throughout the documentation.

| Term | Definition |
|------|------------|
| Subsite | A sub-site is a subset of a larger website, typically focused on a specific topic, department, or audience, with its own distinct structure and possibly its own navigation. |
| Hub Site | A hub site is a central location that connects and aggregates content from multiple related subsites or other sources, providing a unified view and often serving as a gateway or portal to more detailed information. |
| Information Architecture | The practice of organizing the structure and content of a website or intranet to make it understandable and navigable for users. |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

Explain the fundamental ideas that make up the topic.

### Subsites
Subsites are essential for organizing content around specific themes, projects, or departments within a larger website or intranet. They allow for tailored content, navigation, and sometimes even unique branding, while still being part of the overarching structure.

### Hub Sites
Hub sites serve as central hubs that aggregate and connect related subsites, providing users with a single point of access to a collection of resources, news, or information on a particular topic. They facilitate discovery and navigation across multiple related areas.

## Standard Model

Describe the generally accepted or recommended model for this topic.

In the standard model, a website or intranet is structured with a clear hierarchy, starting from a main site or portal, which then branches out into hub sites. Each hub site connects to several subsites, which are focused on specific topics or audiences. This model promotes clarity, ease of navigation, and efficient content management.

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified, as they can lead to complexity and user confusion.

## Common Patterns

Document recurring patterns or approaches associated with this topic.

* **Departmental Subsites**: Creating subsites for each department within an organization to house department-specific content and resources.
* **Project Hub Sites**: Establishing hub sites for large projects that aggregate content, news, and resources from various subsites related to the project.

## Anti-Patterns

Describe common mistakes or discouraged practices.

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues.

* **Deep Nesting**: Creating subsites within subsites to an excessive depth, leading to navigation complexity and user frustration.
* **Isolated Subsites**: Failing to integrate subsites with the rest of the website or intranet, resulting in disconnected user experiences and content silos.

## Edge Cases

Explain unusual, ambiguous, or boundary scenarios related to the topic.

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions.

* **Subsites as Hub Sites**: Situations where a subsite also acts as a hub site for smaller, related subsites, blurring the lines between the two concepts.
* **External Subsites**: Subsites that are hosted externally or managed by third parties, requiring special considerations for integration, security, and branding consistency.

## Related Topics

Link to adjacent or dependent topics.

* **Information Architecture**: The broader discipline of organizing and structuring content in digital spaces.
* **Content Strategy**: Planning for the creation, publication, and governance of content across subsites and hub sites.

## References

List authoritative external references, specifications, or papers.

* **"Information Architecture for the World Wide Web" by Peter Morville and Louis Rosenfeld**: A seminal book on information architecture that discusses principles applicable to subsites and hub sites.
* **W3C Web Accessibility Initiative (WAI)**: Guidelines for making web content, including subsites and hub sites, accessible to people with disabilities.

## Change Log

Document notable changes to this topic over time.

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-02-01 | Added section on edge cases and updated references |
| 1.2 | 2026-03-15 | Clarified definitions and expanded on common patterns |