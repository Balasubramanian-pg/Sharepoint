# 076 Formula Bar and Toolbar

Canonical documentation for 076 Formula Bar and Toolbar. This document defines concepts, terminology, and standard usage.

## Purpose
The Formula Bar and Toolbar serve as the primary interface layer between the user and the underlying data engine. They address the problem of high-density data manipulation by providing a dedicated environment for viewing, entering, and editing complex expressions (Formula Bar) and a centralized location for executing frequent commands and formatting (Toolbar). 

This separation of concerns ensures that the primary canvas remains focused on data visualization, while the Formula Bar and Toolbar provide the precision tools necessary for data logic and presentation management.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Functional requirements for expression input and display.
* Logical relationship between the active selection and the interface elements.
* Structural organization of command sets within a toolbar.
* Synchronization mechanisms between the data grid and the input interface.

**Out of scope:**
* Specific UI/UX styling (e.g., button colors, iconography sets).
* Specific programming languages or calculation engine syntax.
* Vendor-specific keyboard shortcuts.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Formula Bar** | A dedicated input field used to view and edit the content or logic of a selected cell or object. |
| **Toolbar** | A graphical control element containing buttons, icons, or menus that provide quick access to frequently used functions. |
| **Active Selection** | The specific data point, cell, or object currently targeted by the user's focus. |
| **Expression** | The raw string of characters (logic, references, or literals) processed by the underlying engine. |
| **Contextual Control** | A toolbar element that appears or becomes active only when a specific type of data or object is selected. |
| **Synchronization** | The real-time alignment between the value displayed in the data canvas and the value displayed in the Formula Bar. |

## Core Concepts

### 1. Input-Display Duality
The Formula Bar exists to resolve the discrepancy between a "calculated result" (what is seen in the cell) and the "underlying logic" (what is seen in the bar). It provides a high-visibility area for long-form text that would otherwise be truncated in a standard data grid.

### 2. Command Centralization
The Toolbar acts as a discovery layer for the system's capabilities. It categorizes functions into logical groupings (e.g., formatting, data operations, insertion) to reduce cognitive load and minimize the number of steps required to execute common tasks.

### 3. State Awareness
Both the Formula Bar and Toolbar must be "state-aware." This means their contents and enabled/disabled states must reflect the properties and constraints of the current selection. If a selection is read-only, the Formula Bar must reflect this state; if a selection cannot be bolded, the "Bold" button in the Toolbar must be disabled.

## Standard Model

### The Formula Bar Model
The standard Formula Bar consists of three primary components:
1.  **Name/Address Box:** Displays the identifier of the current selection.
2.  **Action Triggers:** Buttons to "Commit" (Enter) or "Discard" (Cancel) the current edit.
3.  **Input Area:** An expandable text field supporting multi-line input and syntax highlighting.

### The Toolbar Model
The standard Toolbar follows a hierarchical or grouped structure:
*   **Persistent Actions:** Global commands like Save, Undo, and Redo.
*   **Formatting Controls:** Tools for modifying the visual representation of data.
*   **Functional Controls:** Tools for data manipulation, such as sorting, filtering, or inserting functions.
*   **Overflow Management:** A mechanism to handle controls that exceed the available horizontal space.

## Common Patterns

### Contextual Switching
The Toolbar dynamically updates based on the selection type. For example, selecting a chart might replace "Text Formatting" tools with "Chart Design" tools.

### Formula Expansion
The Formula Bar allows for vertical expansion to accommodate complex, nested logic, preventing the user from losing track of the expression structure.

### Live Preview
Changes initiated in the Toolbar (such as font size or color) are reflected in the active selection in real-time before the user commits the action.

## Anti-Patterns

### Modal Interference
Requiring a user to close a separate dialog box before they can interact with the Formula Bar or Toolbar, breaking the flow of data entry.

### Desynchronization
Allowing the Formula Bar to display a value that does not match the active selection, leading to data corruption or user confusion.

### Overcrowding (The "Kitchen Sink")
Placing every possible command in the primary Toolbar without categorization or prioritization, which increases search time and reduces usability.

### Hidden State
Failing to indicate *why* a Toolbar button is disabled, leaving the user without a path to enable the desired functionality.

## Edge Cases

### Multi-Selection Discrepancy
When multiple cells with different formulas or values are selected, the Formula Bar must follow a defined protocol (e.g., showing the formula of the "primary" cell or remaining blank) to avoid ambiguity.

### Circular References
The Formula Bar's behavior when a user inputs a formula that references its own cell, requiring specific error handling and visual cues.

### Disconnected Environments
Handling input in the Formula Bar when the connection to the calculation engine is latent or lost, necessitating local caching or "Pending" states.

## Related Topics
*   **012 Data Grid Interaction:** The primary canvas that interacts with the Formula Bar.
*   **045 Expression Syntax:** The logic and grammar used within the Formula Bar.
*   **089 State Management:** The underlying architecture that synchronizes the UI with the data model.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |