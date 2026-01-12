# Modern vs Classic SharePoint

Canonical documentation for Modern vs Classic SharePoint. This document defines concepts, terminology, and standard usage.

## Purpose

The purpose of this documentation is to provide a clear understanding of the differences between Modern and Classic SharePoint, addressing the problem space of choosing the appropriate SharePoint experience for various use cases. This documentation aims to guide users, developers, and administrators in understanding the capabilities, limitations, and best practices for each experience. The goal is to facilitate informed decision-making and effective implementation of SharePoint solutions.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

**In scope:**
* Overview of Modern and Classic SharePoint experiences
* Key features and differences between the two experiences
* Best practices for choosing between Modern and Classic SharePoint

**Out of scope:**
* Tool-specific implementations, such as third-party add-ins or custom solutions
* Vendor-specific behavior, such as Microsoft-specific features or limitations
* Detailed technical instructions for configuring or customizing SharePoint

## Definitions

| Term | Definition |
|------|------------|
| Modern SharePoint | A modern, responsive, and mobile-friendly experience in SharePoint, characterized by a simplified and intuitive user interface |
| Classic SharePoint | A traditional, legacy experience in SharePoint, characterized by a more complex and customizable user interface |
| SharePoint Experience | The overall user interface and interaction model for SharePoint, encompassing both Modern and Classic experiences |
| Site Template | A pre-defined template used to create a new SharePoint site, which can be either Modern or Classic |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

### Modern SharePoint Experience
The Modern SharePoint experience is designed to provide a simple, intuitive, and mobile-friendly interface for users to interact with SharePoint content. It is characterized by a responsive design, simplified navigation, and a focus on content over customization.

### Classic SharePoint Experience
The Classic SharePoint experience is a traditional, legacy experience that provides a more complex and customizable user interface. It is characterized by a richer set of features, more advanced customization options, and a steeper learning curve.

## Standard Model

The standard model for Modern vs Classic SharePoint is to use the Modern experience for most use cases, reserving the Classic experience for scenarios that require advanced customization or legacy compatibility. This model prioritizes simplicity, usability, and mobile-friendliness, while still providing options for complex or specialized use cases.

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified, taking into account the specific requirements and constraints of the project.

## Common Patterns

* Using Modern SharePoint for team sites, document libraries, and basic content management
* Using Classic SharePoint for complex workflows, custom applications, or legacy system integration
* Migrating from Classic to Modern SharePoint for improved usability and mobile-friendliness

## Anti-Patterns

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues.

* Using Classic SharePoint for simple content management or team sites, resulting in unnecessary complexity and customization
* Ignoring the differences between Modern and Classic SharePoint, leading to inconsistent user experiences or unexpected behavior
* Failing to document or justify deviations from the standard model, resulting in maintenance or support issues

## Edge Cases

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions.

* Hybrid scenarios that combine Modern and Classic SharePoint experiences, such as using Modern pages with Classic web parts
* Custom or third-party solutions that rely on Classic SharePoint features or APIs
* Legacy systems or integrations that require Classic SharePoint compatibility

## Related Topics

* SharePoint Site Templates and Provisioning
* SharePoint Customization and Branding
* SharePoint Migration and Upgrade

## References

* Microsoft SharePoint Documentation: [https://docs.microsoft.com/en-us/sharepoint/](https://docs.microsoft.com/en-us/sharepoint/)
* SharePoint Developer Documentation: [https://docs.microsoft.com/en-us/sharepoint/dev/](https://docs.microsoft.com/en-us/sharepoint/dev/)

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-02-01 | Added section on edge cases and updated references |
| 1.2 | 2026-03-01 | Clarified definitions and updated standard model section |