# 060 Flow Templates

Canonical documentation for 060 Flow Templates. This document defines concepts, terminology, and standard usage.

## Purpose
060 Flow Templates exist to provide a standardized, reusable blueprint for automated sequences of operations. In complex systems, repetitive logic and workflow structures often lead to configuration drift, increased maintenance overhead, and inconsistent execution. Flow Templates address these challenges by decoupling the logical structure of a process from its specific execution context.

By utilizing templates, organizations can ensure that best practices, security protocols, and operational standards are baked into the foundation of every automated process, allowing for rapid scaling and simplified governance.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Structural definitions of reusable workflow blueprints.
* Mechanisms for parameterization and abstraction.
* Lifecycle management of templates (versioning, instantiation).
* Governance and standardization principles.

**Out of scope:**
* Specific vendor implementations (e.g., Salesforce Flows, Power Automate, GitHub Actions).
* Low-level code syntax or specific programming language libraries.
* Hardware-level execution details.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Flow Template** | A non-executable blueprint defining the logic, sequence, and requirements of a workflow. |
| **Instance** | A specific, executable realization of a Flow Template, populated with concrete data. |
| **Parameter** | A placeholder within a template that accepts external input to customize the instance behavior. |
| **Node/Step** | An individual unit of work or logic within the flow structure. |
| **Orchestrator** | The system responsible for interpreting the template and managing the execution of its instances. |
| **Binding** | The process of mapping specific data sources or environment variables to template parameters. |

## Core Concepts

### Abstraction of Logic
The fundamental concept of a Flow Template is the separation of *what* is being done from *how* it is configured. A template defines the "what"—the sequence of events, the conditional branches, and the error handling—without requiring knowledge of the specific credentials, endpoints, or variables used during a specific run.

### Parameterization
Templates rely on parameterization to remain flexible. By defining inputs at the template level, the same logic can be applied to different environments (e.g., Development vs. Production) or different business units without modifying the underlying flow structure.

### Determinism
A well-defined Flow Template must be deterministic. Given the same set of input parameters and environmental conditions, the logical path through the template should remain consistent and predictable.

## Standard Model

The standard model for 060 Flow Templates follows a four-stage lifecycle:

1.  **Definition:** The architect defines the nodes, transitions, and logic. This includes identifying which elements are static and which are parameterized.
2.  **Validation:** The template is checked against schema requirements and logical integrity (e.g., ensuring no dead ends or infinite loops).
3.  **Instantiation:** A user or system provides the necessary parameters to create a "Live Flow" or "Instance."
4.  **Execution:** The orchestrator processes the instance, logging outcomes and handling state transitions.

### Structural Components
*   **Trigger Definition:** The event or condition that initiates the flow.
*   **Logic Gates:** Decision points (If/Else, Switch) that determine the path.
*   **Action Nodes:** The functional units that interact with external systems or transform data.
*   **Error Boundaries:** Defined paths for handling failures within specific segments of the flow.

## Common Patterns

### The Skeleton Pattern
A minimal template that defines only the mandatory compliance and logging steps, requiring the user to fill in the core business logic. This ensures all flows meet organizational standards for observability.

### The Wrapper Pattern
A template designed to encapsulate a complex, sensitive process (like a database write) with pre- and post-validation steps, exposing only a simplified interface to the end user.

### The Modular/Nested Pattern
A hierarchical approach where a "Master Template" calls several "Sub-Templates." This promotes extreme reusability and allows for the updating of a single sub-component to reflect across all parent flows.

## Anti-Patterns

### Hardcoding
Embedding specific IDs, URLs, or credentials directly into the template. This defeats the purpose of reusability and creates significant security risks.

### The "God" Template
Creating a single, massive template with excessive conditional branching to handle every possible use case. This leads to unmaintainable "spaghetti logic" and high failure rates.

### Tight Coupling
Designing a template that is overly dependent on the specific state or schema of an external system, making the template fragile to changes in outside environments.

## Edge Cases

### Version Skew
When an instance is running on Version 1.0 of a template, but Version 2.0 is released. The system must determine whether to allow the instance to complete on the old logic or attempt a mid-run migration (which is high-risk).

### Circular Dependencies
In modular patterns, Template A calls Template B, which eventually calls Template A. Without recursion depth limits, this results in infinite loops and system exhaustion.

### Parameter Collision
When a nested template requires a parameter with the same name as the parent template but expects a different data type or format.

## Related Topics
*   **050 Workflow Orchestration:** The broader system that manages the execution of flows.
*   **070 State Management:** How data is persisted between steps in a flow instance.
*   **080 Error Handling and Retries:** Standardized approaches to managing transient and permanent failures.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |