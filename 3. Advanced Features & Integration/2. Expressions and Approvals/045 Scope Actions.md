# 045 Scope Actions

Canonical documentation for 045 Scope Actions. This document defines concepts, terminology, and standard usage.

## Purpose
045 Scope Actions exist to provide a structured framework for defining, limiting, and executing operations within a bounded context. The primary problem space addressed is the ambiguity between "what" an entity can do and "where" or "under what conditions" those actions are valid. By decoupling the action from the global permission set and anchoring it to a specific scope, systems can achieve higher levels of security, auditability, and operational precision.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The theoretical framework for binding actions to specific boundaries.
* Classification of action types within hierarchical and flat scopes.
* Governance principles for action-to-scope mapping.
* Lifecycle of a scoped action (definition to execution).

**Out of scope:**
* Specific vendor implementations (e.g., AWS IAM Policies, OAuth2 Scopes, Kubernetes RBAC).
* Programming language-specific syntax for scope declaration.
* Network-level scoping (e.g., VLANs, Subnets) unless used as a logical boundary for an action.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Action** | A discrete operation or function that can be performed upon a resource or within a system. |
| **Scope** | A logical or physical boundary that defines the extent of influence or access for a given action. |
| **Principal** | The entity (user, service, or process) attempting to perform a scoped action. |
| **Boundary Constraint** | A rule that limits the execution of an action to a specific subset of resources or timeframes. |
| **Scope Elevation** | The process of temporarily or permanently expanding the boundaries within which an action can be performed. |
| **Resolution** | The process of determining if a requested action falls within the permitted scope of the principal. |

## Core Concepts

### The Action-Scope Binding
An action is never considered in isolation. In the 045 framework, an action is a vector that requires a point of origin (Principal) and a defined field of effect (Scope). The binding ensures that even if a principal possesses the "Delete" action, that action is inert unless it targets a resource within its assigned scope.

### Granularity Levels
Scope actions operate across varying levels of specificity:
1.  **Global Scope:** Actions applicable across all domains and resources.
2.  **Domain/Namespace Scope:** Actions restricted to a specific logical grouping.
3.  **Resource Scope:** Actions restricted to a single, unique identifier.
4.  **Attribute Scope:** Actions restricted to specific fields or properties within a resource.

### Inheritance and Propagation
Scopes are often hierarchical. An action permitted at a parent scope typically propagates to child scopes unless an explicit "Deny" or "Override" is present. This follows the principle of **Transitive Authority**.

## Standard Model

The standard model for 045 Scope Actions follows the **PARC** (Principal, Action, Resource, Context) evaluation flow:

1.  **Request:** A Principal initiates an Action.
2.  **Identification:** The system identifies the target Resource.
3.  **Contextualization:** The system gathers environmental variables (time, location, security posture).
4.  **Scope Validation:** The system checks if the Action-Resource pair exists within the Principal's permitted Scope.
5.  **Execution:** If validated, the action is performed; otherwise, a scope violation is logged.

### Mathematical Representation
An authorized action $A$ is valid if and only if:
$$A \in S_p \cap R_t$$
Where $S_p$ is the set of actions allowed for the Principal and $R_t$ is the set of actions allowed on the Target Resource within the current context.

## Common Patterns

### The "Least Privilege" Pattern
Assigning the narrowest possible scope to an action by default. Actions are only expanded to broader scopes upon documented requirement.

### Functional Scoping
Grouping actions by business function (e.g., `Finance:Read`, `Engineering:Write`) rather than technical layers. This aligns technical boundaries with organizational boundaries.

### Temporal Scoping
Defining actions that are only valid within a specific window of time or following a specific trigger event.

## Anti-Patterns

### Scope Creep (Over-scoping)
Granting "Global" or "Wildcard" scopes to actions that only require "Resource" or "Namespace" level access. This increases the blast radius of a compromised principal.

### Ambiguous Action Naming
Using non-descriptive terms like `Manage` or `Access`. Standard 045 practice requires explicit verbs (e.g., `Create`, `Update`, `Archive`).

### Hard-coded Scopes
Embedding scope logic directly into the application code rather than using a centralized policy engine. This makes auditing and updating scopes impossible without a code deployment.

## Edge Cases

### Cross-Scope Dependencies
An action in Scope A may require a secondary, read-only action in Scope B to complete. Systems must decide whether to allow "Implicit Scope Expansion" or require "Explicit Multi-Scope Authorization."

### Orphaned Actions
When a resource is moved from Scope A to Scope B, actions previously authorized may become invalid. The system must define whether the action follows the resource or stays with the scope.

### Shadow Scopes
Situations where a principal gains access to an action through an unintended path, such as a group membership that overlaps with a restricted resource.

## Related Topics
* **Identity and Access Management (IAM):** The broader discipline governing principals and permissions.
* **Principle of Least Privilege (PoLP):** The security philosophy underpinning scoped actions.
* **Attribute-Based Access Control (ABAC):** A method of defining scopes based on resource and principal attributes.
* **Audit Logging:** The recording of scoped action attempts for compliance.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |