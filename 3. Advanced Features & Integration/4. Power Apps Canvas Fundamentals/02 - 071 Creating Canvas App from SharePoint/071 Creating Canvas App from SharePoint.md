# 071 Creating Canvas App from SharePoint

Canonical documentation for 071 Creating Canvas App from SharePoint. This document defines concepts, terminology, and standard usage.

## Purpose
The integration between collaborative data repositories and low-code application environments exists to bridge the gap between raw data storage and functional user interfaces. By generating a Canvas App directly from a SharePoint schema, organizations can rapidly deploy mobile-optimized, task-specific tools that facilitate data entry, visualization, and management without requiring traditional software development lifecycles. This process addresses the need for "Data Democratization," allowing non-developers to create structured interfaces for existing business data.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative regarding the architectural relationship between the data source and the application layer.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The architectural relationship between SharePoint lists/libraries and the Canvas App environment.
* The automated generation of application schemas based on data source metadata.
* Core data operations (Create, Read, Update, Delete) within the generated context.
* Theoretical boundaries of data delegation and synchronization.

**Out of scope:**
* Specific step-by-step UI "click-through" tutorials for the Power Platform interface.
* Advanced custom coding (e.g., PCF components) within the app.
* Third-party connectors or non-SharePoint data sources.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Canvas App** | A low-code application type that allows for complete control over the user interface layout and design, typically bound to one or more data sources. |
| **Data Source** | The underlying repository (in this case, SharePoint) where information is stored and retrieved. |
| **Schema-Driven UI** | An application interface automatically generated based on the columns, data types, and constraints defined in the data source. |
| **Delegation** | The process by which the application offloads data processing (filtering, sorting) to the data source rather than processing it locally within the app. |
| **Connector** | The architectural bridge that facilitates communication and authentication between the application layer and the SharePoint API. |
| **CRUD Operations** | The four basic functions of persistent storage: Create, Read, Update, and Delete. |

## Core Concepts
### 1. Metadata Mapping
When an app is created from SharePoint, the application environment inspects the SharePoint List schema. It maps SharePoint column types (e.g., Choice, Person, Date, Lookup) to corresponding application controls (e.g., Dropdowns, Combo Boxes, Date Pickers).

### 2. The Three-Screen Architecture
The standard output of a SharePoint-to-App generation is a three-screen functional model:
*   **Browse Screen:** A gallery view for searching and filtering records.
*   **Detail Screen:** A read-only view of a specific record's attributes.
*   **Edit/Create Screen:** A form-based interface for modifying existing records or submitting new ones.

### 3. Data Binding and Context
The application maintains a live connection to the SharePoint list. Changes made in the app are pushed to the list via the connector, and the app's state is refreshed to reflect the current state of the data source.

## Standard Model
The recommended model for creating a Canvas App from SharePoint follows the **Direct Generation Pattern**:

1.  **Schema Definition:** The SharePoint List is finalized with appropriate column types and indexing.
2.  **Automated Scaffolding:** The application environment generates the initial screens based on the list's metadata.
3.  **Refinement:** The developer adjusts the UI/UX, adds logic (formulas), and configures delegation settings to ensure performance.
4.  **Deployment:** The app is shared with users who have existing permissions to the underlying SharePoint data.

## Common Patterns
*   **The Mobile Front-End:** Using SharePoint as a backend for field workers to submit data via a mobile-optimized Canvas App.
*   **The Approval Wrapper:** Creating an app that sits on top of a SharePoint list to facilitate a specific business process or approval workflow.
*   **The Dashboard Overlay:** Using the Browse screen as a lightweight reporting tool to visualize list status at a glance.

## Anti-Patterns
*   **Relational Complexity in SharePoint:** Attempting to build complex, multi-table relational databases in SharePoint and expecting the Canvas App to manage referential integrity automatically.
*   **Ignoring Delegation Limits:** Building apps against large SharePoint lists (5,000+ items) without using delegable queries, leading to incomplete data sets in the app.
*   **Over-Customization of Generated Forms:** Breaking the underlying data-card structure of the generated forms, which can lead to data submission failures or maintenance difficulties.
*   **Permission Mismatch:** Assuming the app grants access to data; users must have appropriate permissions on the SharePoint list itself, independent of the app.

## Edge Cases
*   **Complex Column Types:** Columns like "Lookup" or "Person and Group" require specific handling as they return records/objects rather than simple strings.
*   **Offline Mode:** SharePoint-backed apps do not natively support offline data synchronization without significant manual configuration of local collections and caching logic.
*   **Large Attachments:** Handling multiple large file attachments within the generated form can lead to performance degradation or timeout issues.
*   **Version History:** While SharePoint tracks versioning, the standard Canvas App interface does not expose version history to the end-user without custom logic.

## Related Topics
*   **072 Data Delegation in Canvas Apps:** Deep dive into query limits and performance.
*   **085 SharePoint List Schema Design:** Best practices for structuring data sources.
*   **102 Power Automate Integration:** Triggering workflows based on app-driven data changes.
*   **201 App Security and Governance:** Managing permissions between the UI and the Data Source.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |