# 067 Power Apps Types Overview

Canonical documentation for 067 Power Apps Types Overview. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of defining Power Apps types is to categorize application development methodologies based on their architectural starting points, user experience (UX) requirements, and data integration strategies. By distinguishing between these types, organizations can align technical resources with specific business outcomes, ensuring that the chosen framework supports the necessary scale, flexibility, and interface precision.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative, focusing on the architectural archetypes rather than specific versioning or licensing tiers.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   Classification of application archetypes (Canvas, Model-driven, and External-facing).
*   Architectural boundaries between UI-first and Data-first design.
*   Theoretical application of logic and state management within each type.

**Out of scope:**
*   Specific vendor licensing models or pricing.
*   Step-by-step deployment tutorials.
*   Third-party integration specificities.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Canvas Archetype** | A UI-first approach where the developer has total control over the visual layout and pixel-perfect placement of elements. |
| **Model-driven Archetype** | A data-first approach where the application structure is automatically generated based on the underlying data schema and metadata. |
| **External-facing (Pages)** | A specialized type designed for anonymous or authenticated external users, focusing on content management and public data interaction. |
| **Metadata-driven Architecture** | A system where the application's behavior and UI are defined by data descriptions rather than hard-coded visual logic. |
| **Declarative Logic** | Expressing the logic of a computation without describing its control flow (e.g., "What to do" rather than "How to do it"). |

## Core Concepts
The Power Apps ecosystem is built upon three fundamental pillars of application design:

1.  **UI-First Design (Canvas):** Focuses on the user's journey and specific task execution. It allows for high-fidelity branding and custom interactions, often used for mobile-centric or single-purpose utilities.
2.  **Data-First Design (Model-driven):** Focuses on the business process and data integrity. The UI is a byproduct of the data model, ensuring consistency across complex enterprise systems.
3.  **Extensibility:** The ability to augment standard types with custom code (e.g., components or plugins) when the native declarative capabilities reach their functional limits.

## Standard Model
The standard model for Power Apps categorization follows a "Right Tool for the Job" philosophy:

*   **Task-Specific Apps (Canvas):** Best suited for frontline workers or specific, narrow workflows (e.g., expense reporting, equipment inspection).
*   **Process-Heavy Apps (Model-driven):** Best suited for back-office operations, relationship management (CRM), and complex data manipulation where a standardized interface is an advantage.
*   **Public Portals (External):** Best suited for customer-facing interactions, such as support tickets, community forums, or public registrations.

## Common Patterns
*   **The Hybrid Pattern:** Embedding a Canvas app within a Model-driven form to provide a custom UI for a specific step in a larger, data-heavy process.
*   **The Mobile-First Pattern:** Utilizing the Canvas type to leverage device hardware (camera, GPS, sensors) for field-based data entry.
*   **The Admin-Backplane Pattern:** Using a Model-driven app as the administrative interface to manage the data consumed by multiple Canvas apps.

## Anti-Patterns
*   **The "Pixel-Perfect" Model-driven App:** Attempting to force a Model-driven app to match a specific brand guideline through excessive CSS or custom scripts, rather than using a Canvas app.
*   **The "Monolithic" Canvas App:** Building a massive, multi-screen Canvas app that attempts to replicate an entire ERP system, leading to performance degradation and maintenance complexity.
*   **Data Siloing:** Choosing an app type based solely on UI preference while ignoring the long-term benefits of a centralized, shared data schema.

## Edge Cases
*   **Offline-First Requirements:** While both primary types offer offline capabilities, the implementation logic differs significantly. Canvas apps require manual cache management, whereas Model-driven apps rely on automated synchronization profiles.
*   **High-Volume Public Traffic:** Standard internal app types are not designed for thousands of concurrent anonymous users; this scenario necessitates the External-facing (Pages) archetype.
*   **Complex Computational Logic:** Scenarios requiring heavy real-time calculations may exceed the capabilities of declarative formulas, requiring a shift toward server-side logic or custom components.

## Related Topics
*   **Dataverse Architecture:** The underlying data platform for Model-driven apps.
*   **Power Automate Integration:** The primary engine for cross-app logic and workflow automation.
*   **Component Framework (PCF):** The standard for building custom UI elements across all app types.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |