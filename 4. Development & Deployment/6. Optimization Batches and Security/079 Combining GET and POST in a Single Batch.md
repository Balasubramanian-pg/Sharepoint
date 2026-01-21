# [079 Combining GET and POST in a Single Batch](4. Development & Deployment/REST API Mastery/6. Optimization Batches and Security/079 Combining GET and POST in a Single Batch.md)

Canonical documentation for [079 Combining GET and POST in a Single Batch](4. Development & Deployment/REST API Mastery/6. Optimization Batches and Security/079 Combining GET and POST in a Single Batch.md). This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of combining GET and POST operations into a single batch is to optimize network utilization and reduce latency by minimizing the number of round-trips between a client and a server. In complex distributed systems, a client often needs to retrieve data (GET) and modify state (POST) as part of a single logical workflow. By encapsulating these disparate HTTP methods into a single request envelope, systems can ensure better performance over high-latency networks and provide a mechanism for atomic or coordinated execution.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The conceptual framework for request encapsulation.
* Logic governing the execution order of mixed-method batches.
* Error handling strategies for heterogeneous operation sets.
* Security considerations for multi-method payloads.

**Out of scope:**
* Specific vendor implementations (e.g., OData, GraphQL, or AWS AppSync specifics).
* Programming language-specific client libraries.
* Low-level TCP/IP or HTTP/2 multiplexing details.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Batch Request** | A single network request that encapsulates multiple discrete operations (sub-requests). |
| **Sub-request** | An individual operation within a batch, containing its own method (GET, POST, etc.), URI, and payload. |
| **Envelope** | The outer wrapper or container format (usually JSON or Multipart) used to transport the batch. |
| **Atomicity** | The property ensuring that either all sub-requests in a batch succeed or none are applied. |
| **Idempotency** | The property where an operation can be applied multiple times without changing the result beyond the initial application. |
| **Response Mapping** | The mechanism used to correlate specific sub-responses in the batch output to their corresponding sub-requests. |

## Core Concepts
### 1. Request Encapsulation
Since standard HTTP/1.1 does not natively support multiple methods in a single start-line, combined batches utilize an "Envelope Pattern." The outer request is typically a `POST` (as it carries a body), while the inner sub-requests retain their logical methods (GET for retrieval, POST for creation/action).

### 2. Execution Semantics
When mixing GET and POST, the system must define the execution model:
*   **Sequential:** Sub-requests are processed in the order they appear in the array.
*   **Parallel:** Sub-requests are processed simultaneously, assuming no inter-dependencies.
*   **Transactional:** The entire batch is treated as a single unit of work.

### 3. Response Aggregation
The server must return a composite response. Each element in the response collection must include a status code and a reference (ID or index) to the original sub-request to allow the client to disambiguate the results.

## Standard Model
The standard model for combining GET and POST operations follows a structured JSON or Multipart/Mixed format.

1.  **The Outer Request:** A `POST` request to a dedicated `/batch` or `/$batch` endpoint.
2.  **The Payload:** An array of objects, where each object specifies:
    *   `method`: The HTTP verb (GET, POST, PUT, DELETE).
    *   `url`: The resource path.
    *   `headers`: Operation-specific headers.
    *   `body`: The data payload (for POST/PUT).
    *   `id`: A unique identifier for correlation.
3.  **The Processing Engine:**
    *   Validates the envelope.
    *   Dispatches sub-requests to internal handlers.
    *   Collects results.
4.  **The Outer Response:** A `200 OK` or `207 Multi-Status` containing an array of sub-responses.

## Common Patterns
### The "Read-After-Write" Pattern
A client sends a POST to create a resource and a GET to retrieve a related, updated view or a different resource in the same call. This ensures the client has the latest state without a second network hop.

### The "Dependent Batch"
The result of a POST (e.g., a new ID) is required for a subsequent GET in the same batch. This requires the server to support "Content-ID" referencing or variable substitution within the batch.

### Independent Multiplexing
Multiple unrelated GETs and POSTs are bundled simply to save on connection overhead. These are typically executed in parallel by the server to minimize total processing time.

## Anti-Patterns
*   **Method Overloading:** Using a GET method for the outer envelope. GET requests should not have bodies; using them for batches violates HTTP standards.
*   **Ignoring Partial Failures:** Returning a global `200 OK` when some sub-requests failed without providing individual status codes for each sub-request.
*   **Unbounded Batch Size:** Allowing an unlimited number of sub-requests in a single batch, which can lead to timeouts, memory exhaustion, or "Head-of-Line" blocking at the application level.
*   **Mixing Side-Effects without Order:** Executing a GET that expects data modified by a POST in the same batch without enforcing sequential execution.

## Edge Cases
*   **Authentication Divergence:** A batch where the user has permission for the GET sub-request but lacks permission for the POST sub-request. The system must handle partial authorization.
*   **Caching:** GET results within a batch are generally not cacheable by standard intermediate proxies because they are wrapped inside a POST envelope.
*   **Timeouts:** If a POST operation within a batch takes significant time, it may cause the GET operations to time out, even if the GETs were processed quickly.
*   **Redirection:** Handling a `3xx` redirect for a single sub-request within a batch is complex and usually requires the server to follow the redirect internally or return the redirect status as the sub-response.

## Related Topics
*   **042 Transactional Integrity in APIs:** For batches requiring all-or-nothing execution.
*   **115 Idempotency Keys:** Crucial for the POST components of a batch to prevent duplicate processing on retry.
*   **088 HTTP 207 Multi-Status:** The standard status code for reporting results of multiple operations.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |