# 081 Toggle and Slider Controls

Canonical documentation for 081 Toggle and Slider Controls. This document defines concepts, terminology, and standard usage.

## Purpose
The 081 Toggle and Slider Controls exist to facilitate user input for binary state selection and magnitude adjustment within a defined range. These controls bridge the gap between abstract data entry and physical-world metaphors, providing intuitive interfaces for settings management and quantitative tuning. They address the problem of providing immediate, low-friction interaction patterns that minimize cognitive load compared to text-based or multi-step input methods.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Binary State Management:** Logic and behavior of two-state switches (Toggles).
* **Range Selection:** Logic and behavior of continuous and discrete value selection (Sliders).
* **Visual Affordance:** The theoretical requirements for communicating interactivity.
* **Feedback Loops:** The relationship between user action and system response.

**Out of scope:**
* **Specific vendor implementations:** (e.g., Material Design `MDSwitch`, Apple UIKit `UISwitch`).
* **Styling/Theming:** Specific CSS, hex codes, or brand-specific aesthetics.
* **Hardware Controls:** Physical mechanical switches or potentiometers.

## Definitions
| Term | Definition |
|------|------------|
| **Toggle (Switch)** | A digital control used to switch between two mutually exclusive states (e.g., On/Off). |
| **Slider** | A control that allows users to select a value or a range of values along a bar. |
| **Thumb (Handle)** | The interactive element of a slider or toggle that the user moves to change the value. |
| **Track** | The visual path or line along which the thumb moves. |
| **Tick Mark** | A visual indicator on a slider representing a specific, discrete value or increment. |
| **Active State** | The condition in which a toggle is "On" or a slider is currently being manipulated. |
| **Step** | The smallest increment by which a slider value can be increased or decreased. |

## Core Concepts
### Direct Manipulation
Both toggles and sliders rely on the principle of direct manipulation. Users interact with a visual representation of a physical object, providing a sense of control and immediate feedback.

### Immediate Effect
Unlike forms that require a "Submit" action, toggles and sliders are generally expected to apply changes instantaneously. The state change is the action itself.

### State Persistence
A toggle or slider must always reflect the current, persisted state of the system. If a setting is "On," the toggle must remain in the "On" position across sessions until explicitly changed.

### Quantitative vs. Qualitative Input
* **Toggles** handle qualitative binary states (Enabled/Disabled).
* **Sliders** handle quantitative values (Volume 0-100) or relative qualitative values (Low/Medium/High).

## Standard Model
### The Toggle Model
* **Binary Nature:** A toggle must only ever have two states. If a third state (e.g., "Indeterminate") is required, a toggle is the incorrect control.
* **Directionality:** In Left-to-Right (LTR) environments, the "Off" state is positioned to the left, and the "On" state is positioned to the right.
* **Labeling:** Labels should describe the object or action being toggled, not the state itself (e.g., "Bluetooth" rather than "On/Off").

### The Slider Model
* **Range Definition:** Every slider must have a defined Minimum and Maximum.
* **Linearity:** Movement along the track should correspond linearly to the value change unless a logarithmic scale is explicitly defined and labeled.
* **Value Visibility:** For sliders where precision is required, the current numerical value should be displayed dynamically as the thumb moves.

## Common Patterns
* **The Settings Switch:** Used for system preferences (e.g., "Airplane Mode").
* **The Continuous Slider:** Used for values where precision is secondary to "feel" (e.g., Brightness, Volume).
* **The Discrete Slider:** Used when only specific values are allowed (e.g., "Number of Bedrooms: 1, 2, 3, 4+").
* **The Range Slider:** A variation featuring two thumbs to define a minimum and maximum bound within a larger range (e.g., Price Range: $50 - $200).

## Anti-Patterns
* **Toggle as Navigation:** Using a toggle to move a user to a different page or view. Toggles are for settings, not navigation.
* **Delayed Application:** Requiring a "Save" or "Apply" button for a toggle. This contradicts the "Immediate Effect" core concept.
* **Ambiguous Labels:** Using labels like "Enable Feature" inside a toggle that is already labeled "Feature."
* **Invisible Sliders:** Providing a slider without indicating the range or the current value, forcing the user to guess the result.
* **Tiny Hit Targets:** Designing thumbs or tracks that are too small for touch or mouse precision, leading to user frustration.

## Edge Cases
* **High-Latency Environments:** When a toggle controls a cloud-based resource that takes time to activate. The standard model suggests a "loading" or "pending" state visual within the toggle to prevent repeated clicking.
* **Non-Linear Sliders:** Using sliders for exponential scales (e.g., frequency or audio gain). These require clear tick marks to explain the non-linear progression.
* **Extreme Ranges:** Sliders covering massive ranges (e.g., 1 to 1,000,000). These often require a secondary text input field for precision.
* **Accessibility (Keyboard/Screen Reader):** Toggles must be reachable via `Tab` and operable via `Space/Enter`. Sliders must be operable via arrow keys.

## Related Topics
* **042 Input Validation:** How values from sliders are sanitized.
* **015 Visual Feedback Systems:** The theory of communicating state changes to users.
* **102 Form Design Patterns:** How toggles and sliders integrate into larger data-entry structures.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |