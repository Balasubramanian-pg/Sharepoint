# 095 Customizing Auto Generated Apps

Canonical documentation for 095 Customizing Auto Generated Apps. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of customizing auto-generated applications is to bridge the gap between generic, standardized output produced by automated scaffolding tools and the specific, nuanced requirements of a unique business domain. Auto-generation provides a baseline of functionality—typically CRUD (Create, Read, Update, Delete) operations and basic navigation—but rarely accounts for complex business logic, specialized user experiences, or proprietary integration patterns. This topic addresses the methodologies for extending these systems while maintaining a balance between development speed and long-term maintainability.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Architectural strategies for modifying generated code.
* Mechanisms for preserving customizations during regeneration cycles.
* Theoretical boundaries between "Generated Space" and "Custom Space."
* Metadata-driven vs. Code-driven customization models.

**Out of scope:**
* Specific vendor implementations (e.g., specific low-code platforms or CLI scaffolding tools).
* Programming language-specific syntax.
* UI/UX design principles unrelated to the generation process.

## Definitions
| Term | Definition |
|------|------------|
| **Scaffolding** | The process of generating a skeletal structure for an application based on a predefined template or schema. |
| **Ejection** | The permanent act of disconnecting an auto-generated app from its source generator, moving all logic to manual maintenance. |
| **Generation Gap** | A design pattern that separates generated code from handwritten code using inheritance or partial classes to prevent overwriting. |
| **Metadata** | The data (schemas, JSON definitions, or DSLs) that informs the generator how to build the application. |
| **Shadowing** | A technique where a custom file is placed in a specific directory to override a default generated file of the same name. |
| **Hook/Extension Point** | A predefined location within the generated code or lifecycle where custom logic can be injected without altering the core generator output. |

## Core Concepts

### The Customization Paradox
The primary challenge in auto-generated apps is the "Customization Paradox": the more an application is customized, the harder it becomes to leverage the generator for future updates (such as security patches or schema changes). Effective customization strategies aim to minimize this friction.

### Declarative vs. Imperative Customization
*   **Declarative:** Customization achieved by modifying the input metadata (e.g., changing a JSON schema to add a validation rule). The generator then interprets this and produces the desired output.
*   **Imperative:** Customization achieved by writing manual code that interacts with or overrides the generated output.

### The Source of Truth
In auto-generated systems, the "Source of Truth" often shifts. Initially, it is the schema or model. Once customization begins, the source of truth may become fragmented between the model and the manual overrides. Canonical systems strive to keep the model as the primary source of truth for as long as possible.

## Standard Model

The standard model for customizing auto-generated apps follows a tiered approach to ensure stability:

1.  **Metadata Layer:** Adjusting the generator's input (schemas, configuration files) to influence the output.
2.  **Extension Layer:** Utilizing provided hooks, events, or "partial" structures to add logic in designated areas.
3.  **Composition Layer:** Wrapping generated components or services within custom containers rather than modifying the generated code directly.
4.  **Override Layer:** Replacing generated elements entirely when the provided extension points are insufficient.

## Common Patterns

### The Generation Gap Pattern
This pattern involves the generator producing a "Base" class or file (e.g., `UserBase`) which is never edited by the developer. The developer works in a derived class (e.g., `User`) that inherits from the base. When the generator runs again, it only overwrites the "Base" file, preserving the developer's logic in the derived class.

### Aspect-Oriented Extension
Custom logic is injected into the generated application's lifecycle (e.g., "Pre-Save" or "Post-Render" hooks). This allows the developer to add validation, logging, or transformations without touching the core generated boilerplate.

### Component Shadowing
Common in modern web frameworks, this involves creating a file in a specific project directory that matches the path of a generated component. The build system prioritizes the custom file over the generated one.

## Anti-Patterns

### Hard-Coding in Generated Files
Directly editing files marked with "DO NOT EDIT" or "AUTO-GENERATED." This leads to "Update Trauma," where customizations are lost during the next generation cycle or the developer is forced to manually merge complex diffs.

### Over-Configuration
Attempting to force a generator to handle every possible edge case through metadata. This often results in a configuration schema so complex that it becomes more difficult to maintain than manual code.

### The "Big Eject"
Ejecting from a generator too early in the project lifecycle. This loses the benefits of automated updates and schema synchronization before the application has reached a stable state.

## Edge Cases

### Schema Drift
When the underlying data source (e.g., a database) changes, but the generator is not re-run, or the customizations rely on fields that no longer exist. This creates a mismatch between the "Custom Space" and the "Generated Space."

### Circular Dependencies
Occurs when a custom extension requires a generated service, which in turn has been overridden to require the custom extension. This is common in complex dependency injection environments.

### Version Mismatch
When the generator is updated to a new version that produces a different code structure, breaking existing "Shadowing" or "Inheritance" patterns established by the developer.

## Related Topics
*   **042 Schema-Driven Development:** The practice of using data definitions as the primary driver for application state.
*   **112 Low-Code Extensibility:** Specific strategies for extending proprietary visual development platforms.
*   **088 Model-View-Controller (MVC):** The architectural pattern most commonly used as the target for auto-generation.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |