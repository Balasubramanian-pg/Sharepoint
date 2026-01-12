# 068 Canvas Apps Overview

Canonical documentation for 068 Canvas Apps Overview. This document defines concepts, terminology, and standard usage.

## Purpose
The Canvas App paradigm exists to provide a high-productivity, visual-first environment for building custom business applications. It addresses the "app gap"—the space between generic off-the-shelf software and expensive, time-consuming custom-coded solutions. 

By prioritizing user interface (UI) flexibility and declarative logic, Canvas Apps allow creators to build tailored experiences that match specific business processes without requiring traditional imperative programming. This approach democratizes application development, enabling both professional developers and business subject matter experts to iterate rapidly on functional prototypes and production-ready tools.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative, focusing on the architectural and conceptual framework of canvas-based application design.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   **UI-First Design Philosophy:** The principles of "pixel-perfect" layout and drag-and-drop composition.
*   **Declarative Logic:** The use of functional expressions to define application behavior.
*   **Data Integration:** The conceptual framework for connecting UI elements to heterogeneous data sources.
*   **Lifecycle Management:** The high-level stages of canvas app creation, from design to deployment.

**Out of scope:**
*   **Specific Vendor Implementations:** Detailed tutorials for Microsoft Power Apps, AppSheet, or Mendix.
*   **Infrastructure Hosting:** Specific cloud provider configurations or on-premises server management.
*   **Specific Programming Languages:** Syntax guides for specific proprietary formula languages.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
| :--- | :--- |
| **Canvas** | The visual workspace where UI elements are positioned and styled using a coordinate system. |
| **Control** | An individual UI component (e.g., button, gallery, input) that possesses properties and triggers events. |
| **Data Connector** | A bridge that allows the application to communicate with external services or databases without writing custom API calls. |
| **Declarative Logic** | A programming paradigm where the developer defines *what* the app should do (via formulas) rather than *how* to do it (via step-by-step scripts). |
| **Reactive UI** | A design state where the interface automatically updates in response to changes in underlying data or variables. |
| **Screen** | A distinct container within the app representing a single view or page in the user journey. |
| **Delegation** | The process of offloading data processing (filtering, sorting) to the data source rather than performing it within the app's local memory. |

## Core Concepts

### 1. The Visual-First Paradigm
Unlike traditional development where the UI is often a byproduct of the underlying code, Canvas Apps start with the interface. The "Canvas" serves as a literal drawing board where the layout is defined by X and Y coordinates, allowing for precise control over the user experience.

### 2. Functional Expression Logic
Logic in Canvas Apps is typically handled through expressions similar to spreadsheet formulas. These expressions are used to set properties (e.g., `Color`, `Visible`, `Items`) based on data or user input. This reduces the need for complex state management found in traditional software development.

### 3. Data-Agnostic Connectivity
A core tenet of the Canvas App model is the separation of the UI from the data layer. Through a standardized connector architecture, the app can interact with relational databases, file systems, or web APIs using a unified set of commands, regardless of the backend technology.

### 4. Statelessness and State Management
While Canvas Apps are primarily reactive, they utilize "Context" and "Global" variables to manage temporary state. However, the standard model encourages minimizing local state in favor of direct data-source interaction to ensure consistency and performance.

## Standard Model
The standard model for Canvas App development follows a non-linear, iterative lifecycle:

1.  **Requirement Mapping:** Identifying the specific task or "micro-service" the app will perform.
2.  **Data Schema Definition:** Identifying or creating the data structures required to support the UI.
3.  **UI Composition:** Placing controls on the canvas to build the user journey.
4.  **Logic Binding:** Writing expressions to connect UI properties to data and user actions.
5.  **Optimization (Delegation):** Ensuring that data-heavy operations are handled by the server to maintain client-side performance.
6.  **Validation and Publishing:** Testing the app across different form factors (mobile, tablet, web) before deployment.

## Common Patterns

*   **The Task-Specific App:** A "micro-app" designed to do one thing exceptionally well (e.g., expense reporting, inventory check, or room booking).
*   **The Dashboard/Aggregator:** An app that pulls data from multiple disparate sources into a single, read-only visual interface for decision-making.
*   **The Form-Over-Data:** A pattern focused on capturing user input and writing it back to a structured data source with validation logic.
*   **The Master-Detail:** A navigation pattern where a user selects an item from a list (Gallery) to view or edit its specific details on a separate screen or panel.

## Anti-Patterns

*   **The Monolith:** Attempting to build a massive, all-encompassing ERP system within a single Canvas App, leading to performance degradation and maintenance complexity.
*   **Deep Nesting:** Over-nesting containers or galleries, which significantly impacts rendering speed and accessibility.
*   **Client-Side Processing:** Attempting to filter or calculate thousands of records within the app's memory instead of using server-side delegation.
*   **Hard-Coding:** Embedding business logic or configuration values directly into formulas instead of using a configuration table or environment variables.

## Edge Cases

*   **Offline Functionality:** Managing data synchronization and conflict resolution when an app loses connectivity. This requires specialized caching logic and local storage management.
*   **High-Latency Environments:** Designing UI cues (loading spinners, optimistic updates) to handle slow data connectors without frustrating the user.
*   **Complex Recursion:** Since most canvas expression languages are not designed for deep recursion, complex tree-structures or recursive calculations often require creative workarounds or external services.
*   **Large Media Handling:** Managing high-resolution images or large video files within the app's memory constraints.

## Related Topics

*   **069 Model-Driven Apps:** A comparison of data-first vs. UI-first application design.
*   **Data Connector Architecture:** Deep dive into how apps communicate with external APIs.
*   **Application Lifecycle Management (ALM):** Best practices for versioning and deploying apps across environments.
*   **User Experience (UX) Design for Low-Code:** Principles of designing accessible and intuitive interfaces in a constrained environment.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |