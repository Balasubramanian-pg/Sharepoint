# [007 Authentication Basics Access Tokens and Permissions](4. Development & Deployment/REST API Mastery/1. Fundamentals and Connectivity/007 Authentication Basics Access Tokens and Permissions.md)

Canonical documentation for [007 Authentication Basics Access Tokens and Permissions](4. Development & Deployment/REST API Mastery/1. Fundamentals and Connectivity/007 Authentication Basics Access Tokens and Permissions.md). This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Access Tokens and Permissions is to provide a secure, scalable, and decoupled mechanism for authorizing requests in distributed systems. This topic addresses the challenge of verifying whether a requester (the "Subject") has the necessary rights to perform a specific action on a resource without requiring the resource provider to re-authenticate the user or maintain a direct connection to the identity store for every transaction.

By utilizing tokens, systems can achieve statelessness, allowing the Resource Server to trust the Authorization Server’s assertion regarding the Subject's identity and granted privileges.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The conceptual lifecycle of an access token (issuance, presentation, validation).
* The theoretical structure of permissions and scopes.
* The relationship between Authentication (identity) and Authorization (rights).
* Security principles governing token usage.

**Out of scope:**
* Specific vendor implementations (e.g., Auth0, AWS Cognito, Azure AD).
* Specific protocol specifications (e.g., the internal mechanics of OAuth 2.0 or OpenID Connect), though they may be referenced as examples.
* Database schema designs for storing permissions.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Access Token** | A digital artifact that represents the authorization granted to a client, typically containing or referencing the permissions assigned to a subject. |
| **Subject** | The entity (user, service, or device) to whom the token is issued. |
| **Permission** | A discrete right to perform an action on a specific resource or resource type. |
| **Scope** | A mechanism used to limit the range of access granted by a token; often a logical grouping of permissions. |
| **Resource Server** | The service or application that hosts the protected data and validates the access token. |
| **Authorization Server** | The entity responsible for authenticating the subject and issuing the access token. |
| **Claim** | A statement about a subject or the token itself (e.g., "user_id", "exp") asserted by the issuer. |
| **Bearer Token** | A type of access token where possession of the token is the sole requirement for access, regardless of who presents it. |

## Core Concepts

### The Authentication-Authorization Transition
Authentication is the process of verifying *who* a subject is. Once identity is established, the system transitions to Authorization, which determines *what* that subject is allowed to do. The Access Token serves as the bridge between these two phases, carrying the results of the authentication into the authorization context.

### Token Anatomy
While formats vary, a standard access token conceptually contains:
1.  **Metadata:** Information about the token itself (algorithm, type).
2.  **Payload:** Claims regarding the subject, the issuer, the audience (intended recipient), and the expiration time.
3.  **Permissions/Scopes:** The specific rights granted for the duration of the token's validity.
4.  **Proof of Integrity:** A cryptographic signature or message authentication code (MAC) that prevents tampering.

### Stateless Validation
A primary benefit of access tokens is the ability for a Resource Server to validate a request without querying a central database. By verifying the cryptographic signature of the token and checking the expiration claim, the Resource Server can trust the permissions contained within the token.

## Standard Model

The standard model for token-based access follows a four-step lifecycle:

1.  **Request:** The Subject authenticates with the Authorization Server and requests access to a specific resource.
2.  **Issuance:** The Authorization Server verifies the identity and the requested scopes. It generates a signed Access Token containing the approved permissions.
3.  **Presentation:** The Subject (or a client acting on their behalf) includes the Access Token in the header of a request to the Resource Server.
4.  **Verification:** The Resource Server validates the token's signature, ensures it has not expired, checks that it was intended for this specific resource (Audience), and confirms the required permissions are present.

## Common Patterns

### Scoped Access
Permissions are often abstracted into "Scopes." For example, a token might have the scope `read:profile`. The Resource Server maps this scope to specific data fields or API endpoints.

### Role-Based Access Control (RBAC)
Tokens may include a "Role" claim (e.g., `role: admin`). The Resource Server then grants access based on the broad category of the user rather than individual granular permissions.

### Attribute-Based Access Control (ABAC)
Tokens carry attributes about the subject (e.g., `department: engineering`). The Resource Server uses logic to determine access based on these attributes combined with environmental factors (e.g., "Is the user in the engineering department AND is it during business hours?").

## Anti-Patterns

*   **Opaque Token Misuse:** Treating an opaque token (a random string) as a structured token (like a JWT) without a validation endpoint (Introspection).
*   **Infinite Lifetimes:** Issuing access tokens that never expire, which removes the ability to revoke access if a token is compromised.
*   **Sensitive Data in Tokens:** Placing unencrypted PII (Personally Identifiable Information) or secrets inside a token payload. Even if signed, tokens are often easily decoded.
*   **Confusing Authentication with Authorization:** Using an Identity Token (ID Token) to grant access to resources. ID tokens are for identity information; Access Tokens are for resource access.
*   **Excessive Scopes:** Issuing tokens with "God-mode" permissions (e.g., `scope: *`) when only limited access is required (violating the Principle of Least Privilege).

## Edge Cases

### Token Revocation
In a stateless model, revoking a token before it expires is difficult. Common solutions include short-lived tokens with refresh mechanisms or maintaining a "blocklist" of revoked token IDs (JTI) at the Resource Server level, which reintroduces a degree of statefulness.

### Clock Skew
Distributed systems may have slightly out-of-sync clocks. Validation logic must account for a small "grace period" (typically seconds) when checking the `nbf` (not before) and `exp` (expiration) claims.

### Token Bloat
In complex systems with many permissions, the token size can exceed the limits of HTTP headers. This requires moving from "Value Tokens" (carrying all data) to "Reference Tokens" (carrying a pointer to the data).

## Related Topics
*   **JSON Web Tokens (JWT):** The most common standard format for access tokens.
*   **Refresh Tokens:** Long-lived credentials used to obtain new access tokens.
*   **OAuth 2.0 Framework:** The industry-standard protocol for delegated authorization.
*   **Principle of Least Privilege (PoLP):** The security philosophy of granting only the minimum permissions necessary.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |