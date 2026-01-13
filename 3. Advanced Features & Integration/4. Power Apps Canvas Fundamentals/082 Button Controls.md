# 082 Button Controls

Canonical documentation for 082 Button Controls. This document defines concepts, terminology, and standard usage.

## Purpose
Button Controls serve as the primary interface mechanism for triggering immediate, discrete actions within a system. They bridge the gap between user intent and system execution by providing a clear, interactive affordance that signals "actionability." The fundamental purpose of a button is to execute a command, submit data, or change the state of an application in a way that is predictable and deterministic.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Functional definitions and behavioral logic of button elements.
* Interaction states and lifecycle transitions.
* Hierarchical classification and intent-based categorization.
* Accessibility and semantic requirements for action-triggering controls.

**Out of scope:**
* Specific vendor implementations (e.g., HTML `<button>`, Android `Widget.Button`, iOS `UIButton`).
* Visual styling, branding, or specific aesthetic design tokens.
* Navigation-only elements (Links/Anchors) that do not change system state.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Affordance** | The visual or sensory cues that imply an object is interactive and clickable. |
| **Intent** | The specific outcome or action a user expects to occur upon activation. |
| **State** | The current status of the control (e.g., Idle, Hover, Active, Disabled, Loading). |
| **Hierarchy** | The relative importance of a button compared to other controls in the same context. |
| **Idempotency** | The property of certain actions where performing them multiple times has the same effect as performing them once. |
| **Label** | The textual or symbolic content that communicates the button's purpose. |

## Core Concepts

### 1. Actionability
A button must clearly communicate that it is interactive. This is achieved through a combination of shape, contrast, and placement. Unlike static text or decorative elements, a button implies a "contract" with the user: activation will result in a specific, non-passive change.

### 2. Determinism
Button controls should be deterministic. For a given state of the system, clicking a button should produce a predictable result. Ambiguity in button function leads to user friction and potential data loss.

### 3. Feedback Loops
Every button interaction requires a feedback loop. This includes:
* **Input Acknowledgement:** Visual or haptic confirmation that the press was registered.
* **Process Indication:** If the action is asynchronous, the button must reflect that work is in progress.
* **Outcome Confirmation:** Clear indication that the action succeeded or failed.

## Standard Model

The Standard Model for 082 Button Controls follows a state-machine logic:

1.  **Idle State:** The default state where the button is available for interaction.
2.  **Focus/Hover State:** The state when a user targets the control via pointer or keyboard, signaling readiness.
3.  **Active/Pressed State:** The momentary state during the physical or virtual "click."
4.  **Processing State (Optional):** Used for asynchronous actions where the button remains visible but non-interactive while the system executes the command.
5.  **Disabled State:** The control is visible but non-functional, typically because certain prerequisites in the system state have not been met.

### Hierarchy of Intent
Buttons are categorized by their importance within a view:
*   **Primary:** The single most important action (e.g., "Submit," "Save").
*   **Secondary:** Alternative actions that are less common (e.g., "Cancel," "Back").
*   **Tertiary/Ghost:** Low-emphasis actions, often used for auxiliary functions to reduce visual clutter.

## Common Patterns

### The "Call to Action" (CTA)
A high-contrast button designed to draw the user toward the primary objective of a page or workflow.

### Destructive Actions
Buttons that trigger irreversible data loss (e.g., "Delete," "Wipe"). These typically require distinct visual treatment (often red) and may be paired with a confirmation step.

### Toggle Buttons
A specialized button that maintains a binary state (On/Off). Unlike a standard button, its primary purpose is state-switching rather than command execution.

### Split Buttons
A hybrid control where the main body executes a default action, while a secondary "chevron" or dropdown area provides a list of related variations of that action.

## Anti-Patterns

*   **Using Buttons for Navigation:** Using a button control to link to a different URL or page without changing system state. This breaks the semantic expectation of "Action vs. Navigation."
*   **Label Ambiguity:** Using generic labels like "OK" or "Click Here" instead of verb-based labels like "Save Changes" or "Download Report."
*   **Disabled Tooltips:** Disabling a button without providing a reason or a path to enable it, leaving the user trapped.
*   **Redundant Primaries:** Placing two "Primary" buttons side-by-side, creating cognitive load and decision paralysis.

## Edge Cases

### Long Labels and Localization
When a button label exceeds the container width due to translation or long strings. The standard model dictates that buttons should either expand to fit text or use ellipsis with a tooltip, though expansion is preferred for accessibility.

### Rapid-Fire Activation
Scenarios where a user clicks a button multiple times in quick succession. The system must handle "debounce" logic to prevent duplicate submissions, especially for non-idempotent actions (e.g., "Place Order").

### Hidden Actions
Buttons that only appear on hover or specific conditions. While they reduce clutter, they fail the "discoverability" requirement for touch-based or keyboard-only interfaces.

## Related Topics
*   **041 Input Validation:** How buttons interact with form data integrity.
*   **102 Feedback Systems:** Detailed mechanics of progress indicators and toasts.
*   **015 Accessibility Standards:** Specifics on ARIA roles and keyboard navigation (Tab/Enter/Space).
*   **099 State Management:** How the global application state dictates the "Disabled" or "Active" status of controls.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |