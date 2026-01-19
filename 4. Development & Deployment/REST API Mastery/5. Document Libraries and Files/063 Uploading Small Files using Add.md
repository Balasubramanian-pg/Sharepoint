# [063 Uploading Small Files using Add](4. Development & Deployment/REST API Mastery/5. Document Libraries and Files/063 Uploading Small Files using Add.md)

Canonical documentation for [063 Uploading Small Files using Add](4. Development & Deployment/REST API Mastery/5. Document Libraries and Files/063 Uploading Small Files using Add.md). This document defines concepts, terminology, and standard usage.

## Purpose
The "Add" operation for small files provides a high-efficiency, low-latency mechanism for persisting data objects within a storage system or repository. This topic addresses the need for a simplified ingestion path when the overhead of multi-part uploads, session management, or chunked transfer encoding exceeds the complexity of the data being transferred. It ensures that small-scale data transfers are handled atomically and with minimal protocol chatter.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Single-request atomic upload operations.
* Metadata-payload coupling within a single transaction.
* Constraints and thresholds defining "small" files.
* Validation and integrity checks for immediate persistence.

**Out of scope:**
* Resumable upload sessions or stateful transfers.
* Multi-part or chunked data streaming.
* Client-side compression algorithms.
* Specific vendor API implementations (e.g., AWS S3 PutObject, Google Cloud Storage Simple Upload).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Atomic Upload** | A transfer process where the file and its associated metadata are committed to the destination in a single, indivisible operation. |
| **Size Threshold** | The maximum data volume (typically measured in MiB) permitted for a single "Add" operation before a multi-part method is required. |
| **Payload** | The actual binary or text content of the file being uploaded. |
| **Metadata** | Supplemental information describing the file (e.g., content-type, creation date, permissions) sent alongside the payload. |
| **Ingestion Latency** | The time elapsed between the initiation of the request and the confirmation of persistence. |

## Core Concepts

### Atomicity
The "Add" operation is fundamentally atomic. The system must either accept the entire file and its metadata or reject the request entirely. There is no intermediate state where a file exists without its metadata or in a partially uploaded form.

### Efficiency vs. Reliability
For small files, the primary constraint is often the "Round Trip Time" (RTT) rather than bandwidth. By using a single request, the "Add" pattern minimizes the handshake overhead associated with initializing and finalizing complex upload sessions.

### Synchronous Validation
Unlike large-scale uploads that may be validated asynchronously, small file uploads typically undergo immediate validation (schema, size, and type) before the connection is closed, providing the client with an immediate success or failure signal.

## Standard Model
The standard model for 063 Uploading Small Files follows a "Single-Shot" request-response pattern:

1.  **Request Initiation:** The client constructs a single request containing the target destination, the file metadata, and the file payload.
2.  **Transport:** The payload is transmitted in the body of the request.
3.  **Server-Side Processing:**
    *   The system verifies the payload size against the **Size Threshold**.
    *   The system performs integrity checks (e.g., MD5/SHA-256 hashes).
    *   The system persists the data and metadata to the underlying storage.
4.  **Finalization:** The system returns a unique identifier or a URI for the newly created resource.

## Common Patterns

### Inline Metadata
Metadata is often included in the request headers or as a preamble to the body, allowing the storage system to categorize the file without parsing the entire payload first.

### Binary Stream Transfer
The most efficient pattern for small files is a raw binary stream where the request body is the file itself, minimizing the encoding overhead (such as Base64) which can increase payload size by approximately 33%.

### Immediate Consistency
Small file uploads typically target systems that provide "read-after-write" consistency, ensuring the file is available for retrieval the moment the "Add" operation returns a success code.

## Anti-Patterns

*   **Over-Chunking:** Using multi-part upload logic for files significantly below the Size Threshold, which increases latency and resource consumption on both client and server.
*   **Base64 Bloating:** Encoding binary files into strings within a JSON body for large "small" files, leading to unnecessary memory pressure and bandwidth waste.
*   **Lack of Content-Length:** Failing to specify the expected size of the payload, which prevents the server from rejecting oversized files early in the transmission.
*   **Polling for Completion:** Treating an atomic "Add" operation as an asynchronous task and polling for its existence rather than relying on the request's response code.

## Edge Cases

*   **Zero-Byte Files:** The system must define whether an "Add" operation for an empty file is a valid creation of a placeholder or an invalid request.
*   **Maximum Threshold Boundary:** Scenarios where a file is exactly the size of the threshold limit. Systems must be explicit about whether the limit is inclusive or exclusive.
*   **Network Interruption:** If a connection drops during a single-shot upload, the system must ensure no partial data is persisted, maintaining the integrity of the namespace.
*   **Duplicate Identifiers:** Handling "Add" requests for files that already exist (e.g., Overwrite vs. Fail vs. Version).

## Related Topics
*   **064 Multi-part Uploads:** For files exceeding the Size Threshold.
*   **082 Content Integrity and Checksumming:** Methods for verifying data during transfer.
*   **105 Metadata Schemas:** Standards for defining the supplemental data attached during an "Add" operation.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |