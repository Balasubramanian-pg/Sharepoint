# What is SharePoint

Canonical documentation for What is SharePoint. This document defines concepts, terminology, and standard usage.

## Purpose

SharePoint is a web-based collaborative platform that integrates with Microsoft Office, providing a comprehensive set of tools for document management, content management, and workflow automation. The purpose of this topic is to provide a clear understanding of SharePoint's capabilities, features, and applications, addressing the problem space of information management, collaboration, and workflow automation in organizations. This documentation aims to serve as a foundational resource for individuals seeking to understand the core concepts, terminology, and best practices associated with SharePoint.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

**In scope:**
* Overview of SharePoint's core features and capabilities
* Definitions of key terms and concepts related to SharePoint
* Standard model for SharePoint implementation and usage
* Common patterns and anti-patterns associated with SharePoint

**Out of scope:**
* Tool-specific implementations, such as custom coding or third-party integrations
* Vendor-specific behavior, including Microsoft-specific features or limitations
* Detailed instructions for SharePoint configuration or administration

## Definitions

| Term | Definition |
|------|------------|
| SharePoint | A web-based collaborative platform developed by Microsoft, integrating document management, content management, and workflow automation capabilities. |
| SharePoint Farm | A collection of SharePoint servers that work together to provide a scalable and fault-tolerant environment for SharePoint applications. |
| SharePoint Site | A top-level container for SharePoint content, including lists, libraries, and pages. |
| SharePoint List | A collection of items, such as documents, tasks, or contacts, stored in a SharePoint site. |
| SharePoint Library | A repository for storing and managing files, such as documents, images, or videos, in a SharePoint site. |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

### SharePoint Architecture
SharePoint's architecture is based on a client-server model, where the client is typically a web browser, and the server is a SharePoint farm. This architecture provides a scalable and secure environment for collaboration and content management.

### SharePoint Content Management
SharePoint provides a range of content management features, including document libraries, lists, and pages. These features enable users to create, manage, and share content, such as documents, images, and videos.

## Standard Model

The standard model for SharePoint implementation and usage involves the following components:
1. **SharePoint Farm**: A collection of SharePoint servers that work together to provide a scalable and fault-tolerant environment for SharePoint applications.
2. **SharePoint Site**: A top-level container for SharePoint content, including lists, libraries, and pages.
3. **SharePoint Lists and Libraries**: Collections of items, such as documents, tasks, or contacts, stored in a SharePoint site.
4. **SharePoint Pages**: Web pages that provide a user interface for accessing and interacting with SharePoint content.

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified.

## Common Patterns

* **Document Management**: Using SharePoint to manage and store documents, such as policies, procedures, and reports.
* **Collaboration**: Using SharePoint to facilitate collaboration among teams, including project management, task assignment, and communication.
* **Content Publishing**: Using SharePoint to publish and manage content, such as news articles, blog posts, and announcements.

## Anti-Patterns

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues.

* **Over-Engineering**: Creating complex, custom solutions that are difficult to maintain and support.
* **Under-Utilization**: Failing to leverage SharePoint's built-in features and capabilities, leading to inefficient use of resources.
* **Lack of Governance**: Failing to establish clear policies, procedures, and guidelines for SharePoint usage and management.

## Edge Cases

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions.

* **Large File Uploads**: Handling large file uploads, such as videos or high-resolution images, which can impact SharePoint performance and storage.
* **Custom Code**: Integrating custom code, such as JavaScript or C#, which can introduce security risks and maintenance challenges.
* **Third-Party Integrations**: Integrating third-party tools and services, which can introduce compatibility issues and support challenges.

## Related Topics

* **Microsoft Office**: Integrating SharePoint with Microsoft Office applications, such as Word, Excel, and PowerPoint.
* **Azure Active Directory**: Using Azure Active Directory to manage user identities and authentication for SharePoint.
* **Power Automate**: Using Power Automate to automate workflows and business processes in SharePoint.

## References

* Microsoft SharePoint Documentation: <https://docs.microsoft.com/en-us/sharepoint/>
* SharePoint Developer Documentation: <https://docs.microsoft.com/en-us/sharepoint/dev/>
* SharePoint Community Forum: <https://techcommunity.microsoft.com/t5/sharepoint/ct-p/SharePoint>

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-01-15 | Added section on edge cases |
| 1.2 | 2026-01-20 | Updated references to include SharePoint community forum |