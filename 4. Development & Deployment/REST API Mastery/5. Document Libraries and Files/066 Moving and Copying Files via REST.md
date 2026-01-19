# [066 Moving and Copying Files via REST](4. Development & Deployment/REST API Mastery/5. Document Libraries and Files/066 Moving and Copying Files via REST.md)

Canonical documentation for [066 Moving and Copying Files via REST](4. Development & Deployment/REST API Mastery/5. Document Libraries and Files/066 Moving and Copying Files via REST.md). This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of moving and copying files via REST is to manage the location and lifecycle of digital resources within a networked environment without requiring the transfer of the actual data payload between the client and the server. 

In a distributed system, resources are identified by URIs. Moving and copying operations allow for the reorganization of these resources, the creation of backups, or the promotion of assets through different stages of a workflow. By performing these operations server-side, systems minimize bandwidth consumption, reduce latency, and maintain data integrity by ensuring that the server handles the underlying storage logic.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Architectural patterns for resource relocation and duplication.
* HTTP methods and headers used for file manipulation.
* Consistency and atomicity requirements for file operations.
* Metadata handling during state transitions.

**Out of scope:**
* Specific vendor API implementations (e.g., AWS S3, Google Drive API, Azure Blob Storage).
* Client-side "Download-then-Upload" workflows.
* Physical hardware storage protocols (e.g., NVMe, SATA).
* Graphical User Interface (GUI) design for file management.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Source Resource** | The original URI identifying the file to be moved or copied. |
| **Destination Resource** | The target URI where the file will be placed or duplicated. |
| **Atomicity** | The property ensuring that a move or copy operation either completes entirely or fails without side effects. |
| **Idempotency** | The property where performing the same operation multiple times yields the same result as the first successful execution. |
| **Metadata Preservation** | The process of maintaining attributes (e.g., creation date, permissions, custom tags) during the transition. |
| **Overwrite Policy** | The logic determining whether an existing resource at the destination should be replaced, versioned, or protected. |

## Core Concepts
### Resource Identity
In a RESTful context, a file is a resource identified by a unique URI. Moving a file implies a change in its identity (URI), while copying implies the creation of a new identity with a shared state.

### Server-Side Execution
The defining characteristic of this topic is that the data movement occurs within the server's infrastructure. The client sends an instruction (a "control message"), and the server executes the logic, returning only the status of the operation.

### State Representation
A copy operation results in two independent resources that share the same initial state but may diverge thereafter. A move operation is a transformation of the resource's location, often modeled as a creation at the destination and a deletion at the source.

## Standard Model
The standard model for moving and copying files via REST typically follows one of two architectural paths:

1.  **WebDAV Extensions (RFC 4918):** This model introduces specific HTTP verbs—`COPY` and `MOVE`. The client specifies the source in the Request-URI and the destination in a `Destination` header.
2.  **Pure RESTful Mapping:** Since standard HTTP/1.1 does not include `COPY` or `MOVE`, many systems map these actions to `POST` or `PUT`.
    *   **Copy:** A `POST` request to a "collection" or a "copy service" endpoint with a payload defining the source and destination.
    *   **Move:** A combination of a copy and a delete, or a `PATCH` request to update the resource's path attribute.

## Common Patterns
### 1. The Destination Header Pattern
Commonly used in WebDAV-compliant systems.
*   **Method:** `MOVE` or `COPY`
*   **URI:** `/source/file.txt`
*   **Header:** `Destination: /target/file.txt`

### 2. The Controller Endpoint Pattern
Used when sticking strictly to `POST`.
*   **Method:** `POST`
*   **URI:** `/files/operations`
*   **Payload:** `{"action": "move", "from": "/a.txt", "to": "/b.txt"}`

### 3. The Side-Effect PUT Pattern
*   **Method:** `PUT`
*   **URI:** `/destination/file.txt`
*   **Header:** `X-Copy-From: /source/file.txt`
*   **Description:** The server creates the resource at the URI by pulling data from the source specified in the header.

## Anti-Patterns
*   **Client-Mediated Transfer:** Downloading a file from the server and uploading it back to a different path. This is inefficient and risks data corruption.
*   **Non-Atomic Moves:** Deleting the source before confirming the destination has been successfully written. This can lead to permanent data loss if the process is interrupted.
*   **Ignoring Metadata:** Failing to decide whether permissions and timestamps should be inherited from the destination folder or preserved from the source.
*   **GET for State Change:** Using a `GET` request with query parameters to trigger a move or copy (e.g., `GET /move?from=a&to=b`). This violates the safety principles of the HTTP protocol.

## Edge Cases
*   **Cross-Server Operations:** Moving a file between two different REST servers. This usually requires a "Pull" model where the destination server acts as a client to the source server.
*   **Circular References:** Attempting to copy a folder into itself, which can lead to infinite recursion if not validated by the server.
*   **Large File Timeouts:** Operations on multi-terabyte files may exceed standard HTTP timeout windows. These should be handled via **Asynchronous Patterns** (returning a `202 Accepted` with a status polling URI).
*   **Collision Handling:** When a file already exists at the destination. Standard responses include `412 Precondition Failed` (if `Overwrite: F` is set) or automatic renaming (e.g., `file(1).txt`).

## Related Topics
*   **024 Resource Versioning:** How copies interact with version history.
*   **045 Asynchronous Request-Reply:** Handling long-running file operations.
*   **088 Content Negotiation:** Ensuring the copied resource maintains its media type.
*   **RFC 4918:** HTTP Extensions for Web Distributed Authoring and Versioning (WebDAV).

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |