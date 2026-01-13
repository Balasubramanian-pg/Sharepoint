# 008 Desktop Flows RPA

Canonical documentation for 008 Desktop Flows RPA. This document defines concepts, terminology, and standard usage.

## Purpose
Desktop Flows Robotic Process Automation (RPA) exists to automate repetitive, rule-based human interactions within a desktop computing environment. It addresses the "last mile" of digital transformation by enabling integration with legacy applications, thick clients, and web interfaces that lack modern Application Programming Interfaces (APIs). By mimicking human input—such as keystrokes, mouse movements, and screen scraping—Desktop Flows allow organizations to bridge disparate systems and execute workflows at a scale and speed unattainable by manual labor.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **UI Automation:** Interaction with graphical user interface (GUI) elements.
* **Execution Modes:** Attended and unattended automation paradigms.
* **Input Simulation:** Emulation of peripheral devices (keyboard, mouse).
* **Data Extraction:** Techniques for retrieving information from structured and unstructured screen layouts.
* **Error Handling:** Logic for managing UI-based failures and environmental shifts.

**Out of scope:**
* **API-based Automation:** Direct server-to-server communication (Digital Process Automation).
* **Physical Robotics:** Hardware-based automation or industrial robotics.
* **Specific Vendor Implementations:** Proprietary syntax or platform-specific licensing models.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Attended RPA** | Automation that runs on a user's workstation, requiring human intervention or triggering to complete tasks. |
| **Unattended RPA** | Automation that runs independently on a dedicated server or virtual machine without human supervision. |
| **Selector** | A unique identifier or path used to locate a specific UI element (e.g., a button or text field) within an application's hierarchy. |
| **Surface Automation** | A method of automation that relies on image recognition or coordinate-based clicking rather than underlying object metadata. |
| **OCR (Optical Character Recognition)** | The conversion of images of typed, handwritten, or printed text into machine-encoded text for data extraction. |
| **Control** | An individual component of a GUI, such as a checkbox, radio button, or dropdown menu. |
| **Latency** | The delay between an automation command and the application's response, often a critical factor in desktop flow stability. |

## Core Concepts

### UI Element Identification
The foundation of Desktop Flows is the ability to identify and interact with software controls. This is primarily achieved through:
* **Object-based Identification:** Accessing the application's underlying code structure (e.g., HTML DOM, WPF trees, or Java Access Bridge) to find specific IDs or attributes.
* **Visual Identification:** Using computer vision or pattern matching to find elements based on their appearance.

### Execution Context
Desktop Flows operate within a specific session context. This includes the operating system environment, screen resolution, user permissions, and active window focus. Maintaining a consistent execution context is vital for the reliability of the flow.

### Simulation vs. Injection
* **Simulation:** Moving the physical cursor and sending hardware-level scan codes. This is highly compatible but can be disrupted by human movement.
* **Injection:** Sending messages directly to the application's event handler. This is faster and more robust but may not be supported by all legacy applications.

## Standard Model

The standard model for a Desktop Flow follows a structured lifecycle:

1.  **Initialization:** Setting up the environment, launching required applications, and clearing previous states.
2.  **Navigation:** Moving through the application's UI to reach the target functional area.
3.  **Data Processing:** Performing the core task (e.g., entering data, scraping results).
4.  **Validation:** Confirming that the UI state reflects the intended outcome of the action.
5.  **Exception Handling:** Catching UI timeouts, application crashes, or unexpected pop-ups.
6.  **Finalization:** Closing applications and logging the transaction status.

## Common Patterns

### The "Wait-Action-Verify" Pattern
To ensure stability, every interaction should follow a sequence:
1.  **Wait:** Ensure the element is visible and enabled.
2.  **Action:** Perform the click or keystroke.
3.  **Verify:** Check for a UI change (e.g., a new window appearing) to confirm success.

### State Machine Logic
Complex flows often use a state machine pattern where the automation identifies its current "state" (which screen is currently visible) and determines the next valid transition, allowing for non-linear navigation.

### Data-Driven Loops
Iterating through a dataset (e.g., a CSV or Excel file) where each row represents a transaction to be processed through the desktop application.

## Anti-Patterns

*   **Hardcoded Coordinates:** Using fixed X/Y pixel locations for clicking. This fails if the screen resolution, window size, or UI layout changes.
*   **Infinite Loops:** Failing to implement a timeout or maximum retry count on "Wait" conditions.
*   **Ignoring Latency:** Assuming an application will respond instantly. This leads to "race conditions" where the bot clicks an element before it has loaded.
*   **Lack of Logging:** Failing to capture screenshots or state data upon failure, making debugging in unattended environments impossible.

## Edge Cases

*   **Dynamic Selectors:** Applications where element IDs change every time the program is launched (e.g., web applications with obfuscated CSS classes).
*   **Context Switches:** Handling unexpected OS-level interruptions, such as Windows Updates, security prompts, or "Low Disk Space" warnings.
*   **Virtual Desktops (VDI):** Automating applications inside a Citrix or RDP session where the bot only sees a flat image of the UI rather than individual objects.
*   **Hidden Elements:** Controls that exist in the application tree but are obscured by other windows or require scrolling to become "interactable."

## Related Topics
*   **007 Digital Process Automation (DPA):** API-led automation.
*   **012 Process Mining:** Discovering the workflows that Desktop Flows should automate.
*   **015 Computer Vision:** Advanced image processing for surface automation.
*   **022 Identity and Access Management (IAM):** Managing credentials for bot accounts.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |