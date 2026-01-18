# [013 Identifying the Current User via API](4. Development & Deployment/REST API Mastery/1. Fundamentals and Connectivity/013 Identifying the Current User via API.md)

Canonical documentation for [013 Identifying the Current User via API](4. Development & Deployment/REST API Mastery/1. Fundamentals and Connectivity/013 Identifying the Current User via API.md). This document defines concepts, terminology, and standard usage.

## Purpose
In distributed systems and client-server architectures, the server must consistently and securely determine the identity of the entity making a request. Identifying the current user allows an API to provide personalized data, enforce fine-grained access control, and maintain audit logs. This topic addresses the challenge of mapping transient request credentials (such as tokens or session identifiers) to a persistent identity within the application's domain.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Mechanisms for resolving identity from request metadata.
* The conceptual model of the "Current User" context.
* Standardized patterns for exposing identity information to clients.
* Security considerations regarding identity propagation.

**Out of scope:**
* Specific authentication protocols (e.g., the inner workings of OAuth2, SAML, or OIDC).
* Password hashing algorithms or database schema designs for user tables.
* Vendor-specific SDK implementations.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Principal** | The entity (user, device, or service) that is successfully authenticated and identified by the system. |
| **Subject (sub)** | A unique identifier for the principal, intended to be stable and never reassigned. |
| **Identity Context** | The collection of attributes and metadata associated with the current requester during the lifecycle of a single API call. |
| **Claims** | Assertions made about a principal (e.g., name, email, roles) packaged within a security token. |
| **Token Introspection** | The process by which an API validates a credential to retrieve the associated identity information. |
| **Impersonation** | A state where a principal acts on behalf of another user, usually for administrative or support purposes. |

## Core Concepts
### Identity Resolution
Identity resolution is the process of transforming a credential (e.g., a Bearer token or Session ID) into a known internal user object. This typically happens at the middleware or gateway level before the request reaches the business logic.

### Statelessness vs. Statefulness
*   **Stateless Identification:** The identity is contained entirely within the request (e.g., a signed JWT). The server does not need to query a session store to know who the user is.
*   **Stateful Identification:** The request contains a reference (e.g., an opaque session cookie) that the server must look up in a server-side store to retrieve the identity.

### The "Me" Concept
The "Me" concept is an alias pattern where the API provides a dedicated resource path (e.g., `/users/me`) that resolves dynamically based on the requester's credentials. This decouples the client from needing to know its own unique ID before making its first request.

## Standard Model
The standard model for identifying the current user follows a layered approach:

1.  **Extraction:** The API extracts the credential from the transport layer (usually the `Authorization` header or a secure cookie).
2.  **Validation:** The system verifies the integrity and expiration of the credential.
3.  **Context Injection:** The validated identity (Principal) is attached to the request context (e.g., `req.user` or `ThreadLocal`).
4.  **Resolution:** Business logic queries the Identity Context to filter data or authorize actions.
5.  **Exposure:** If the client needs to know its own identity details, it queries a standardized endpoint (the "Who Am I" pattern).

## Common Patterns
### The `/me` Endpoint
A GET request to a singleton resource (e.g., `/api/v1/me` or `/api/v1/users/self`) returns the profile of the currently authenticated user. This is the primary method for clients to bootstrap their UI with user-specific data.

### Header-Based Propagation
In microservice architectures, an API Gateway identifies the user and passes the identity downstream to internal services via custom headers (e.g., `X-User-ID`, `X-User-Roles`).

### Token-Embedded Claims
Using structured tokens (like JWT) to embed the user's ID and basic roles directly. This reduces database load by allowing services to identify the user without a secondary lookup.

## Anti-Patterns
*   **Client-Provided Identity:** Trusting a User ID passed in a request body or query parameter (e.g., `GET /orders?user_id=123`) without verifying that the ID matches the authenticated session.
*   **Sensitive Data in Tokens:** Placing PII (Personally Identifiable Information) or sensitive secrets in unencrypted client-side tokens.
*   **Over-reliance on Global State:** Using global variables to store the current user, which can lead to "identity bleeding" in multi-threaded or asynchronous environments.
*   **Opaque Error Messages:** Returning "User Not Found" when the identity resolution fails, which can leak information about the existence of users.

## Edge Cases
*   **Anonymous/Guest Users:** How the system represents a requester who has no valid credentials but is allowed to access public resources.
*   **Service-to-Service Identity:** Scenarios where there is no "human" user, and the "current user" is actually a machine or application principal.
*   **Token Expiration during Long-Running Requests:** Handling cases where a user's identity is valid at the start of a request but expires before the response is sent.
*   **Identity Deletion:** Handling active sessions or valid tokens for a user who has been deleted or suspended in the primary identity store.

## Related Topics
*   **010 API Authentication:** The methods used to prove identity.
*   **011 API Authorization:** The methods used to determine what an identified user can do.
*   **025 Audit Logging:** Recording actions taken by the identified user.
*   **042 Rate Limiting:** Applying limits based on the identified principal.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |