# 077 Common Controls Overview

Canonical documentation for 077 Common Controls Overview. This document defines concepts, terminology, and standard usage.

## Purpose
The 077 Common Controls framework exists to streamline the management, implementation, and assessment of security and operational requirements across an organization. By identifying and centralizing controls that apply to multiple systems or business units, organizations can reduce redundant efforts, ensure consistency in security posture, and optimize resource allocation. 

This topic addresses the problem of "compliance fatigue" and the inefficiency of implementing identical safeguards independently for every individual information system. It provides a structured approach for "implementing once and assessing many."

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Inheritance Models:** The theoretical framework for how systems consume controls provided by an external entity.
* **Centralized Governance:** The principles of managing shared safeguards.
* **Control Categorization:** Distinguishing between common, hybrid, and system-specific controls.
* **Responsibility Mapping:** Defining the relationship between control providers and control consumers.

**Out of scope:**
* **Specific vendor implementations:** (e.g., specific configurations for AWS, Azure, or SAP).
* **Specific Regulatory Frameworks:** While applicable to NIST, ISO, or SOC2, this document focuses on the underlying concept of commonality rather than the requirements of a single standard.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Common Control** | A security or operational control that is inheritable by one or more organizational information systems. |
| **Control Inheritance** | A situation where an information system or application receives protection from a control that is developed, implemented, assessed, and monitored by an external entity or centralized department. |
| **Control Provider** | The entity (department, system, or organization) responsible for the development, implementation, and maintenance of a common control. |
| **Control Consumer** | The system, application, or business unit that relies on the common control to meet its requirements. |
| **Hybrid Control** | A control that is characterized by having both a common component and a system-specific component. |
| **System-Specific Control** | A control that is implemented entirely within a specific information system and is not shared with others. |

## Core Concepts

### The Principle of Inheritance
The fundamental concept of 077 Common Controls is **Inheritance**. Instead of every application team building their own physical data center security, identity provider, or incident response team, they "inherit" these protections from a centralized provider. This creates a "Single Source of Truth" for the control's effectiveness.

### Centralized Assessment
Common controls are assessed once by an independent auditor or internal assessor. The results of this assessment are then documented in a way that allows multiple system owners to reference the findings in their own authorization or compliance packages.

### Shared Responsibility
Common controls necessitate a Shared Responsibility Model. The provider is responsible for the *efficacy* of the control, while the consumer is responsible for *verifying* that the control meets their specific needs and is correctly integrated.

## Standard Model

The standard model for Common Controls follows a tripartite structure:

1.  **Identification:** The organization identifies controls that are candidates for commonality (e.g., Physical Security, Personnel Security, Awareness Training).
2.  **Implementation & Documentation:** The Control Provider implements the control and maintains a "Common Control Provider Package" which includes the implementation details and assessment results.
3.  **Authorization & Reference:** System owners (Consumers) reference the Common Control Provider Package in their System Security Plans (SSP), explicitly stating which controls are inherited.

## Common Patterns

### Infrastructure as a Common Control
The most frequent pattern involves physical or cloud infrastructure. The underlying hardware, cooling, and physical access logs are managed by the infrastructure team and inherited by all applications running on that hardware.

### Policy and Procedure Inheritance
Organizational-level policies (e.g., an Acceptable Use Policy) are typically common controls. Individual systems do not write their own version of the policy but inherit the organizational standard.

### Centralized Identity and Access Management (IAM)
A centralized directory service (e.g., LDAP or SSO) acts as a common control provider for authentication. The individual application remains responsible for authorization (system-specific) but inherits the authentication mechanism (common).

## Anti-Patterns

*   **The "Black Box" Fallacy:** Assuming a control is fully inherited without verifying the boundary. (e.g., assuming a cloud provider handles all data encryption when they only provide the *capability* for encryption).
*   **Redundant Implementation:** Implementing a system-specific version of a control that is already provided centrally, leading to configuration drift and wasted resources.
*   **Stale Documentation:** Failing to update the Common Control Provider Package, causing all inheriting systems to rely on outdated or invalid assessment data.
*   **Shadow Controls:** Implementing "common" controls informally without official designation, leading to a lack of accountability when the control fails.

## Edge Cases

*   **Partial Inheritance (Hybrid):** A scenario where a centralized team provides a firewall (common), but the application team must define the specific port rules (system-specific). This requires precise documentation to avoid gaps.
*   **Legacy System Incompatibility:** Older systems that cannot integrate with common controls (e.g., a legacy app that cannot use the corporate SSO). These require "Compensating Controls" which are system-specific.
*   **Cross-Jurisdictional Controls:** When a common control is provided in one legal jurisdiction (e.g., EU) but consumed by a system in another (e.g., USA), creating potential regulatory conflicts.

## Related Topics

*   **082 Shared Responsibility Models:** Detailed breakdown of the provider/consumer interface.
*   **104 Risk Management Framework (RMF):** The broader process in which common controls are selected and authorized.
*   **210 Identity and Access Management:** A primary domain for common control implementation.
*   **015 Compliance Baseline Management:** Defining the set of controls that all systems must inherit.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |