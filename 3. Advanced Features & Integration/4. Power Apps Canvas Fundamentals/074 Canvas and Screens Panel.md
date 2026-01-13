# 074 Canvas and Screens Panel

Canonical documentation for 074 Canvas and Screens Panel. This document defines concepts, terminology, and standard usage.

## Purpose
The **Canvas and Screens Panel** architecture exists to provide a dual-modality interface for visual orchestration. It addresses the problem of managing complex spatial data (the Canvas) alongside structured, hierarchical, or sequential data (the Screens Panel). 

The Canvas serves as the primary workspace for spatial composition, while the Screens Panel acts as the navigational and organizational anchor. Together, they allow users to maintain a "birds-eye view" of a project while simultaneously performing granular edits on individual viewports or output surfaces.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
**In scope:**
* **Spatial Orchestration:** The logic of placing, scaling, and orienting visual elements within a coordinate system.
* **Navigation Logic:** The relationship between selecting an item in a list (Screens Panel) and focusing on a region in the workspace (Canvas).
* **Hierarchy Management:** How screens are ordered, grouped, and layered.
* **Viewport Definition:** The boundaries that define what portion of the canvas is rendered to an output.

**Out of scope:**
* **Specific Rendering Engines:** The technical methods used to draw pixels (e.g., WebGL, Skia, Metal).
* **Input Device Specifics:** Implementation details for mouse, touch, or stylus (though the concepts of "Pan" and "Zoom" are included).
* **Asset Management:** The storage of external files (images, videos) used within the screens.

## Definitions
| Term | Definition |
|------|------------|
| **Canvas** | An infinite or bounded two-dimensional space that serves as the container for all visual elements and screens. |
| **Screen** | A defined viewport or "artboard" within the canvas representing a discrete output or state. |
| **Screens Panel** | A dedicated interface component (typically a sidebar) that lists, organizes, and provides quick access to all screens within the project. |
| **Viewport** | The specific rectangular area of the canvas currently visible to the user or designated for final output. |
| **Coordinate System** | The mathematical grid (X, Y) used to determine the position of screens and elements relative to the Canvas origin (0,0). |
| **Z-Order** | The stacking priority of screens or elements, determining which appears "on top" when overlapping. |

## Core Concepts

### 1. Spatial vs. Logical Organization
The system operates on two simultaneous planes:
* **Spatial (Canvas):** Elements are organized by proximity and visual flow. This is non-linear and allows for free-form brainstorming or layout.
* **Logical (Screens Panel):** Elements are organized by sequence, importance, or category. This is linear and provides a structured index for rapid navigation.

### 2. The Focus-and-Context Relationship
The Screens Panel provides the **Context** (how many screens exist and their names), while the Canvas provides the **Focus** (the actual content of the screens). Selecting a screen in the panel must trigger a "Focus" event on the canvas, typically involving a translation of the canvas coordinates to center the selected screen.

### 3. Infinite Workspace
Modern implementations assume an "Infinite Canvas" where the coordinate system extends indefinitely in all directions. The Screens Panel acts as the "Map" to this infinite space, preventing the user from becoming "lost" in empty coordinates.

## Standard Model
The standard model for a Canvas and Screens Panel system follows a **Master-Detail** pattern:

1.  **The Global Origin:** A fixed (0,0) point on the Canvas.
2.  **Screen Containers:** Each "Screen" is a bounded box with its own local coordinate system nested within the Global Origin.
3.  **The Panel Index:** The Screens Panel reflects the current state of the Canvas. If a screen is added to the Canvas, it is appended to the Panel. If a screen is reordered in the Panel, its Z-order or logical sequence on the Canvas may be updated accordingly.
4.  **Bidirectional Synchronization:** 
    *   Actions on the **Canvas** (e.g., renaming a screen label) update the **Panel**.
    *   Actions in the **Panel** (e.g., deleting a list item) remove the object from the **Canvas**.

## Common Patterns

### The "Flow" Pattern
Screens are arranged on the Canvas in a left-to-right or top-to-bottom sequence to represent a user journey or timeline. The Screens Panel reflects this sequence numerically.

### The "State" Pattern
The Canvas contains a single primary screen, and the Screens Panel is used to toggle between different "States" or "Layers" of that same screen, rather than different spatial locations.

### The "Grid" Pattern
Screens are arranged in a strict grid on the Canvas for batch processing or comparison. The Screens Panel allows for multi-selection to apply bulk changes to the grid.

## Anti-Patterns

*   **Disconnected States:** Allowing a screen to exist in the Screens Panel but not on the Canvas (or vice versa), leading to "ghost" data.
*   **Deep Nesting:** Creating screens within screens within screens. This complicates the coordinate math and makes navigation via the Screens Panel unintuitive.
*   **Coordinate Drift:** Failing to snap screens to integer coordinates, resulting in "blurry" rendering or sub-pixel gaps between adjacent screens.
*   **Panel Overload:** Displaying too much metadata (e.g., full thumbnails, file sizes, timestamps) in the Screens Panel, which reduces its effectiveness as a high-speed navigation tool.

## Edge Cases

*   **Overlapping Screens:** When two screens occupy the same X/Y coordinates. The system must rely on Z-order and the Screens Panel to allow the user to select the "hidden" screen.
*   **Extreme Coordinates:** When a screen is placed so far from the origin (e.g., 1,000,000,000 pixels) that floating-point errors begin to affect rendering precision.
*   **Empty Canvas:** The behavior of the Screens Panel when no screens exist. A standard model should provide a "Create First Screen" call-to-action in both the Panel and the Canvas.
*   **Orphaned Elements:** Visual elements that exist on the Canvas but are not contained within any defined "Screen." The Screens Panel typically ignores these, or moves them to a "Global" or "Unassigned" category.

## Related Topics
*   **012 Coordinate Systems and Projections**
*   **045 Layer Management and Z-Indexing**
*   **089 Viewport Transformation and Zoom Logic**
*   **102 Multi-instance Synchronization**

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |