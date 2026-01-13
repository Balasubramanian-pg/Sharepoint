# 051 Flow Scope and Permissions

Canonical documentation for 051 Flow Scope and Permissions. This document defines concepts, terminology, and standard usage.

## Purpose
The 051 Flow Scope and Permissions framework exists to govern the execution boundaries and access rights of automated sequences (flows). In complex systems, automated processes often act as intermediaries between users, data, and external services. Without a defined scope and permission model, these processes risk becoming vectors for privilege escalation, data leakage, or unauthorized resource consumption.

This topic addresses the problem of ensuring that an automated flow operates with the minimum necessary authority required to complete its task, while maintaining a clear audit trail and preventing the "confused deputy" problem.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   **Execution Boundaries:** The logical limits of a flow's operational reach.
*   **Identity Context:** How a flow assumes an identity (delegated vs. application).
*   **Permission Granularity:** The levels at which access can be granted or restricted.
*   **Lifecycle of Permissions:** How permissions are acquired, used, and revoked during flow execution.

**Out of scope:**
*   **Specific Vendor Implementations:** Syntax for specific platforms (e.g., Azure Logic Apps, AWS Step Functions, Salesforce Flow).
*   **Network-level Security:** Firewalls, VPCs, or DNS configurations (unless directly controlled by the flow scope).
*   **User Interface Design:** How permissions are presented to end-users in a GUI.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
| :--- | :--- |
| **Flow** | A sequence of automated steps or actions triggered by an event to achieve a specific outcome. |
| **Scope** | The defined boundary (temporal, spatial, or logical) within which a flow is authorized to operate. |
| **Principal** | The entity (user, service account, or managed identity) whose permissions the flow utilizes. |
| **Delegated Permission** | Access granted to a flow to act on behalf of a specific user, limited by that user's own rights. |
| **Application Permission** | Access granted to the flow itself, independent of a signed-in user, typically used for background processes. |
| **Least Privilege** | The principle of granting only the minimum permissions necessary for a flow to perform its intended function. |
| **Contextual Elevation** | A temporary increase in permissions granted to a flow for a specific, high-privilege step within a broader sequence. |

## Core Concepts

### 1. The Execution Context
Every flow operates within an execution context. This context carries the identity of the principal and the environmental variables (such as tenant ID, region, or department) that define the flow's "home." The context dictates which resources are visible to the flow before any specific permissions are even evaluated.

### 2. Scope Boundaries
Scopes can be defined in three dimensions:
*   **Data Scope:** Which specific datasets or records the flow can touch.
*   **Functional Scope:** Which specific actions (Read, Write, Delete, Execute) the flow can perform.
*   **Temporal Scope:** The duration for which the flow's permissions are valid (e.g., only during the execution window).

### 3. Permission Inheritance
Flows often inherit permissions from their environment or their creator. Understanding the hierarchy of inheritance is critical to preventing unintended access. Permissions may be "Additive" (combining all available rights) or "Restrictive" (limited to the intersection of flow-specific and user-specific rights).

## Standard Model

The standard model for 051 Flow Scope and Permissions follows a **Tripartite Authorization** structure:

1.  **The Trigger Authority:** The event that initiates the flow must be authenticated. The trigger defines the initial "Seed Identity."
2.  **The Flow Definition:** The logic of the flow itself, which may require specific "App-only" scopes to interact with system-level APIs.
3.  **The Resource Gatekeeper:** The target system (e.g., a database or API) that evaluates the incoming request from the flow against its own Access Control Lists (ACLs).

In this model, the effective permission is the **intersection** of the Trigger Authority and the Flow Definition's authorized scopes.

## Common Patterns

### Delegated Execution Pattern
The flow runs using the identity of the user who triggered it. This is the safest pattern for user-facing automation as it ensures the flow cannot do anything the user themselves cannot do.

### Service Principal Pattern
The flow runs under a dedicated service account. This is used for scheduled tasks or system-to-system integrations where no human user is present. It requires strict monitoring as it often possesses broad permissions.

### Just-in-Time (JIT) Scoping
Permissions are not assigned to the flow permanently. Instead, the flow requests a short-lived token with a specific scope at the moment an action is required, and the token expires immediately after the action completes.

## Anti-Patterns

### The "God-Flow"
Assigning administrative or "Full Control" permissions to a flow to avoid troubleshooting permission errors. This creates a massive security risk if the flow logic is compromised or contains bugs.

### Hardcoded Credentials
Storing secrets, API keys, or passwords directly within the flow definition rather than using a secure vault or managed identity.

### Implicit Trust of Input
Assuming that because a flow has the *permission* to write to a database, the *data* it is writing is safe. Flow scope does not replace the need for input validation.

### Scope Creep
Adding new actions to an existing flow without re-evaluating if the original scope and permissions are still appropriate for the expanded functionality.

## Edge Cases

### Cross-Tenant/Cross-Boundary Flows
When a flow initiated in Environment A needs to access resources in Environment B. This requires "Federated Trust" where Environment B must recognize the identity issued by Environment A.

### Long-Running Flows
Flows that pause for human approval or wait for external events. The permissions granted at the start of the flow may expire before the flow resumes, requiring "Refresh Token" logic or re-authentication.

### Recursive Flows
A flow that triggers itself or another flow that eventually triggers the first. If permissions are not scoped correctly, this can lead to infinite loops that consume all available system resources (Denial of Service).

## Related Topics
*   **022 Identity Management:** The foundation of principals and actors.
*   **084 Audit Logging:** The mechanism for recording how flow permissions are exercised.
*   **112 API Governance:** How external endpoints manage incoming flow requests.

## Change Log

| Version | Date | Description |
| :--- | :--- | :--- |
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |