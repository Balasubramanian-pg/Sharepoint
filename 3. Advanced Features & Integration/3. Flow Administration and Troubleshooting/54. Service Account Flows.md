# 054 Service Account Flows

Canonical documentation for 054 Service Account Flows. This document defines concepts, terminology, and standard usage.

## Purpose
The 054 Service Account Flows define the mechanisms by which non-human entities—such as applications, background processes, or automated scripts—authenticate and authorize themselves to access protected resources. Unlike user-centric flows that require interactive login (e.g., username/password or MFA), service account flows are designed for autonomous, machine-to-machine (M2M) communication. 

The primary objective of these flows is to provide a secure, scalable, and auditable method for software components to interact without human intervention, ensuring that the principle of least privilege is maintained across distributed systems.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Authentication Mechanisms:** Methods for verifying the identity of a service principal.
* **Authorization Delegation:** How permissions are assigned to and exercised by service accounts.
* **Lifecycle Management:** The theoretical stages of a service account from creation to revocation.
* **Security Constraints:** Standard requirements for securing machine identities.

**Out of scope:**
* **Specific vendor implementations:** Detailed guides for AWS IAM, Google Cloud Service Accounts, or Azure Service Principals.
* **User Authentication:** Interactive flows such as Authorization Code or Implicit flows.
* **Hardware-level identity:** Low-level TPM or hardware-specific attestation details, unless used as a credential source.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Service Account** | A non-human identity used by an application or service to interact with other services or resources. |
| **Service Principal** | The security context or "identity" of a service account within a specific security domain. |
| **Client Credentials** | A set of security attributes (e.g., Client ID and Secret) used by a service to prove its identity. |
| **Bearer Token** | A security token that grants access to the holder without requiring further proof of identity upon presentation. |
| **Workload Identity** | A method of assigning an identity to a running process based on its environment (e.g., a container or VM) rather than a static secret. |
| **Scope** | A mechanism to limit the amount of access granted to a service account. |
| **Audience (aud)** | A field within a token that identifies the intended recipients of the token. |

## Core Concepts
The 054 Service Account Flows are built upon three fundamental pillars:

1.  **Non-Interactive Authentication:** The flow must be capable of execution without a browser or human input. It relies on pre-shared keys, certificates, or environmental metadata.
2.  **Identity vs. Permission:** A service account represents *who* the caller is, while the attached policies or roles define *what* the caller can do. The flow is the bridge that converts identity into a usable credential (token).
3.  **Trust Relationships:** Service account flows rely on a pre-established trust between the Identity Provider (IdP) and the Resource Server. The IdP vouches for the service account, and the Resource Server trusts the IdP's assertion.

## Standard Model
The standard model for 054 Service Account Flows follows the **Client Credentials Grant** pattern. The process typically involves the following steps:

1.  **Request:** The Service Account (Client) sends a request to the Identity Provider. This request includes the Client ID, Client Secret (or certificate), and the requested Scope.
2.  **Validation:** The Identity Provider validates the credentials against its internal directory.
3.  **Issuance:** Upon successful validation, the Identity Provider issues an Access Token (usually a JWT).
4.  **Consumption:** The Service Account presents the Access Token to the Resource Server in the HTTP Authorization header.
5.  **Verification:** The Resource Server validates the token's signature, expiration, and audience before granting access to the requested resource.

## Common Patterns
*   **Secret-Based Flow:** The most common pattern where a Client ID and a long-lived Client Secret are used. Best suited for secure, server-side environments.
*   **Certificate-Based Flow (MTLS):** Uses X.509 certificates for authentication. This is more secure than secrets as it leverages public-key cryptography and is resistant to interception.
*   **Workload Identity Federation:** A modern pattern where a service account "exchanges" a token from its local environment (e.g., a Kubernetes ServiceAccount token) for a token from a central Identity Provider. This eliminates the need for static secrets.
*   **Assertion-Based Flow:** The service account creates a self-signed JWT (assertion) and signs it with a private key. The IdP verifies the assertion using the registered public key.

## Anti-Patterns
*   **Hardcoding Credentials:** Storing Client Secrets in source code or unencrypted configuration files.
*   **Shared Service Accounts:** Using a single service account for multiple distinct applications, making auditing and revocation impossible.
*   **Over-Privileged Accounts:** Granting "Admin" or "Owner" roles to a service account that only needs read access to a specific database.
*   **Using User Accounts for Services:** Creating a "dummy" human user account to run automated tasks, which bypasses service-specific security controls and often violates MFA policies.
*   **Infinite Token Lifetimes:** Issuing tokens that do not expire, increasing the window of opportunity for an attacker if a token is leaked.

## Edge Cases
*   **Clock Skew:** Discrepancies between the system clocks of the IdP, the Client, and the Resource Server can cause valid tokens to be rejected as "not yet valid" or "expired."
*   **Token Revocation:** Unlike user sessions, service account tokens are often stateless (JWTs). Revoking access before a token expires requires a blocklist or a short Time-To-Live (TTL).
*   **Transitive Identity:** When Service A calls Service B, which then calls Service C. The flow must decide whether Service B uses its own identity or "impersonates" Service A.
*   **Credential Rotation:** Updating secrets or certificates without causing downtime requires the system to support "grace periods" where both old and new credentials are valid.

## Related Topics
*   **012 Identity Federation:** The mechanism for linking identities across different trust domains.
*   **088 Token Exchange:** The process of swapping one token for another with different scopes or audiences.
*   **104 Least Privilege Architecture:** The security principle governing how permissions should be assigned to service accounts.
*   **RFC 6749 (The OAuth 2.0 Authorization Framework):** The foundational specification for the Client Credentials Grant.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |