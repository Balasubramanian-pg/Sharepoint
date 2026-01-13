# 097 Customize Gallery Template

Canonical documentation for 097 Customize Gallery Template. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of the Gallery Template customization standard is to provide a framework for modifying the visual representation and functional behavior of repeated data sets. In modern interface design, a "Gallery" serves as a primary vehicle for displaying collections of similar items (e.g., products, profiles, media, or records). 

Customization allows designers and developers to transcend default layouts, ensuring that the information density, aesthetic alignment, and interactive capabilities of the gallery meet specific user experience (UX) requirements. This topic addresses the challenge of balancing standardized data structures with flexible presentation layers.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Structural Modification:** Defining the layout and arrangement of elements within a single gallery item.
* **Data Mapping:** The logic of binding specific data fields to template components.
* **Visual Styling:** The application of thematic elements (typography, spacing, color) to the template.
* **Conditional Logic:** Rules that govern how a template changes based on the data it consumes.
* **Responsive Behavior:** How the template adapts to different viewport constraints.

**Out of scope:**
* **Backend Data Retrieval:** The specific methods used to fetch data from a database (SQL, NoSQL, API).
* **Vendor-Specific Syntax:** Specific code snippets for platforms like React, Power Apps, or WordPress (though the logic applies to all).
* **Server-Side Performance:** Optimization of the data stream itself.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Gallery** | A UI component designed to display a collection of records using a repeating visual structure. |
| **Template** | The blueprint or "master" item that defines the layout, style, and data bindings for every item in the gallery. |
| **Item Container** | The individual bounding box or area that encapsulates a single record's representation. |
| **Data Binding** | The process of connecting a specific attribute of a data record (e.g., "Price") to a specific UI element (e.g., a Label). |
| **Placeholder** | A temporary visual or structural element used within a template before live data is populated. |
| **Iteration** | The process of the system generating a new instance of the template for every record in the data source. |

## Core Concepts

### The Repeating Unit
The fundamental concept of a Gallery Template is the "Repeating Unit." Unlike a static page where every element is unique, a gallery relies on a single template definition that is cloned for every entry in the data source. Customizing the template at the "Unit" level automatically propagates changes across the entire collection.

### Data-Driven UI
Customization is not merely aesthetic; it is data-driven. A customized template must be able to interpret data values to determine its state. For example, a "Status" field in the data might trigger a color change in the template's background.

### Spatial Constraints
Gallery templates exist within a parent container. Customization must account for how items flow (horizontally, vertically, or in a grid) and how they interact with neighboring items (margins, padding, and alignment).

## Standard Model

The standard model for Gallery Template customization follows a three-layer architecture:

1.  **The Schema Layer:** Defines the available data fields that can be injected into the template.
2.  **The Presentation Layer (The Template):** Defines the HTML/XML/Component structure, including the placement of images, text, and buttons.
3.  **The Logic Layer:** Defines the "if-then" rules (e.g., "If 'Stock' < 5, show 'Low Stock' badge").

### The Lifecycle of a Customized Template
*   **Initialization:** The template is defined with placeholders.
*   **Binding:** Data fields are mapped to placeholders.
*   **Transformation:** Styles and logic are applied based on the specific values of the record.
*   **Rendering:** The final UI is generated for the end-user.

## Common Patterns

### The "Card" Pattern
The most prevalent gallery template, featuring a container with a shadow or border, an image at the top, a title, and a call-to-action (CTA) at the bottom.

### The "Line Item" Pattern
A condensed, horizontal template used for high-density data displays, often resembling a list but retaining the customization flexibility of a gallery.

### Conditional Formatting
Applying different styles to a template based on data thresholds. For example, highlighting a "Priority" item with a different border color.

### Interactive States
Customizing how the template reacts to user input (Hover, Focus, Selection). This often involves changing the elevation or scale of the item container.

## Anti-Patterns

*   **Hardcoding Content:** Placing static text or images inside a template that should be driven by the data source.
*   **Over-Nesting:** Creating excessively complex hierarchies within a single item container, leading to performance degradation during rendering.
*   **Fixed Dimensions in Fluid Layouts:** Assigning absolute pixel widths to template elements, which causes the gallery to break on smaller screens.
*   **Logic Bloat:** Placing heavy computational logic inside the template's render cycle rather than pre-processing the data.

## Edge Cases

*   **Empty States:** How the gallery behaves when the data source returns zero records. A customized template should ideally include a "No Data" state.
*   **Variable Content Length:** Handling instances where one record has a 10-word description and another has 500 words. Standardized templates must use truncation or flexible heights.
*   **Missing Data (Nulls):** Defining how the template renders when a bound data field is empty (e.g., hiding an image component if no URL is provided).
*   **Heterogeneous Data:** When a single gallery must display different types of items (e.g., a mix of "Ad" tiles and "Product" tiles). This requires "Template Switching" logic.

## Related Topics
*   **042 Data Binding Standards:** The underlying mechanics of connecting UI to Data.
*   **115 Responsive Design Frameworks:** How containers behave across device types.
*   **088 State Management:** Handling the "Selected" or "Active" state of gallery items.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |