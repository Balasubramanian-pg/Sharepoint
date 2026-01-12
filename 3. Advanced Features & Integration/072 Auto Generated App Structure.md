# 072 Auto Generated App Structure

Canonical documentation for 072 Auto Generated App Structure. This document defines concepts, terminology, and standard usage.

## Purpose
The 072 Auto Generated App Structure exists to solve the problem of architectural fragmentation and "boilerplate fatigue" within software ecosystems. By providing a standardized, machine-readable blueprint for application layouts, it ensures that all projects—regardless of their specific business logic—adhere to a predictable organizational hierarchy. 

This structure addresses the need for:
*   **Consistency:** Ensuring that developers can navigate any codebase within the ecosystem without a learning curve.
*   **Scalability:** Providing a foundation that supports growth from a simple prototype to a complex production system.
*   **Automation:** Enabling tooling to programmatically interact with, update, and audit the codebase based on known file locations and naming conventions.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   The logical organization of directories and files.
*   The relationship between core application layers (e.g., entry points, configuration, and business logic).
*   The metadata required to define and regenerate the structure.
*   The theoretical boundaries between generated code and user-defined code.

**Out of scope:**
*   Specific programming language syntax or framework-specific implementations (e.g., React vs. Django).
*   Specific CLI tools or scaffolding engines used to perform the generation.
*   Deployment or CI/CD pipeline configurations, except where they intersect with the file structure.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Scaffolding** | The process of generating the initial file and directory structure based on a predefined template. |
| **Boilerplate** | Standardized code blocks or files that are required for the application to function but do not contain unique business logic. |
| **Manifest** | A machine-readable file (e.g., JSON or YAML) that describes the intended state and version of the generated structure. |
| **Hydration** | The process of injecting project-specific variables (e.g., app name, author) into the generated templates. |
| **Protected Region** | A specific area within a generated file where user-written code is preserved during regeneration. |
| **Shadowing** | The act of overriding a generated file with a custom version, effectively taking ownership away from the generator. |

## Core Concepts

### 1. Convention over Configuration
The 072 structure prioritizes sensible defaults. By assuming specific locations for assets, tests, and source code, the need for complex configuration files is minimized.

### 2. Separation of Concerns (Structural)
The structure enforces a physical separation between:
*   **Entry Points:** Where the application starts.
*   **Core Logic:** The "brain" of the application, isolated from external interfaces.
*   **Infrastructure:** External dependencies, database schemas, and API clients.
*   **Configuration:** Environment-specific settings.

### 3. Idempotency in Generation
A core tenet of the 072 standard is that running the generator multiple times on the same directory should produce a consistent result without destroying user-defined logic, provided the user adheres to the defined "Protected Regions."

## Standard Model
The standard model for a 072-compliant application follows a hierarchical tree designed for modularity.

```text
[root]
├── .072-metadata/      # Metadata and manifest for the generator
├── config/             # Environment and application configuration
├── docs/               # Technical documentation and specifications
├── src/                # Source code
│   ├── api/            # External interface definitions
│   ├── core/           # Pure business logic (domain)
│   ├── data/           # Data access and persistence layers
│   └── shared/         # Utilities and common types
├── tests/              # Test suites (unit, integration, e2e)
└── README.md           # Project overview
```

## Common Patterns

### The "Core-Shell" Pattern
The generated structure often places the "Core" (business logic) in a protected directory while the "Shell" (framework-specific adapters, HTTP handlers) is more susceptible to automated updates or framework migrations.

### The Manifest-Driven Update
In this pattern, the `.072-metadata/` directory contains a versioned manifest. When the central 072 standard is updated, a migration tool compares the local manifest with the new standard and applies structural changes (moving files, updating boilerplate) automatically.

## Anti-Patterns

*   **Logic Leakage:** Placing business logic inside the `config/` or `api/` directories, making it difficult to swap interfaces or environments.
*   **Manual Structural Modification:** Renaming generated directories without updating the manifest, which breaks the automation's ability to track the project state.
*   **Over-Scaffolding:** Generating deeply nested directories for simple projects, leading to unnecessary complexity and "folder-diving."
*   **Hardcoding Metadata:** Embedding project-specific strings directly into templates instead of using the hydration process.

## Edge Cases

*   **Polyglot Projects:** When a single repository contains multiple languages, the 072 structure should be applied to each sub-module independently, with a "Master Manifest" at the root.
*   **Legacy Integration:** When applying the 072 structure to an existing, non-compliant codebase. This usually requires a "Bridge" phase where the old structure coexists with the new one until migration is complete.
*   **Partial Generation:** Scenarios where only specific layers (e.g., just the API layer) are managed by the 072 standard, while the rest of the app is manually structured.

## Related Topics
*   **071 Architectural Standards:** The high-level principles that inform the 072 structure.
*   **073 Metadata Schema:** The technical specification for the manifest files used in 072.
*   **Domain-Driven Design (DDD):** The architectural philosophy that heavily influences the `src/core` organization.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |