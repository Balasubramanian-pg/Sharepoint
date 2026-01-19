# [086 Understanding 403 Forbidden vs 401 Unauthorized](4. Development & Deployment/REST API Mastery/6. Optimization Batches and Security/086 Understanding 403 Forbidden vs 401 Unauthorized.md)

Canonical documentation for [086 Understanding 403 Forbidden vs 401 Unauthorized](4. Development & Deployment/REST API Mastery/6. Optimization Batches and Security/086 Understanding 403 Forbidden vs 401 Unauthorized.md). This document defines concepts, terminology, and standard usage.

## Purpose
The distinction between HTTP 401 (Unauthorized) and HTTP 403 (Forbidden) is fundamental to the security architecture of web-based systems. This topic addresses the ambiguity often found in the naming of these status codes—specifically that "401 Unauthorized" semantically refers to a lack of *authentication*, while "403 Forbidden" refers to a lack of *authorization*. 

Properly implementing these codes ensures that clients (both human and machine) can programmatically determine whether they need to provide credentials or if the requested action is fundamentally disallowed regardless of their identity.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative, adhering to the standards defined in RFC 9110 (HTTP Semantics).

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The semantic difference between authentication and authorization.
* The technical requirements for issuing a 401 vs. a 403 response.
* Client-side expectations upon receiving these status codes.
* Security implications of status code selection.

**Out of scope:**
* Specific implementation details in frameworks (e.g., Spring Security, Passport.js, Django).
* Configuration of specific web servers (e.g., Nginx, Apache).
* Non-HTTP protocol error codes.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Authentication** | The process of verifying the identity of a user, device, or system (the "Who"). |
| **Authorization** | The process of verifying that an authenticated entity has permission to perform a specific action or access a specific resource (the "What"). |
| **Principal** | An entity that can be authenticated (e.g., a user, a service account). |
| **Challenge** | A mechanism (usually via the `WWW-Authenticate` header) by which a server requests credentials from a client. |
| **Resource** | The specific URI or data object the client is attempting to access. |

## Core Concepts

### The Identity vs. Permission Divide
The core of the 401 vs. 403 distinction lies in the state of the requester's identity:
1.  **401 Unauthorized:** The requester's identity is unknown or unverified. The server is saying, "I don't know who you are, or your proof of identity is invalid."
2.  **403 Forbidden:** The requester's identity is known and verified, but that identity does not possess the necessary rights to access the resource. The server is saying, "I know who you are, but you aren't allowed to do this."

### The Misnomer of "Unauthorized"
In standard English, "unauthorized" often implies a lack of permission. However, in the HTTP specification, 401 is strictly tied to **Authentication**. If a user is logged in but lacks permissions, returning a 401 is technically incorrect because it signals to the client that they should try to log in again, creating an infinite authentication loop.

## Standard Model

### The 401 Unauthorized Workflow
According to RFC 9110, a 401 response **must** include a `WWW-Authenticate` header field containing at least one challenge applicable to the target resource.
*   **Trigger:** Missing, invalid, or expired credentials.
*   **Client Action:** The client should prompt the user for credentials or attempt to refresh a token.
*   **Requirement:** Must include a challenge header.

### The 403 Forbidden Workflow
A 403 response indicates that the server understood the request but refuses to fulfill it. Unlike 401, providing credentials will not change the outcome.
*   **Trigger:** Insufficient roles, insufficient scopes, IP blacklisting, or business logic restrictions.
*   **Client Action:** The client should cease the request and notify the user that they lack the necessary permissions.
*   **Requirement:** No specific header is required, though a response body explaining the reason is recommended.

## Common Patterns

### Step-Up Authentication
In some scenarios, a user may be authenticated (403 is not yet applicable) but requires a higher level of assurance (e.g., Multi-Factor Authentication) to access a sensitive resource. While some systems use a 403 here, the standard approach is often a 401 with a specific challenge or a custom application-level code to trigger the "step-up" process.

### RBAC and ABAC Mapping
*   **Role-Based Access Control (RBAC):** If a user has the role "User" but the resource requires "Admin," the server issues a **403**.
*   **Attribute-Based Access Control (ABAC):** If a user owns a resource but it is currently "Locked" by system logic, the server issues a **403**.

## Anti-Patterns

### 1. Using 401 for Logged-in Users
Issuing a 401 to a user who is already successfully logged in but lacks a specific role. This causes browsers and clients to repeatedly prompt for credentials that will never work for that resource.

### 2. Using 403 for Unauthenticated Users
Issuing a 403 when no identity has been provided. This prevents the client from knowing that it *could* access the resource if it simply provided valid credentials.

### 3. Leaking Information via 403
In highly secure environments, returning a 403 confirms that a resource exists, even if the user can't see it. If the existence of the resource itself is a secret, an anti-pattern is avoided by returning a **404 Not Found** instead of a 403.

## Edge Cases

### Expired Tokens
When a Bearer token (JWT) is expired, the identity is no longer verifiable. This should result in a **401**, not a 403, as the "Who" is no longer established.

### IP-Based Restrictions
If a server restricts access based on IP address rather than user identity, it typically issues a **403**. Since the "identity" in this context is the IP, and the IP is known but not allowed, 403 is the semantically correct choice.

### The "Hidden" 403
Some firewalls or Web Application Firewalls (WAFs) return a 403 when they detect a malicious payload (like SQL injection). In this case, the 403 does not refer to the user's identity, but to the server's refusal to process the specific request body for security reasons.

## Related Topics
*   **RFC 9110 (HTTP Semantics):** The primary standard defining these codes.
*   **OAuth 2.0 Scopes:** Often the mechanism used to determine if a 403 should be issued.
*   **JWT (JSON Web Tokens):** A common method for carrying the identity used to decide between 401 and 403.
*   **404 Not Found:** Used as a security measure to hide the existence of resources from unauthorized users.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |