# Internal vs External Users

Canonical documentation for Internal vs External Users. This document defines concepts, terminology, and standard usage.

## Purpose

The distinction between internal and external users is a crucial aspect of system design, security, and access control. This topic exists to provide clarity on the differences between these two types of users, the implications of each, and the best practices for managing their interactions with a system. The problem space it addresses includes the need for secure authentication and authorization, data protection, and compliance with regulatory requirements. Understanding the differences between internal and external users is essential for ensuring the confidentiality, integrity, and availability of system resources and data.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

**In scope:**
* Definitions and characteristics of internal and external users
* Access control and authentication mechanisms for each type of user
* Security considerations and best practices for managing internal and external user interactions

**Out of scope:**
* Tool-specific implementations of access control and authentication
* Vendor-specific behavior and configurations
* Detailed technical instructions for implementing security measures

## Definitions

| Term | Definition |
|------|------------|
| Internal User | An individual who is part of the organization or entity that owns or operates the system, and has been granted access to system resources and data for legitimate business purposes. |
| External User | An individual who is not part of the organization or entity that owns or operates the system, but has been granted access to system resources and data for specific purposes, such as customers, partners, or contractors. |
| Access Control | The process of granting or denying access to system resources and data based on user identity, role, and permissions. |
| Authentication | The process of verifying the identity of a user, device, or system. |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

### Internal Users
Internal users are typically employees, contractors, or third-party vendors who have been granted access to system resources and data to perform their job functions. They often have a higher level of access and privileges than external users, and are subject to the organization's security policies and procedures.

### External Users
External users, on the other hand, are individuals who interact with the system for specific purposes, such as customers, partners, or contractors. They typically have limited access to system resources and data, and are subject to the organization's security policies and procedures, as well as any applicable laws and regulations.

## Standard Model

The standard model for managing internal and external users involves a combination of access control, authentication, and authorization mechanisms. This includes:

* Implementing a robust authentication mechanism, such as multi-factor authentication, to verify the identity of users
* Assigning roles and permissions to users based on their job functions and responsibilities
* Implementing access control mechanisms, such as firewalls and intrusion detection systems, to protect system resources and data
* Regularly reviewing and updating user access and permissions to ensure they are aligned with business needs and security requirements

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified.

## Common Patterns

* Using role-based access control (RBAC) to manage user access and permissions
* Implementing a least privilege access model, where users are granted only the access and permissions necessary to perform their job functions
* Using external identity providers, such as social media or cloud-based services, to authenticate external users

## Anti-Patterns

* Allowing internal users to share credentials or access tokens with external users
* Failing to regularly review and update user access and permissions
* Using weak or outdated authentication mechanisms, such as single-factor authentication or plaintext passwords

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues, and can compromise the security and integrity of system resources and data.

## Edge Cases

* Managing access and permissions for external users who are also internal users, such as contractors or partners
* Handling situations where internal users are also external users, such as employees who are also customers
* Managing access and permissions for users who have multiple roles or responsibilities, such as administrators who are also developers

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions, so it's essential to carefully consider and plan for them.

## Related Topics

* Identity and Access Management (IAM)
* Authentication and Authorization
* Role-Based Access Control (RBAC)
* Security and Compliance

## References

* NIST Special Publication 800-63-3: Electronic Authentication Guideline
* ISO/IEC 27001:2013: Information security management systems — Requirements
* OWASP Authentication Cheat Sheet

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-02-15 | Added section on edge cases and updated references |
| 1.2 | 2026-03-20 | Updated definitions and core concepts to reflect industry best practices |