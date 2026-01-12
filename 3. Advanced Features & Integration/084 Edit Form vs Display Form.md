# 084 Edit Form vs Display Form

Canonical documentation for 084 Edit Form vs Display Form. This document defines concepts, terminology, and standard usage.

## Purpose
The distinction between Edit and Display forms exists to manage the lifecycle of data interaction within a user interface. This topic addresses the fundamental separation between **data consumption** (reading) and **data manipulation** (writing). By separating these modes, systems can optimize for readability, prevent accidental data corruption, enforce security constraints, and provide clear user intent.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Functional requirements for viewing vs. modifying data.
* State management transitions between read and write modes.
* Visual affordances and user intent signals.
* Data integrity and validation logic placement.

**Out of scope:**
* Specific UI framework syntax (e.g., React, Angular, PowerApps).
* Database schema design.
* Network protocol specifications for data transmission.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Display Form** | A read-only representation of a data record optimized for legibility and information density. |
| **Edit Form** | An interactive interface containing input controls designed to capture and validate user-provided data changes. |
| **Affordance** | A visual cue that implies the function or interactability of an element (e.g., a text box implies input). |
| **State Transition** | The process of moving a user interface from a read-only state to an editable state. |
| **Data Binding** | The mechanism that connects the UI elements of a form to the underlying data source. |
| **Validation** | The process of ensuring that data entered into an Edit Form meets defined business rules before submission. |

## Core Concepts

### 1. Intent-Based Interaction
The primary differentiator is user intent. A Display Form assumes the user wishes to consume information without the risk of altering it. An Edit Form assumes the user has a specific objective to update, correct, or append information.

### 2. Information Density vs. Input Utility
*   **Display Forms** prioritize white space, typography, and layout to make information easy to scan. They often hide empty fields to reduce noise.
*   **Edit Forms** prioritize input controls (text boxes, dropdowns, pickers). They must show all relevant fields—even empty ones—to allow for data entry.

### 3. Data Integrity and Safety
Display forms act as a safety buffer. By requiring an explicit transition to an Edit Form, the system prevents "pocket-dial" style data corruption where a user accidentally modifies a field while scrolling or navigating.

## Standard Model

The standard model for managing the relationship between these two forms follows a cyclical lifecycle:

1.  **Entry (Display):** The user opens a record. The system renders a Display Form.
2.  **Trigger (Transition):** The user selects an "Edit" action.
3.  **Modification (Edit):** The system replaces the Display Form with an Edit Form. Input controls are populated with existing values.
4.  **Validation (Evaluation):** As the user makes changes, the system evaluates the data against business rules.
5.  **Commit/Revert (Resolution):**
    *   **Save:** Data is sent to the data store; the system returns to the Display Form with updated values.
    *   **Cancel:** Changes are discarded; the system returns to the Display Form with original values.

## Common Patterns

### Switch-in-Place
The UI elements transform from static text to input fields within the same layout. This maintains the user's spatial context but can be technically complex to implement regarding layout shifting.

### Dedicated View/Edit Routes
The system uses distinct URLs or views for "View" and "Edit." This is common in web applications to allow for deep-linking to a specific mode and to simplify permission logic.

### Modal/Overlay Editing
The Display Form remains in the background while the Edit Form appears in a layer above it (a modal or drawer). This is effective for quick updates to specific subsets of data.

### Inline/Cell Editing
Common in data grids or tables, where clicking a specific value transforms only that discrete element into an input field.

## Anti-Patterns

*   **The Permanent Edit:** Keeping a form in "Edit" mode at all times. This increases the risk of accidental data loss and often results in a cluttered UI filled with unnecessary input boxes.
*   **The Hidden Cancel:** Providing a "Save" button without a clear "Cancel" or "Discard" option, forcing the user to refresh the page or manually undo changes.
*   **Inconsistent Layouts:** Drastically changing the position of fields when switching from Display to Edit. This forces the user to re-orient themselves visually.
*   **Silent Failures:** Allowing a user to "Save" an Edit Form when validation has failed, or failing to provide feedback on why a save was rejected.

## Edge Cases

*   **Partial Permissions:** A user may have permission to view all fields (Display Form) but only permission to edit a subset of those fields. The Edit Form must gracefully handle "Read-Only" fields within an otherwise editable context.
*   **Calculated Fields:** Fields that are derived from other data (e.g., "Total Price"). These appear in Display Forms but are typically excluded or disabled in Edit Forms to prevent manual overrides of logic.
*   **Concurrency:** Two users viewing a Display Form simultaneously, while one transitions to an Edit Form. The system must handle the "Lost Update" problem when the second user attempts to edit.
*   **Multi-Page Forms:** When a record is too large for one screen, the transition between Display and Edit must maintain the user's current "page" or "section" context.

## Related Topics
*   **012 CRUD Operations:** The foundational data actions (Create, Read, Update, Delete).
*   **045 Form Validation:** The logic governing data entry constraints.
*   **102 User Permissions and Roles:** Determining who can access Edit vs. Display modes.
*   **028 Optimistic vs. Pessimistic Locking:** Strategies for handling concurrent edits.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |