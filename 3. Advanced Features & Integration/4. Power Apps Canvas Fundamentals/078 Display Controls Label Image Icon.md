# 078 Display Controls Label Image Icon

Canonical documentation for 078 Display Controls Label Image Icon. This document defines concepts, terminology, and standard usage.

## Purpose
The 078 Display Controls Label Image Icon framework exists to standardize the visual and semantic identification of interactive elements within a user interface. Its primary purpose is to ensure that users can identify, understand, and predict the behavior of a control through the harmonious use of text (Labels), symbolic representations (Icons), and descriptive graphics (Images). 

By defining the relationship between these three elements, this documentation addresses the problem of cognitive load, ambiguity in navigation, and accessibility barriers in complex information systems.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The relationship and hierarchy between labels, images, and icons within a single control.
* Semantic requirements for accessibility and screen readers.
* Spatial positioning and alignment logic.
* Functional affordance provided by visual identifiers.

**Out of scope:**
* Specific vendor implementations (e.g., Material Design, Apple Human Interface Guidelines).
* Technical implementation details (e.g., SVG vs. PNG, CSS Flexbox vs. Grid).
* Branding or aesthetic styling (e.g., color palettes, border radii).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Label** | A textual string that provides the primary name or description of a control's function. |
| **Icon** | A simplified, symbolic graphic used to represent a concept, action, or object (e.g., a magnifying glass for "Search"). |
| **Image** | A complex, detailed visual representation, often unique to a specific data instance (e.g., a user profile photo or a product thumbnail). |
| **Affordance** | The visual qualities of a control that suggest how it should be used. |
| **Visual Redundancy** | The practice of providing both a label and an icon to ensure clarity across different user cognitive styles. |
| **Decorative Element** | An icon or image that provides no unique information and is ignored by assistive technologies. |

## Core Concepts
### The Identification Triad
Every display control relies on one or more of the following to communicate its intent:
1.  **Textual Clarity (Label):** The most unambiguous method of communication.
2.  **Symbolic Recognition (Icon):** Speeds up recognition for experienced users and provides visual anchors.
3.  **Contextual Representation (Image):** Provides specific data-driven context that a generic icon cannot.

### Cognitive Load and Recognition
The human brain processes images faster than text, but text is less prone to misinterpretation. The 078 standard dictates that while icons improve scanning speed, they should rarely exist without a textual label or an accessible alternative (like a tooltip or ARIA label) unless the icon is universally standardized (e.g., a "Home" house icon).

### Accessibility and Semantics
A control must be perceivable regardless of the visual medium. This requires:
*   **Labels** to be programmatically associated with the control.
*   **Icons** to be marked as "hidden" if they are redundant to the label.
*   **Images** to have alternative text if they convey information not present in the label.

## Standard Model
The standard model for a display control follows a strict hierarchy of information:

1.  **Leading Element (Optional):** Usually an Icon or Image. It serves as a visual anchor on the left (in LTR languages).
2.  **Primary Content (Required):** The Label. This is the semantic core of the control.
3.  **Trailing Element (Optional):** Usually a secondary Icon (e.g., an external link arrow or a dropdown chevron) indicating the *type* of action or the state of the control.

### Alignment Logic
*   **Vertical Alignment:** All three elements should be centered along a common horizontal axis.
*   **Spacing:** Consistent gutters must exist between the Icon/Image and the Label to prevent visual crowding.

## Common Patterns
*   **Icon + Label:** The most common pattern for navigation and primary actions.
*   **Image + Label:** Used for entity-specific controls, such as user accounts or product selections.
*   **Icon-Only (Standardized):** Reserved for high-frequency, universally understood actions (e.g., Print, Save, Delete) where space is at a premium.
*   **Label-Only:** Used when the action is abstract or when an icon would add unnecessary visual noise.

## Anti-Patterns
*   **Mystery Meat Navigation:** Using icons without labels or tooltips, forcing the user to "guess" the function.
*   **Redundant Alt-Text:** Providing alt-text for an icon that is identical to the adjacent label (e.g., an icon of a disk with the alt-text "Save" next to a button labeled "Save").
*   **Icon Overload:** Using different icons to represent the same action in different parts of the system.
*   **Misleading Metaphors:** Using an icon that has a conflicting conventional meaning (e.g., using a "Heart" icon for "Save" instead of "Favorite").

## Edge Cases
*   **Right-to-Left (RTL) Languages:** When mirroring the interface, icons that imply direction (like arrows) must be flipped, but "static" icons (like a camera) should generally remain as-is.
*   **Truncated Labels:** When a label is too long for the container, the icon must remain visible while the text truncates with an ellipsis.
*   **Missing Assets:** Systems must define a "Fallback Icon" or "Placeholder Image" to maintain the layout structure if the primary visual asset fails to load.
*   **Dynamic Labels:** Labels that change based on state (e.g., "Play" vs. "Pause"). The icon and label must update simultaneously to maintain synchronization.

## Related Topics
*   **042 Accessibility Standards:** Detailed requirements for screen reader compatibility.
*   **105 Navigation Systems:** How display controls are aggregated into menus and bars.
*   **012 Typography and Font Scaling:** Standards for label legibility and sizing.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |