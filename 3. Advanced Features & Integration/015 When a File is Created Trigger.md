# 015 When a File is Created Trigger

Canonical documentation for 015 When a File is Created Trigger. This document defines concepts, terminology, and standard usage.

## Purpose
The **015 When a File is Created Trigger** exists to facilitate event-driven architectures by providing a reactive mechanism for file system changes. It addresses the inefficiency of polling-based systems by allowing an orchestrator or consumer to remain idle until a new data object is instantiated within a monitored storage scope. This trigger serves as the entry point for automated workflows, data pipelines, and synchronization processes.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The logical event of file instantiation within a defined namespace.
* Metadata requirements for trigger payloads.
* State transitions from "non-existent" to "created."
* Theoretical constraints of event delivery (latency, ordering, and reliability).

**Out of scope:**
* Specific vendor API syntaxes (e.g., AWS S3 Event Bridge, Azure Grid, or local OS file watchers).
* Physical hardware storage mechanics.
* File content parsing or internal data structures.

## Definitions
| Term | Definition |
|------|------------|
| **Trigger** | A declarative statement that initiates an action when a specific event condition is met. |
| **Event Payload** | The packet of data emitted by the trigger, typically containing metadata about the created file. |
| **Namespace** | The logical boundary or directory structure being monitored for new files. |
| **Atomic Write** | A file operation that ensures a file is not "visible" to the trigger until the write process is entirely complete. |
| **Idempotency** | The property of a system where the same event can be processed multiple times without changing the result beyond the initial application. |
| **Debouncing** | A mechanism to prevent multiple trigger firings for a single logical creation event that involves multiple sub-steps. |

## Core Concepts
### Event-Driven Activation
The trigger operates on the principle of "Push" rather than "Pull." Instead of a consumer checking a directory at intervals, the storage provider emits a signal the moment a file enters the system.

### Metadata vs. Content
The trigger is primarily concerned with the **existence** and **attributes** of the file (e.g., name, size, timestamp, path) rather than the data contained within the file. The payload should provide enough information for a downstream consumer to retrieve the content if necessary.

### Persistence State
A "Created" event implies a state change where a unique identifier (URI or Path) that did not previously exist—or was previously deleted—now points to a valid data object.

## Standard Model
The standard model for a File Created Trigger follows a three-stage lifecycle:

1.  **Detection:** The underlying storage system identifies a new entry in its file allocation table or object index.
2.  **Validation:** The system ensures the file meets "readiness" criteria (e.g., the file handle is closed, or the upload checksum is verified).
3.  **Dispatch:** An asynchronous message is sent to the subscriber/orchestrator containing the event payload.

### Minimum Required Payload
A canonical trigger payload must include:
*   `event_id`: A unique identifier for the trigger instance.
*   `timestamp`: The UTC time the creation was finalized.
*   `file_path`: The absolute reference to the file.
*   `file_size`: The size in bytes at the time of creation.

## Common Patterns
*   **The Gatekeeper Pattern:** Using the trigger to initiate a virus scan or schema validation before allowing the file to move to a "Production" namespace.
*   **Fan-out:** A single file creation event triggering multiple independent downstream processes (e.g., generating a thumbnail, extracting metadata, and notifying a user).
*   **Claim Check:** The trigger passes a reference (URL) to the file rather than the file itself, allowing the consumer to pull the data only when resources are available.

## Anti-Patterns
*   **Polling-in-Trigger:** Designing a trigger that requires the consumer to "check back" to see if the file is actually finished. The trigger should only fire when the file is ready.
*   **Infinite Loops:** Configuring a trigger to monitor a folder, then having the resulting action save a new file (or a modified version) back into the same monitored folder without strict filters.
*   **Heavy Processing in the Trigger Thread:** Executing long-running compute tasks within the trigger's immediate execution context, which can lead to backpressure or missed events.

## Edge Cases
*   **Zero-Byte Files:** Systems must define whether an empty file constitutes a "creation" event. Some integrations ignore zero-byte files, while others treat them as valid signals.
*   **Atomic Renames:** Moving a file from `Folder A` to `Folder B`. From the perspective of `Folder B`, this is a "Created" event, even though the file existed elsewhere previously.
*   **Rapid Overwrites:** If a file is created and then immediately overwritten, the system may emit one or two events depending on the storage layer's consistency model (Eventual vs. Strong consistency).
*   **Multipart Uploads:** In cloud storage, a file may be uploaded in chunks. The trigger must only fire upon the final "Complete Multipart Upload" signal to avoid processing partial data.

## Related Topics
*   **016 When a File is Modified Trigger:** Distinguishing between new instantiation and updates to existing objects.
*   **Event-Driven Architecture (EDA):** The broader architectural style utilizing these triggers.
*   **Idempotent Consumer Pattern:** Ensuring that if a trigger fires twice for the same file, the system remains stable.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |