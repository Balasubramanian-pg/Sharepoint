# SharePoint Groups

Canonical documentation for SharePoint Groups. This document defines concepts, terminology, and standard usage.

## Purpose

SharePoint Groups is a fundamental concept in SharePoint that enables the management of user permissions and access to site content. The purpose of this topic is to provide a comprehensive understanding of SharePoint Groups, including their creation, management, and usage. This documentation addresses the problem space of access control and permission management in SharePoint, ensuring that users have the necessary permissions to perform their tasks while maintaining the security and integrity of site content.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

**In scope:**
* Concept of SharePoint Groups and their role in access control
* Group types (e.g., SharePoint Groups, Active Directory Groups)
* Group management (e.g., creating, editing, deleting groups)
* Group membership and permission assignment

**Out of scope:**
* Tool-specific implementations (e.g., PowerShell scripts, third-party tools)
* Vendor-specific behavior (e.g., Microsoft 365 vs. on-premises SharePoint)
* Detailed instructions for specific SharePoint versions or configurations

## Definitions

| Term | Definition |
|------|------------|
| SharePoint Group | A collection of users or other groups that can be assigned permissions to access SharePoint site content |
| Group Type | A categorization of groups based on their purpose or membership (e.g., SharePoint Group, Active Directory Group) |
| Group Membership | The set of users or groups that are members of a SharePoint Group |
| Permission | A specific right or access level assigned to a user or group to perform actions on SharePoint site content |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

### Concept One: Group Types
SharePoint Groups can be categorized into different types based on their purpose or membership. Understanding these group types is essential for effective group management and permission assignment.

### Concept Two: Group Membership and Permission Assignment
Group membership and permission assignment are critical aspects of SharePoint Groups. Users or groups can be added to or removed from a SharePoint Group, and permissions can be assigned to the group to control access to site content.

## Standard Model

The standard model for SharePoint Groups involves creating and managing groups using the SharePoint user interface or PowerShell. The recommended approach is to create groups that align with the organization's structure and business needs, and to assign permissions to these groups based on the principle of least privilege.

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified.

## Common Patterns

* Creating separate groups for different departments or teams to manage access to specific site content
* Using Active Directory Groups to synchronize group membership with SharePoint Groups
* Assigning permissions to groups based on job functions or roles

## Anti-Patterns

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues.

* Creating too many groups with overlapping membership or permissions
* Assigning permissions to individual users instead of groups
* Not regularly reviewing and updating group membership and permissions

## Edge Cases

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions.

* Nested groups (i.e., groups within groups)
* Groups with external members (e.g., partners, vendors)
* Groups with unique permission requirements (e.g., read-only access to specific site content)

## Related Topics

* SharePoint Permissions and Access Control
* Active Directory Integration with SharePoint
* SharePoint Site Management and Governance

## References

* Microsoft SharePoint documentation: [https://docs.microsoft.com/en-us/sharepoint/](https://docs.microsoft.com/en-us/sharepoint/)
* SharePoint Group management: [https://docs.microsoft.com/en-us/sharepoint/sharepoint-groups](https://docs.microsoft.com/en-us/sharepoint/sharepoint-groups)

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-02-01 | Added section on edge cases |
| 1.2 | 2026-03-01 | Updated references to include Microsoft SharePoint documentation |