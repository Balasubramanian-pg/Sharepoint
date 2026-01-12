# 018 Common SharePoint Actions

Canonical documentation for 018 Common SharePoint Actions. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Common SharePoint Actions is to provide a standardized framework for interacting with objects within the SharePoint ecosystem. This topic addresses the need for consistent data manipulation, collaboration protocols, and information lifecycle management within a web-based collaborative environment. By defining these actions, organizations can ensure data integrity, security compliance, and operational efficiency across diverse site collections.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative, focusing on the logical operations of the SharePoint platform rather than specific UI iterations.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Content Operations:** Standard CRUD (Create, Read, Update, Delete) operations for files, list items, and folders.
* **Collaboration Actions:** Sharing, co-authoring, and version control mechanisms.
* **Administrative Actions:** Permission management, site provisioning, and structural modifications.
* **Metadata Management:** Tagging, content type application, and indexing.

**Out of scope:**
* **Custom Development:** Specific code implementations using SPFx, C#, or third-party APIs.
* **Infrastructure Management:** Server-side maintenance for on-premises deployments (SQL Server, IIS).
* **External Integrations:** Specific configurations for non-Microsoft third-party applications.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Site Collection** | A hierarchical grouping of sites that share common administration, navigation, and security boundaries. |
| **Library** | A specialized container for storing and managing documents, supporting versioning and check-out/check-in. |
| **List** | A collection of data organized in rows and columns, similar to a spreadsheet but with relational capabilities. |
| **Inheritance** | The mechanism by which security settings and metadata are passed from a parent object to a child object. |
| **Check-out/Check-in** | A concurrency control mechanism that locks a file for exclusive editing by one user to prevent version conflicts. |
| **Metadata** | Structured information that describes, explains, or locates the primary content (e.g., "Author," "Department"). |
| **Security Trimming** | The process of hiding content or UI elements from users who do not have the requisite permissions to view them. |

## Core Concepts
The fundamental ideas governing SharePoint actions revolve around the **Object Hierarchy** and **Contextual Security**.

1.  **Hierarchical Context:** Every action occurs within a specific scope (Tenant > Site Collection > Site > Library/List > Folder > Item). The impact of an action is often determined by its position in this hierarchy.
2.  **State Management:** Actions often transition an item between states (e.g., Draft to Published, Checked-out to Checked-in).
3.  **Atomic Operations:** Most common actions are designed to be atomic; for instance, moving a file involves a simultaneous "Create" at the destination and "Delete" at the source to ensure data persistence.

## Standard Model
The standard model for SharePoint actions follows the **Request-Authorization-Execution** pattern:

1.  **Request:** A user or system initiates an action (e.g., "Upload Document").
2.  **Authorization:** The system evaluates the user's Effective Permissions against the object's Access Control List (ACL).
3.  **Execution:** If authorized, the system performs the operation and updates the transaction log (Audit Log) and version history.
4.  **Propagation:** The system triggers any secondary processes, such as search indexing or automated workflows.

## Common Patterns
*   **Co-authoring:** Multiple users performing "Update" actions on the same document simultaneously, managed by real-time synchronization engines.
*   **Sharing via Link:** Granting access through a unique tokenized URL rather than direct modification of the site-level permissions.
*   **Content Type Routing:** Automatically categorizing and moving items based on predefined metadata attributes.
*   **Approval Workflows:** A sequence of "Update" actions where an item's status is changed by designated stakeholders before becoming "Published."

## Anti-Patterns
*   **Deep Folder Nesting:** Using folders for categorization instead of metadata, which leads to URL length issues and poor discoverability.
*   **Unique Permissions at Item Level:** Breaking inheritance on individual files or list items frequently, which creates significant administrative overhead and performance degradation.
*   **Manual Versioning:** Appending dates or initials to filenames (e.g., `Document_v1_Final_JM.docx`) instead of utilizing the native versioning engine.
*   **Over-provisioning Sites:** Creating new site collections for small tasks that could be handled by a single list or library.

## Edge Cases
*   **Large List Threshold:** Actions performed on lists exceeding 5,000 items may fail or require specialized indexing to prevent "Resource Throttling."
*   **Orphaned Permissions:** When a user is removed from an identity provider (e.g., Entra ID) but their explicit permissions remain on specific SharePoint objects.
*   **External Guest Access:** Actions performed by users outside the primary organization, which may be subject to restrictive Conditional Access policies.
*   **File Name Limitations:** Handling special characters (~, #, %, &) or reserved names (CON, PRN) that may cause action failure during upload or sync.

## Related Topics
*   **019 SharePoint Governance:** Policies governing the creation and management of sites.
*   **042 Information Architecture:** The structural design of shared information environments.
*   **105 Identity and Access Management (IAM):** The underlying security framework for authentication and authorization.
*   **210 Data Retention and Disposal:** The lifecycle management of content after its active use.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |