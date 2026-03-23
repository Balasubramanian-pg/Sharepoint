# 002 Power Automate Capabilities

Canonical documentation for 002 Power Automate Capabilities. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Power Automate capabilities is to provide a unified framework for Digital Process Automation (DPA), Robotic Process Automation (RPA), and Business Process Management (BPM). This topic addresses the problem of fragmented workflows, manual data entry, and the lack of interoperability between disparate legacy and modern systems. By providing a scalable orchestration layer, these capabilities allow organizations to codify business logic and automate repetitive tasks across heterogeneous environments.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Orchestration Paradigms:** Event-driven, scheduled, and manual execution models.
* **Automation Types:** API-based (Cloud), UI-based (Desktop), and Stage-based (Business Process).
* **Intelligence Integration:** The application of machine learning and cognitive services within automated workflows.
* **Process Analysis:** Methodologies for discovering and optimizing existing workflows.

**Out of scope:**
* **Specific Vendor Connectors:** Detailed documentation for third-party integrations (e.g., Salesforce, SAP).
* **Licensing and Pricing:** Commercial structures and subscription tiers.
* **Hardware Specifications:** Local machine requirements for desktop-based agents.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Trigger** | The specific event or condition that initiates the execution of an automated workflow. |
| **Action** | A discrete operation performed by the automation engine, such as data retrieval, transformation, or transmission. |
| **Connector** | A proxy or wrapper around an API that allows the automation engine to interact with an external service. |
| **Orchestration** | The coordination and management of multiple automated tasks to achieve a complex business outcome. |
| **Attended Automation** | UI-based automation that requires human intervention or runs on a user's workstation during their session. |
| **Unattended Automation** | UI-based automation that runs independently on a server or virtual machine without human presence. |
| **Semantic Processing** | The capability to extract meaning and structured data from unstructured sources (e.g., documents, emails). |

## Core Concepts

### 1. Digital Process Automation (DPA)
DPA focuses on the integration of modern applications via APIs. It leverages high-speed, reliable connections to move data and trigger logic across cloud and on-premises services. This is the primary layer for system-to-system communication.

### 2. Robotic Process Automation (RPA)
RPA addresses the "last mile" of automation where APIs are unavailable. It simulates human interaction with user interfaces (UI) to automate legacy software, terminal emulators, or web applications that lack programmatic access.

### 3. Business Process Management (BPM)
BPM capabilities provide a guided experience for human-centric processes. It defines stages, steps, and required data inputs to ensure organizational compliance and consistency in multi-step, multi-persona workflows.

### 4. Process and Task Mining
These capabilities involve the data-driven discovery of automation opportunities. By analyzing system logs (Process Mining) or user interactions (Task Mining), the platform identifies bottlenecks and high-ROI candidates for automation.

## Standard Model
The standard model for Power Automate capabilities follows a **Layered Orchestration Architecture**:

1.  **Trigger Layer:** Monitors for events (e.g., Webhooks, Polling, Schedules).
2.  **Logic Layer:** Evaluates conditions, loops, and branching logic (Control Flow).
3.  **Data Transformation Layer:** Normalizes and maps data between disparate schemas.
4.  **Execution Layer:** Dispatches commands to APIs (DPA) or UI Agents (RPA).
5.  **Persistence Layer:** Maintains state for long-running processes and approval cycles.

## Common Patterns

*   **The Gatekeeper Pattern:** Using a manual or automated approval step before a high-impact action is executed.
*   **The Sidecar Pattern:** Running a secondary automation in parallel to a primary process to handle logging, notifications, or telemetry.
*   **The Retry/Backoff Pattern:** Implementing logic to handle transient failures by pausing and re-attempting actions with increasing intervals.
*   **The Fan-out/Fan-in Pattern:** Triggering multiple parallel processes from a single event and aggregating the results before proceeding.

## Anti-Patterns

*   **The "God Flow":** Creating a single, massive automation that handles too many unrelated business logic paths, leading to unmaintainable complexity.
*   **Hardcoded Dependencies:** Embedding environment-specific values (URLs, IDs) directly into the logic rather than using a configuration or environment variable layer.
*   **UI-First Bias:** Using RPA (UI-based) for a task where a stable API (DPA) is available, resulting in increased fragility.
*   **Infinite Loops:** Designing triggers that are activated by the output of their own actions without sufficient exit conditions.

## Edge Cases

*   **Long-Running Transactions:** Workflows that must remain active for weeks or months (e.g., legal approvals). These require state persistence beyond standard session timeouts.
*   **High-Frequency Throttling:** Scenarios where the volume of trigger events exceeds the API limits of the source or destination systems.
*   **Dynamic Schema Evolution:** Handling situations where the structure of the data returned by a connector changes unexpectedly, breaking downstream mappings.
*   **Air-Gapped Environments:** Implementing RPA in environments with no outbound internet connectivity, requiring specialized gateway configurations.

## Related Topics

*   **001 Automation Governance:** Frameworks for managing security, compliance, and ALM.
*   **003 Connector Architecture:** Deep dive into the technical structure of API wrappers.
*   **004 AI Integration:** Specifics on utilizing Large Language Models (LLMs) and Cognitive Services.
*   **005 Error Handling and Resiliency:** Advanced strategies for exception management.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |