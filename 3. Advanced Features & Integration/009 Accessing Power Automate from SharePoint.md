# 009 Accessing Power Automate from SharePoint

Canonical documentation for 009 Accessing Power Automate from SharePoint. This document defines concepts, terminology, and standard usage.

## Purpose
The integration between SharePoint and Power Automate exists to bridge the gap between content management and business process automation. It addresses the need for event-driven logic and manual workflow initiation within the context of document libraries and lists. By providing direct access points, the system allows users to transition from data storage to process execution without context switching, ensuring that business logic is tightly coupled with the data it governs.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative regarding the architectural relationship between the content host (SharePoint) and the orchestration engine (Power Automate).

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Entry points within the SharePoint User Interface (UI).
* The mechanism of context passing from a list/library to a workflow.
* Trigger types (Automated vs. Instant).
* Permission requirements for accessing and managing integrations.

**Out of scope:**
* Internal logic design within Power Automate (e.g., how to build a specific flow).
* Third-party workflow engines or connectors.
* On-premises SharePoint Server configurations (unless via Data Gateway).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Trigger** | The specific event or action that initiates the execution of a workflow. |
| **Contextual Data** | The metadata and properties of a SharePoint item (ID, Name, Path) passed to the workflow engine upon initiation. |
| **Command Bar** | The primary UI element in SharePoint lists and libraries where "Automate" or "Integrate" options are surfaced. |
| **Instant Flow** | A workflow triggered manually by a user selecting an item and choosing a command. |
| **Automated Flow** | A workflow triggered by a system event, such as the creation or modification of an item. |
| **Flow Owner** | The identity under which the workflow is managed and whose permissions may dictate access levels. |

## Core Concepts
The relationship between SharePoint and Power Automate is built on three fundamental pillars:

1.  **Contextual Awareness:** Accessing Power Automate from SharePoint is not merely a link; it is a state-aware transition. The system identifies the specific list, library, or item currently in view and passes these identifiers to the workflow engine.
2.  **Event Decoupling:** While the access point exists within SharePoint, the execution is decoupled. SharePoint acts as the "Event Producer," and Power Automate acts as the "Event Consumer."
3.  **Permission Parity:** Access to initiate or create workflows is governed by the intersection of SharePoint site permissions and Power Automate environment permissions. A user cannot automate what they cannot access.

## Standard Model
The standard model for accessing Power Automate from SharePoint follows a tiered hierarchy:

### 1. The Integration Menu
Users access the "Integrate" or "Automate" dropdown menu located in the SharePoint Command Bar. This serves as the primary gateway for:
*   Creating new flows based on the current list/library schema.
*   Viewing existing flows associated with the container.
*   Accessing "Configure Flows" to manage settings.

### 2. The Item-Level Trigger (Instant Flows)
For manual processes, the standard model utilizes the "For a selected item" or "For a selected file" trigger. This appears in the item's context menu (three dots) or the command bar when an item is highlighted.

### 3. The Background Observer (Automated Flows)
Automated access is non-visual. The workflow engine "subscribes" to the SharePoint list's change log. When a "Create," "Update," or "Delete" event occurs, the engine is notified via a webhook-like mechanism.

## Common Patterns
*   **The Approval Loop:** A user selects a document and initiates an "Approval" flow via the Command Bar. The system passes the file's metadata to the flow to route it to the appropriate manager.
*   **Data Synchronization:** A flow is triggered automatically whenever a list item is modified, ensuring that data is mirrored to an external database or another SharePoint list.
*   **Provisioning:** Using a SharePoint list as a "Request Form" where the act of saving a new item triggers a complex workflow to provision resources (e.g., creating a new Team or Site).

## Anti-Patterns
*   **Hardcoding Identifiers:** Creating flows that use static GUIDs or URLs instead of the dynamic context provided by the SharePoint trigger. This makes the flow fragile and non-portable.
*   **Over-Triggering:** Designing automated flows on high-frequency lists without appropriate "Trigger Conditions," leading to unnecessary runs and performance throttling.
*   **Identity Confusion:** Relying on the "Run-only user" permissions for automated flows, which can lead to failures if the initiating user lacks the necessary underlying permissions for secondary actions.
*   **Shadow Automation:** Creating critical business processes in personal environments rather than Service Accounts or Co-owned environments, leading to "orphaned" flows when a user leaves the organization.

## Edge Cases
*   **Large List Throttling:** When a list exceeds the 5,000-item view threshold, certain "Access" methods (like viewing all flows associated with the list) may experience latency or failure.
*   **Guest Access:** External users (Guests) may see the SharePoint UI but may be restricted from accessing the Power Automate interface or triggering flows depending on tenant-level cross-tenant access settings.
*   **Hidden Lists:** Accessing Power Automate for system lists or hidden libraries that do not surface the "Integrate" menu requires manual connection via the Power Automate portal using the list's GUID.
*   **Content Approval State:** If "Content Approval" is enabled on a library, a flow triggered by a "New Item" may be limited in what it can do until the item reaches an "Approved" state.

## Related Topics
*   **010 SharePoint Trigger Conditions:** Deep dive into filtering events before they reach the workflow engine.
*   **042 Power Automate Environment Strategy:** How environments affect the visibility of flows within SharePoint.
*   **088 SharePoint Permissions Framework:** Understanding how Site Member/Owner roles translate to Flow permissions.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |