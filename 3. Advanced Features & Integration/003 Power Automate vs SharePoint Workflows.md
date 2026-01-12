# 003 Power Automate vs SharePoint Workflows

Canonical documentation for 003 Power Automate vs SharePoint Workflows. This document defines concepts, terminology, and standard usage.

## Purpose
The transition from SharePoint Workflows to Power Automate represents a fundamental shift in enterprise automation architecture. This topic exists to define the structural and philosophical differences between legacy, platform-bound automation (SharePoint Workflows) and modern, API-driven cloud orchestration (Power Automate). It addresses the problem space of process automation, data integration, and the lifecycle management of business logic within the Microsoft ecosystem.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative, focusing on the architectural evolution rather than specific UI navigation.

## Scope
**In scope:**
* Comparison of architectural foundations (Internal vs. External execution).
* Theoretical boundaries of connectivity and scope.
* Governance and security models.
* Triggering mechanisms and event-driven logic.

**Out of scope:**
* Step-by-step migration tutorials.
* Specific licensing costs or SKU comparisons.
* Third-party workflow engines (e.g., Nintex, K2).

## Definitions
| Term | Definition |
|------|------------|
| **SharePoint Workflow** | A legacy automation engine (2010/2013) hosted natively within the SharePoint environment, tightly coupled to SharePoint lists and libraries. |
| **Power Automate** | A modern, cloud-based service (formerly Microsoft Flow) that orchestrates tasks across multiple disparate services via APIs. |
| **Trigger** | The specific event (e.g., item creation, timer, or manual invocation) that initiates a workflow instance. |
| **Connector** | A wrapper around an API that allows the automation engine to communicate with a specific service or data source. |
| **Orchestration** | The automated arrangement, coordination, and management of complex computer systems, middleware, and services. |
| **Throttling** | The intentional slowing or limiting of process execution to preserve system resources and stability. |

## Core Concepts

### 1. Architectural Coupling
SharePoint Workflows are **internally coupled**. The logic resides and executes within the SharePoint content service. Power Automate is **externally decoupled**; it exists as a standalone service that interacts with SharePoint (and other services) as a peer via the REST API.

### 2. Scope of Influence
*   **Legacy Model:** Limited primarily to the SharePoint site collection or tenant boundary. Interacting with external systems requires custom code (Web Activities) or complex service bus configurations.
*   **Modern Model:** Designed for cross-service orchestration. It treats SharePoint as one of hundreds of potential data sources, enabling seamless movement of data between CRM, ERP, and productivity suites.

### 3. Execution Environment
SharePoint Workflows rely on the Workflow Manager or the SharePoint Foundation host. Power Automate operates on a globally distributed, serverless architecture, allowing for higher scalability but introducing different latency and throttling profiles.

## Standard Model
The standard model for modern enterprise automation favors **Power Automate** as the primary engine. The model follows these principles:
1.  **Event-Driven:** Workflows should be triggered by state changes in data rather than polling.
2.  **API-First:** All interactions should occur through authenticated API calls facilitated by connectors.
3.  **Low-Code Logic:** Business logic should be readable and maintainable by non-developers, while allowing for "pro-code" extensibility via Azure Functions or Custom Connectors.
4.  **Centralized Governance:** Automation should be subject to Data Loss Prevention (DLP) policies and centralized monitoring.

## Common Patterns
*   **Sequential Approval:** A linear path where an item must be reviewed by one or more actors before a final state is reached.
*   **State Machine (Simulated):** Using "Switch" cases and status columns to move an item through various non-linear phases of a lifecycle.
*   **Data Synchronization:** Mirroring data from a SharePoint list to an external SQL database or another SaaS application.
*   **Scheduled Maintenance:** Running recurring logic (e.g., daily) to identify overdue tasks or generate reports.

## Anti-Patterns
*   **The "Lift and Shift" Fallacy:** Attempting to replicate legacy SharePoint Workflow logic 1:1 in Power Automate without accounting for the differences in execution limits and connector behavior.
*   **Hardcoded Credentials:** Using individual user accounts for service connections rather than Service Principals or Service Accounts.
*   **Infinite Loops:** Designing triggers that update the same item that triggered them without adequate conditional checks, leading to recursive execution.
*   **Monolithic Flows:** Creating a single, massive flow with hundreds of actions instead of modularizing logic into "Child Flows."

## Edge Cases
*   **High-Frequency Transactions:** Scenarios requiring thousands of executions per minute may exceed Power Automate's request limits, necessitating a move to Azure Logic Apps.
*   **On-Premises Data Access:** Accessing legacy SharePoint Server data requires the installation and maintenance of an On-Premises Data Gateway.
*   **Long-Running Processes:** Workflows that must wait for months for an action may exceed the standard 30-day execution limit of Power Automate, requiring state-persistence strategies.
*   **Large File Handling:** Moving files over a certain size (e.g., >100MB) requires specific "Chunking" configurations or specialized connectors.

## Related Topics
*   **004 Azure Logic Apps:** The enterprise-grade equivalent of Power Automate for pro-developers.
*   **012 Data Loss Prevention (DLP):** Security frameworks for controlling data flow between connectors.
*   **025 SharePoint REST API:** The underlying communication layer used by modern automation.
*   **040 Managed Environments:** Governance features for scaling automation across an organization.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |