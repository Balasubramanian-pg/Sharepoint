# 061 Using Flow Templates

Canonical documentation for 061 Using Flow Templates. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Flow Templates is to provide a standardized, reusable blueprint for automated processes. This topic addresses the inefficiencies and risks associated with "from-scratch" development, such as inconsistent logic, high manual effort, and the propagation of non-standard practices. By utilizing templates, organizations can ensure architectural alignment, accelerate deployment cycles, and lower the barrier to entry for process automation.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The conceptual framework of templated automation.
* Lifecycle management of flow blueprints.
* Parameterization and abstraction strategies.
* Governance and distribution models for reusable logic.

**Out of scope:**
* Specific vendor implementations (e.g., Salesforce Flows, Power Automate Templates, AWS Step Functions).
* Coding syntax for specific programming languages.
* UI/UX design of template marketplaces.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Flow Template** | A non-executable blueprint containing pre-configured logic, triggers, and actions intended to be cloned or inherited. |
| **Instance** | A specific, executable realization of a Flow Template, often containing unique configurations or data. |
| **Parameterization** | The process of defining variables within a template that must be supplied with specific values during instantiation. |
| **Hydration** | The act of injecting instance-specific data into a template to make it functional. |
| **Upstream Template** | The source blueprint from which multiple instances or child templates are derived. |
| **Logic Skeleton** | A template type that defines the order of operations but leaves the specific service integrations empty. |

## Core Concepts

### 1. The Blueprint Philosophy
A Flow Template is not a finished product but a foundational structure. It encapsulates "best practice" logic for a specific use case (e.g., "Lead Processing" or "Error Handling"). The philosophy centers on the separation of **Structure** (the flow) from **Context** (the specific data/environment).

### 2. Abstraction and Generalization
Effective templates abstract specific identifiers (like hardcoded IDs or specific server names) into generalized placeholders. This allows the same logic to be applied across different departments, environments, or business units without modifying the core engine.

### 3. Inheritance vs. Cloning
*   **Cloning (Decoupled):** The template is copied. Changes to the original template do not affect the copy. This offers maximum flexibility but creates a maintenance burden.
*   **Inheritance (Coupled):** The instance maintains a link to the template. Updates to the template propagate to the instance. This ensures consistency but limits local customization.

## Standard Model

The standard model for Using Flow Templates follows a four-stage lifecycle:

1.  **Definition & Generalization:** Identifying a recurring process and stripping it of instance-specific data to create a generic blueprint.
2.  **Categorization & Metadata:** Tagging the template with metadata (purpose, version, owner, dependencies) to ensure discoverability.
3.  **Instantiation & Configuration:** A user selects the template and provides the necessary parameters (the "Hydration" phase) to create a functional flow.
4.  **Validation:** The resulting instance is validated against the template’s intended logic to ensure that local modifications haven't broken the core functionality.

## Common Patterns

### The "Wrapper" Pattern
A template that provides a standard "Start" and "End" (e.g., logging, error handling, and telemetry) while allowing the user to insert custom logic in the middle.

### The "Industry Vertical" Pattern
Templates pre-configured for specific regulatory or industry requirements (e.g., a HIPAA-compliant data ingestion flow) where the logic is rigid but the data sources are variable.

### The "Utility" Pattern
Small, modular templates designed to perform a single, repetitive task (e.g., date formatting or string manipulation) that can be nested within larger flows.

## Anti-Patterns

*   **Hardcoding:** Including environment-specific values (URLs, IDs, Keys) within the template, which necessitates manual editing after instantiation.
*   **Over-Parameterization:** Providing too many configuration options, making the template as complex to set up as building a flow from scratch.
*   **The "God" Template:** Attempting to create a single template that handles too many disparate use cases through complex conditional branching, leading to unmaintainable logic.
*   **Version Neglect:** Failing to version templates, causing existing instances to break when the underlying blueprint is updated.

## Edge Cases

*   **Circular Dependencies:** When a template references a component that, in turn, requires the template to be active, creating a deployment deadlock.
*   **Orphaned Instances:** Instances that remain active after their parent template has been deprecated or deleted, leading to "shadow logic" within a system.
*   **Partial Hydration:** Scenarios where only some parameters are provided at instantiation, requiring the flow to remain in a "draft" or "invalid" state until completion.
*   **Cross-Environment Drift:** When a template is designed in a development environment but relies on features or services not yet available in the production environment.

## Related Topics

*   **042 Error Handling Frameworks:** How templates incorporate standardized recovery logic.
*   **088 Version Control Systems:** Managing the evolution of blueprints over time.
*   **105 Metadata Management:** The classification and discovery of reusable assets.
*   **112 Orchestration vs. Choreography:** How templated flows interact within a larger ecosystem.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |