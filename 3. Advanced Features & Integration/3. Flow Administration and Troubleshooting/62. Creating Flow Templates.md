# 062 Creating Flow Templates

Canonical documentation for 062 Creating Flow Templates. This document defines concepts, terminology, and standard usage.

## Purpose
The creation of flow templates addresses the need for scalability, consistency, and rapid deployment within automated orchestration environments. By abstracting common logic into reusable blueprints, organizations can reduce manual configuration errors, enforce governance standards, and accelerate the development lifecycle. Flow templates serve as the foundational architecture for modular workflow design, allowing for the separation of process logic from specific execution data.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Theoretical frameworks for template design and abstraction.
* Parameterization strategies for dynamic instantiation.
* Structural requirements for reusable workflow logic.
* Governance and versioning principles for template management.

**Out of scope:**
* Specific vendor-specific syntax (e.g., YAML, JSON, or proprietary GUI steps).
* Runtime execution performance tuning.
* Third-party integration authentication methods.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Flow Template** | A pre-configured, reusable blueprint of a process that defines the sequence of operations, logic, and placeholders for a specific workflow. |
| **Instantiation** | The process of creating a functional, executable instance of a flow based on a template by providing specific configuration values. |
| **Parameterization** | The practice of replacing static values with variables or placeholders to allow the template to adapt to different contexts. |
| **Abstraction** | The removal of specific implementation details to focus on the high-level logic and structure of the process. |
| **Hardcoding** | The discouraged practice of embedding fixed data directly into a template, which reduces reusability. |
| **Orchestrator** | The underlying system or engine that interprets the template and manages the execution of the resulting flow. |

## Core Concepts

### 1. Reusability and Modularity
The primary value of a flow template is its ability to be reused across different departments, projects, or use cases. Modularity ensures that a template can function as a standalone unit or as a component within a larger "Parent-Child" flow architecture.

### 2. Logical Abstraction
A well-designed template focuses on the *intent* of the process rather than the *mechanics* of the data. For example, a "Notification Template" should define the logic for sending an alert without being tethered to a specific email address or messaging platform.

### 3. Parameterization
Templates must expose "hooks" or inputs. These allow the user (or the calling system) to inject specific data at the moment of instantiation. This ensures the template remains "stateless" until it is deployed.

### 4. Governance and Compliance
Templates act as a "Golden Image" for processes. By creating a template, an organization ensures that every flow derived from it adheres to security, logging, and error-handling standards.

## Standard Model

The standard model for creating flow templates follows a four-phase lifecycle:

1.  **Identification:** Analyzing recurring manual processes to determine if they meet the criteria for templatization (frequency, complexity, and variability).
2.  **Structural Design:** Mapping the logical sequence of steps, decision points, and loops. This phase defines the "Skeleton" of the flow.
3.  **Variable Mapping:** Identifying which elements must be dynamic. This involves defining input parameters (required vs. optional) and output schemas.
4.  **Validation and Publication:** Testing the template against various data sets to ensure logical integrity, followed by versioning and releasing it to a central repository or library.

## Common Patterns

### The Skeleton Pattern
A template that provides only the structural framework (e.g., Error Handling, Logging, and Authentication) but leaves the core business logic to be defined by the implementer.

### The Task-Specific Pattern
A template designed for a highly specific, repeatable technical task, such as "Database Record Synchronization" or "User Provisioning," where only the source and destination endpoints change.

### The Wrapper Pattern
A template that wraps around an existing API or service to provide a standardized interface for other flows to consume, ensuring consistent data formatting.

## Anti-Patterns

*   **The God Template:** Attempting to create a single template that handles too many disparate use cases, leading to excessive conditional logic and high maintenance costs.
*   **Hardcoded Credentials:** Including sensitive information (API keys, passwords) directly within the template structure rather than using a secure vault or parameter.
*   **Lack of Versioning:** Updating a template in place without version control, which can break existing flows instantiated from previous versions.
*   **Over-Parameterization:** Requiring an excessive number of inputs for simple tasks, which increases the barrier to entry for users.

## Edge Cases

*   **Circular Dependencies:** A scenario where Template A calls Template B, which in turn attempts to instantiate Template A. This must be prevented at the architectural level.
*   **Deprecated Components:** When a template relies on a sub-process or connector that is retired. The template must have a strategy for "Graceful Degradation" or clear version deprecation notices.
*   **Dynamic Schema Changes:** If the data structure of an external system changes, the template must be robust enough to handle unexpected fields or provide clear validation errors.

## Related Topics

*   **061 Workflow Orchestration Principles:** The foundational logic governing how flows are executed.
*   **063 Flow Versioning and Lifecycle Management:** The standards for managing updates to existing templates.
*   **084 Error Handling and Exception Management:** Standardized methods for managing failures within a flow.
*   **102 Parameterization and Variable Scoping:** Deep dive into how data is passed between flow segments.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |