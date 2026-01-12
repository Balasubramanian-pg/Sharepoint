# SharePoint Online vs Server vs Hybrid

Canonical documentation for SharePoint Online vs Server vs Hybrid. This document defines concepts, terminology, and standard usage.

## Purpose

The purpose of this documentation is to provide a comprehensive understanding of the differences, benefits, and trade-offs between SharePoint Online, SharePoint Server, and Hybrid environments. This topic exists to address the problem space of choosing the most suitable SharePoint deployment model for an organization's specific needs, considering factors such as scalability, security, cost, and functionality. By understanding the strengths and weaknesses of each deployment model, organizations can make informed decisions about their SharePoint infrastructure.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

**In scope:**
* Overview of SharePoint Online, SharePoint Server, and Hybrid environments
* Comparison of features, benefits, and limitations of each deployment model
* Best practices for choosing a deployment model
* Common scenarios and use cases for each deployment model

**Out of scope:**
* Tool-specific implementations (e.g., third-party tools and software)
* Vendor-specific behavior (e.g., Microsoft-specific features or limitations)
* Detailed technical instructions for setting up or configuring SharePoint environments

## Definitions

| Term | Definition |
|------|------------|
| SharePoint Online | A cloud-based version of SharePoint, hosted and managed by Microsoft |
| SharePoint Server | An on-premises version of SharePoint, installed and managed by an organization |
| Hybrid Environment | A deployment model that combines SharePoint Online and SharePoint Server, allowing for integration and synchronization between the two |
| Cloud-First Strategy | An approach that prioritizes cloud-based services and applications, with on-premises infrastructure used only when necessary |
| On-Premises Strategy | An approach that prioritizes on-premises infrastructure and applications, with cloud-based services used only when necessary |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

### SharePoint Online
SharePoint Online is a cloud-based version of SharePoint that provides a scalable, secure, and cost-effective way to deploy SharePoint. It offers a range of features and functionalities, including collaboration, content management, and business process automation.

### SharePoint Server
SharePoint Server is an on-premises version of SharePoint that provides a high degree of control and customization. It is suitable for organizations with complex security requirements, large-scale deployments, or specific integration needs.

### Hybrid Environment
A Hybrid Environment combines the benefits of SharePoint Online and SharePoint Server, allowing organizations to integrate and synchronize their on-premises and cloud-based SharePoint deployments. This approach enables organizations to take advantage of cloud-based services while still maintaining control over their on-premises infrastructure.

## Standard Model

The standard model for SharePoint deployment is a Cloud-First Strategy, where SharePoint Online is the primary deployment model, and SharePoint Server is used only when necessary. This approach provides a scalable, secure, and cost-effective way to deploy SharePoint, while also allowing for integration with on-premises infrastructure when required.

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified.

## Common Patterns

* Using SharePoint Online for collaboration and content management, while using SharePoint Server for custom applications and integrations
* Implementing a Hybrid Environment to integrate on-premises and cloud-based SharePoint deployments
* Using SharePoint Online as a disaster recovery solution for on-premises SharePoint deployments

## Anti-Patterns

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues.

* Using SharePoint Server as the primary deployment model, without considering the benefits of SharePoint Online
* Implementing a Hybrid Environment without a clear understanding of the integration requirements and potential complexities
* Using SharePoint Online for large-scale, complex deployments without proper planning and configuration

## Edge Cases

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions.

* Organizations with highly customized or complex SharePoint deployments that require significant integration with on-premises infrastructure
* Organizations with strict security or compliance requirements that may not be met by SharePoint Online
* Organizations with limited internet connectivity or bandwidth, which may impact the performance of SharePoint Online

## Related Topics

* SharePoint Architecture and Design
* SharePoint Security and Compliance
* SharePoint Migration and Upgrade

## References

* Microsoft SharePoint Documentation
* SharePoint Online Service Description
* SharePoint Server Documentation

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-02-15 | Added section on Edge Cases |
| 1.2 | 2026-03-20 | Updated section on Common Patterns and Anti-Patterns |