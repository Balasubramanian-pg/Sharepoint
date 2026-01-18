# 002 Evolution of SharePoint APIs Server side to REST

Canonical documentation for 002 Evolution of SharePoint APIs Server side to REST. This document defines concepts, terminology, and standard usage.

## Purpose
The evolution of SharePoint APIs represents a fundamental shift in enterprise software architecture: the transition from tightly coupled, monolithic server-side execution to decoupled, platform-agnostic, and web-standard communication. This topic addresses the problem of data accessibility and extensibility in a distributed environment. It explains how the interface for interacting with content moved from direct memory access on a local server to standardized HTTP requests that can be executed from any device or operating system.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative, focusing on the architectural transition rather than specific code snippets.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The architectural transition from Server-Side Object Model (SSOM) to Client-Side Object Model (CSOM) and REST.
* The conceptual differences between RPC-style calls and Resource-oriented architectures.
* The impact of cloud computing on API design.
* Theoretical boundaries of data serialization and transport protocols (SOAP vs. JSON/OData).

**Out of scope:**
* Specific syntax for C#, JavaScript, or PowerShell.
* Third-party API wrappers or non-native integration platforms.
* Detailed configuration of authentication providers (e.g., OAuth flow specifics).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| SSOM | **Server-Side Object Model:** A set of libraries that run directly on the server hosting the application, requiring direct access to the server's memory and database. |
| CSOM | **Client-Side Object Model:** A set of proxy libraries that allow remote applications to communicate with the server using a specific language-bound wrapper (.NET, JS). |
| REST | **Representational State Transfer:** An architectural style that uses standard HTTP verbs (GET, POST, PUT, DELETE) to manipulate resources identified by URLs. |
| OData | **Open Data Protocol:** A standardized protocol for building and consuming RESTful APIs, providing a uniform way to query and manipulate data sets. |
| CRUD | **Create, Read, Update, Delete:** The four basic functions of persistent storage. |
| Serialization | The process of converting an object in memory into a format (like XML or JSON) that can be transmitted over a network. |

## Core Concepts

### 1. Coupling and Execution Context
The primary driver of API evolution is the location of code execution. In the **Server-side era**, code was "on-box," meaning the logic and the data resided on the same physical or virtual infrastructure. In the **REST era**, code is "off-box," executing in a browser, a mobile app, or a separate microservice, communicating via a network layer.

### 2. Abstraction Layers
As APIs evolved, the level of abstraction increased. 
* **Direct Access:** Accessing the database or local DLLs.
* **Proxy Access:** Using a library that mimics local objects but handles network communication under the hood (CSOM).
* **Interface Access:** Interacting with a standardized endpoint where the underlying implementation is completely hidden (REST).

### 3. From Statefulness to Statelessness
Server-side APIs often relied on the server maintaining a session state for the user. RESTful APIs are inherently stateless; every request contains all the information necessary to understand and process that request, improving scalability and reliability in cloud environments.

## Standard Model

The evolution follows a four-stage progression:

1.  **The Monolithic Phase (SSOM):** High performance but high risk. Code has "full trust" and can potentially crash the entire server farm. It is restricted to the Windows/C# ecosystem.
2.  **The Remote Procedure Phase (SOAP/Web Services):** The first attempt at remote access using XML-based SOAP services. It allowed off-box communication but was verbose, difficult to parse, and lacked flexibility.
3.  **The Proxy Phase (CSOM):** Introduced to bridge the gap for developers moving away from the server. It provided a familiar object-oriented experience while using an efficient "batching" mechanism to reduce network round-trips.
4.  **The Resource Phase (REST/OData):** The current standard. It treats every list, folder, and item as a URL-addressable resource. It uses JSON for lightweight data transfer and is compatible with every modern programming language.

## Common Patterns

### The Batching Pattern
To overcome the latency of network calls compared to local memory access, modern APIs utilize batching. Instead of sending ten requests for ten items, the client wraps these into a single HTTP request, and the server returns a single multipart response.

### The Discovery Pattern
Modern REST implementations often use "HATEOAS" (Hypermedia as the Engine of Application State) or metadata endpoints (like `$metadata` in OData) to allow clients to discover available resources and actions dynamically without hardcoding URLs.

### The Expansion Pattern
Using query parameters (e.g., `$expand`) to retrieve related data in a single call, mimicking "Joins" in a relational database while maintaining a resource-oriented structure.

## Anti-Patterns

*   **Chatty Interfaces:** Making multiple small REST calls to perform a single logical operation, leading to significant network overhead.
*   **Over-fetching:** Requesting an entire object (all columns/fields) when only one or two are needed. This wastes bandwidth and server resources.
*   **Tunneling:** Using a `POST` request for everything, effectively ignoring the semantic meaning of HTTP verbs (GET for retrieval, DELETE for removal, etc.).
*   **Hardcoding Endpoints:** Assuming a specific URL structure instead of using the API's discovery or relative path features.

## Edge Cases

*   **Large List Thresholds:** REST APIs often encounter "throttling" or "threshold" limits when querying very large datasets that were previously accessible via direct SQL-backed server-side queries.
*   **Binary Data Transfer:** Handling large file uploads/downloads via REST requires specific handling (chunking) compared to the streamlined stream-handling available in server-side code.
*   **Complex Permissions:** Some legacy permission structures do not map cleanly to RESTful resource URIs, occasionally requiring specialized "utility" endpoints that break the pure REST model.

## Related Topics

*   **001 Authentication Standards:** The shift from NTLM/Kerberos to OAuth2 and OpenID Connect.
*   **003 Client-Side Rendering (CSR):** How the move to REST enabled modern front-end frameworks.
*   **004 Microservices Architecture:** The broader industry trend of which this evolution is a part.
*   **OData Protocol Specification:** The underlying standard for the REST implementation.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |