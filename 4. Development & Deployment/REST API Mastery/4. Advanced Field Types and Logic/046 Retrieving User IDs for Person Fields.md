# [046 Retrieving User IDs for Person Fields](4. Development & Deployment/REST API Mastery/4. Advanced Field Types and Logic/046 Retrieving User IDs for Person Fields.md)

Canonical documentation for [046 Retrieving User IDs for Person Fields](4. Development & Deployment/REST API Mastery/4. Advanced Field Types and Logic/046 Retrieving User IDs for Person Fields.md). This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of retrieving User IDs for Person Fields is to bridge the gap between human-readable identity (names, emails) and machine-readable data structures. In modern data systems, "Person" fields are relational pointers rather than static text strings. To perform operations such as filtering, updating, or auditing records associated with an individual, a system must resolve a provided identity string into a unique, immutable identifier recognized by the host environment's identity provider.

This process ensures referential integrity and prevents data ambiguity caused by duplicate names or changing user attributes (e.g., a surname change after marriage).

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The logic of identity resolution and mapping.
* The relationship between User Profiles and Data Records.
* Theoretical frameworks for lookup mechanisms.
* Validation requirements for identity inputs.

**Out of scope:**
* Specific vendor API syntax (e.g., SharePoint REST, Salesforce SOQL, Microsoft Graph).
* Authentication protocols (OAuth2, SAML).
* User provisioning or lifecycle management.

## Definitions
| Term | Definition |
|------|------------|
| **Person Field** | A specialized data type designed to store a reference to a user profile within an identity directory. |
| **User ID** | A unique, often numeric or GUID-based, identifier assigned to a specific principal within a system. |
| **Identity Resolution** | The process of matching an input (such as an email or login name) to a specific User ID in the system's directory. |
| **Principal** | An entity that can be authenticated (e.g., a user, a service account, or a security group). |
| **Lookup** | The action of querying a directory or user information list to retrieve metadata associated with a specific identity. |

## Core Concepts

### Identity as a Reference
In a normalized database or application environment, a Person Field does not store the user's name. Instead, it stores a foreign key (the User ID). The system then performs a "join" or "lookup" at runtime to display the user's current name or profile picture. This ensures that if a user's profile is updated, all records associated with them reflect the change automatically.

### The Resolution Lifecycle
1.  **Input Acquisition:** A human-readable identifier (Email, UPN, or Display Name) is provided.
2.  **Query Execution:** The system searches the Identity Provider (IdP) or the local User Information List (UIL).
3.  **Validation:** The system confirms a single, active match exists.
4.  **ID Extraction:** The unique identifier is retrieved and cached or committed to the target field.

## Standard Model
The standard model for retrieving User IDs follows a **Resolution-Before-Persistence** workflow:

1.  **Source Discovery:** Determine if the user exists in the local application context.
2.  **Directory Fallback:** If the user is not found locally, query the global directory (e.g., Active Directory, LDAP).
3.  **Hydration:** If the user is found in the global directory but not locally, "hydrate" or "ensure" the user into the local context to generate a local-scoped ID.
4.  **Assignment:** Use the resulting ID for all subsequent data operations.

## Common Patterns

### Email-Based Resolution
Using a unique email address as the search key. This is the most common pattern due to the global uniqueness of email addresses within a single tenant.

### UPN (User Principal Name) Matching
Matching based on the user's sign-in identity. This is more robust than email in environments where a user may have multiple alias email addresses but only one primary login identity.

### EnsureUser Pattern
A proactive pattern where the system attempts to find a user and, if they do not exist in the local site/application context, automatically adds them and returns the newly created local ID.

## Anti-Patterns

### Using Display Names as Keys
Attempting to retrieve a User ID based on a string like "John Smith." This leads to collisions in organizations with multiple employees of the same name and fails if the name is formatted differently (e.g., "Smith, John").

### Hardcoding IDs
Storing numeric or GUID IDs directly in code or configuration. User IDs are often environment-specific; an ID in a "Development" environment rarely matches the ID for the same user in "Production."

### Excessive Polling
Querying the identity provider for every single row in a large dataset. This leads to rate-limiting and performance degradation.

## Edge Cases

### Deleted or Deactivated Users
When a user leaves an organization, their ID may remain in the system for historical records, but they may no longer be discoverable via standard search. Systems must decide whether to retain the "Ghost ID" or map it to a "System/Legacy User."

### Guest and External Users
Users from outside the primary directory often have complex identifiers (e.g., `user_example.com#EXT#@tenant.onmicrosoft.com`). Retrieval logic must account for these non-standard string formats.

### Service Principals and Bots
Automated accounts may not have standard attributes like "First Name" or "Last Name," causing resolution logic designed for humans to fail.

### Duplicate Identities
In merged organizations, a single person might have two accounts. The retrieval logic must have a tie-breaking mechanism or a way to identify the "Primary" identity.

## Related Topics
* **012 Data Normalization:** The theory of reducing redundancy by using IDs instead of strings.
* **088 Identity Provider Integration:** How applications connect to external directories.
* **104 Referential Integrity:** Maintaining consistent relationships between tables.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |