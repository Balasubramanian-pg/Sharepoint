# [001 Introduction to REST Principles](4. Development & Deployment/REST API Mastery/1. Fundamentals and Connectivity/001 Introduction to REST Principles.md)

Canonical documentation for [001 Introduction to REST Principles](4. Development & Deployment/REST API Mastery/1. Fundamentals and Connectivity/001 Introduction to REST Principles.md). This document defines concepts, terminology, and standard usage.

## Purpose
The Representational State Transfer (REST) architectural style was developed to provide a standardized framework for distributed hypermedia systems. Its primary purpose is to enable decoupling between clients and servers, ensuring that systems can scale, evolve, and remain interoperable over long periods. By adhering to a specific set of constraints, REST minimizes latency and network communication while maximizing the independence of individual components.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative. While REST is most commonly implemented via HTTP, the principles defined herein apply to the architectural style itself, regardless of the underlying protocol.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The six fundamental architectural constraints of REST.
* The relationship between resources, representations, and state.
* Theoretical boundaries of Resource-Oriented Architecture (ROA).
* Standardized interaction semantics.

**Out of scope:**
* Specific vendor implementations (e.g., AWS API Gateway, Azure API Management).
* Programming language-specific libraries (e.g., Express.js, Spring Boot).
* Detailed specifications of the HTTP protocol (except where they intersect with REST principles).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Resource** | Any concept that can be named and identified, such as a document, an image, or a collection of other objects. |
| **Representation** | A specific capture of the state of a resource at a given point in time, encoded in a format such as JSON, XML, or HTML. |
| **Identifier (URI)** | A Uniform Resource Identifier that uniquely identifies a specific resource within the system. |
| **State** | The data and metadata associated with a resource or the progress of a client’s interaction with the application. |
| **Statelessness** | A constraint where the server does not store any client context between requests; each request must contain all information necessary to understand it. |
| **Idempotency** | A property of an operation where multiple identical requests have the same effect as a single request. |
| **HATEOAS** | Hypermedia As The Engine Of Application State; a constraint stating that the client interacts with the application entirely through hypermedia provided dynamically by the server. |

## Core Concepts

### The Six Constraints
To be considered "RESTful," an architecture must adhere to the following constraints:

1.  **Client-Server Architecture:** Separation of concerns between the user interface (client) and data storage (server). This allows components to evolve independently.
2.  **Statelessness:** No client session state is stored on the server. The responsibility for maintaining session state lies entirely with the client.
3.  **Cacheability:** Responses must define themselves as cacheable or non-cacheable to prevent clients from reusing stale or inappropriate data.
4.  **Layered System:** A client cannot ordinarily tell whether it is connected directly to the end server or to an intermediary (e.g., a proxy or load balancer).
5.  **Code on Demand (Optional):** Servers can temporarily extend or customize the functionality of a client by transferring executable code (e.g., JavaScript).
6.  **Uniform Interface:** This is the central feature of REST. It requires a consistent way to interact with the server, regardless of the resource type.

### The Uniform Interface Sub-Constraints
The Uniform Interface is further defined by four pillars:
*   **Identification of Resources:** Resources are identified in requests (e.g., via URIs).
*   **Manipulation of Resources through Representations:** When a client holds a representation of a resource, it has enough information to modify or delete the resource on the server (provided permissions allow).
*   **Self-descriptive Messages:** Each message includes enough information to describe how to process the message (e.g., Media Types).
*   **Hypermedia as the Engine of Application State (HATEOAS):** Clients discover available actions and resources through links provided in the server's responses.

## Standard Model

### Resource-Oriented Architecture (ROA)
The standard model for REST is the Resource-Oriented Architecture. In this model:
*   **Everything is a Resource:** Every significant entity is modeled as a resource.
*   **Unique Identifiers:** Every resource is reachable through a unique URI.
*   **Standard Methods:** Operations on resources are limited to a predefined set of "Uniform" methods (in HTTP: GET, POST, PUT, DELETE, etc.).

### State vs. Representation
A resource is an abstract concept. A **Representation** is how that resource is expressed (e.g., a "User" resource might be represented as a JSON object). The **State** refers to the actual data values within that resource at a specific moment.

## Common Patterns

### CRUD Mapping
While REST is not strictly CRUD (Create, Read, Update, Delete), it is commonly mapped to these operations:
*   **Create:** POST to a collection URI.
*   **Read:** GET a specific resource or collection URI.
*   **Update:** PUT (replace) or PATCH (partial update) a specific resource URI.
*   **Delete:** DELETE a specific resource URI.

### Collection and Element URIs
*   `https://api.example.com/v1/users` (Collection)
*   `https://api.example.com/v1/users/123` (Element)

## Anti-Patterns

*   **Tunneling:** Using a single method (usually POST) to perform all operations, effectively ignoring the Uniform Interface (e.g., SOAP-style over HTTP).
*   **Stateful Servers:** Storing session data (like user login state) in server memory, which violates the Statelessness constraint and hinders scalability.
*   **Breaking HATEOAS:** Hardcoding URIs in the client rather than following links provided by the server.
*   **Using Verbs in URIs:** Including actions in the URI (e.g., `/getUser` or `/deleteUser`) instead of using the standard method and a noun-based URI.

## Edge Cases

### Long-Running Tasks
REST is inherently synchronous in its request/response cycle. For tasks that take significant time, the standard pattern is to return a `202 Accepted` status with a link to a "Task" or "Job" resource that the client can poll for completion.

### Large Payload Pagination
When a resource collection is too large to return in a single representation, the server should implement pagination. This is often handled via query parameters (e.g., `?limit=10&offset=20`) and hypermedia links (`next`, `prev`) to maintain the HATEOAS constraint.

### Over-fetching and Under-fetching
Standard REST can lead to over-fetching (getting more data than needed) or under-fetching (needing multiple requests to get related data). While patterns like "Field Filtering" exist, this is the primary problem space addressed by alternative styles like GraphQL.

## Related Topics
*   **002 HTTP Status Codes and Semantics:** Detailed usage of protocol-level responses.
*   **003 Resource Naming Conventions:** Best practices for URI design.
*   **004 Media Type Negotiation:** How clients and servers agree on representation formats.
*   **005 HATEOAS Implementation Strategies:** Deep dive into hypermedia controls.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |