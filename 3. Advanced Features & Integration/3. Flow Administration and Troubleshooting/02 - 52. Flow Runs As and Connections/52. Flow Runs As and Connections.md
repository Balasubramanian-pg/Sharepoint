# 052 Flow Runs As and Connections

Canonical documentation for 052 Flow Runs As and Connections. This document defines concepts, terminology, and standard usage.

## Purpose
The "Flow Runs As and Connections" framework addresses the critical requirement of identity management and security context within automated workflows. In any automated system, a process must interact with external resources (databases, APIs, file systems). This topic defines how an automation engine establishes its identity, how it authenticates to external systems, and the governance of permissions during execution. It ensures that automated processes operate within a predictable, secure, and auditable security boundary.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The conceptual relationship between a workflow engine and external resource authentication.
* The distinction between the identity that triggers a flow and the identity that executes it.
* Lifecycle management of credentials used within automated connections.
* Security principles governing delegated access.

**Out of scope:**
* Specific vendor implementation details (e.g., Microsoft Power Automate, Zapier, or Workato UI steps).
* Network-level security (e.g., VPNs, Firewalls) unless directly related to identity handshakes.
* Programming language-specific syntax for API calls.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Flow** | A sequence of automated steps or logic executed by an orchestration engine. |
| **Run-As Identity** | The security principal (user or service account) whose permissions are used to execute the flow's logic. |
| **Connection** | A persisted, authenticated configuration that allows a flow to interact with a specific external service. |
| **Triggering Identity** | The entity (human or system) whose action initiates the flow execution. |
| **Service Account** | A non-human identity intended for automated processes, typically with static or long-lived credentials. |
| **Delegated Permissions** | A security model where a flow acts on behalf of a user, limited by both the user's and the flow's permissions. |
| **Connection Reference** | An abstraction layer that decouples the flow logic from the specific credentials of a connection. |

## Core Concepts

### Identity Decoupling
A fundamental concept where the identity that *designs* or *owns* a flow is not necessarily the identity that *executes* the flow. This allows for organizational continuity even if a specific employee leaves the organization.

### The Execution Context
Every flow run operates within an execution context. This context determines:
1. **Visibility:** What data the flow can see.
2. **Authority:** What actions the flow can perform (Read, Write, Delete).
3. **Auditability:** Which identity is logged in the target system's audit trail.

### Connection Persistence
Connections are typically stored outside the flow logic. They encapsulate the authentication handshake (OAuth tokens, API keys, or Basic Auth) and provide a "bridge" for the flow to cross into external environments.

## Standard Model

The standard model for Flow Runs As and Connections follows a three-tier architecture:

1.  **The Trigger:** An event occurs. The system captures the **Triggering Identity**.
2.  **The Logic (Run-As):** The flow engine evaluates the **Run-As Configuration**. It decides whether to use the Triggering Identity's context or a pre-defined **Service Identity**.
3.  **The Action (Connection):** The flow invokes a **Connection**. The connection uses the stored credentials to authenticate with the target resource.

### Run-As Options
*   **Run as Triggering User:** The flow inherits the permissions of the person who started it. Ideal for personal productivity.
*   **Run as Flow Owner/Service Account:** The flow uses a centralized identity. Ideal for enterprise-wide processes where consistency is required regardless of who initiated the event.

## Common Patterns

### The Service Account Pattern
For production-grade automations, connections are established using a dedicated Service Account. This ensures that the flow does not break if a specific user's password expires or if they change roles.

### The "On Behalf Of" (OBO) Pattern
The flow uses the Triggering Identity to perform actions. This ensures that the audit logs in the destination system correctly reflect the human who initiated the process.

### Connection Pooling/Sharing
In large organizations, connections are often created once by an administrator and shared with multiple flows or users. This centralizes credential management and reduces the "secret sprawl."

## Anti-Patterns

*   **Hardcoded Credentials:** Embedding API keys or passwords directly into the flow logic rather than using a Connection object.
*   **Over-Privileged Service Accounts:** Granting a flow's connection "Global Admin" or "Superuser" rights when it only needs to read a single spreadsheet.
*   **Using Personal Accounts for Critical Infrastructure:** Creating production flows that run under an individual employee's identity, leading to "Orphaned Flows" when that employee leaves.
*   **Implicit Trust:** Assuming that because a user can trigger a flow, they should have access to all connections used within that flow.

## Edge Cases

### Token Expiration in Long-Running Flows
If a flow is designed to wait for several days (e.g., an approval process), the authentication token in the Connection may expire before the flow resumes. Robust systems must implement "refresh token" logic or re-authentication checkpoints.

### Permission Mismatch
A scenario where the **Run-As Identity** has permission to execute the flow, but the **Connection Identity** lacks permission to the specific resource (e.g., a specific folder in a cloud drive). This results in a "Partial Success" or "Access Denied" error mid-execution.

### Multi-Tenant Impersonation
In environments spanning multiple cloud tenants, a flow may run in Tenant A but need to connect to a resource in Tenant B. This requires complex cross-tenant trust configurations and specific Run-As permissions.

## Related Topics
*   **012 Identity and Access Management (IAM):** The foundational security layer for all identities.
*   **088 Audit Logging and Traceability:** How flow executions are recorded.
*   **104 Secrets Management:** The secure storage of the credentials used by Connections.
*   **022 Least Privilege Principle:** The guiding philosophy for assigning permissions to Run-As identities.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |