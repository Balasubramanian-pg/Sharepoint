# 016 When a File is Modified Trigger

Canonical documentation for 016 When a File is Modified Trigger. This document defines concepts, terminology, and standard usage.

## Purpose
The "When a File is Modified" trigger exists to bridge the gap between static data storage and reactive process automation. Its primary purpose is to monitor a specific storage location or object and initiate a downstream workflow or computational task immediately following a change to an existing file. 

This mechanism addresses the problem of latency and manual oversight in data-driven environments, ensuring that systems remain synchronized with the most current version of a digital asset without requiring constant human intervention or inefficient, high-frequency manual polling.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logic and mechanics of modification detection (Push vs. Pull).
* State comparison methodologies (Timestamps, Checksums, Entity Tags).
* Theoretical boundaries of "modification" (Metadata vs. Content).
* Triggering conditions and filtering logic.

**Out of scope:**
* Specific vendor implementations (e.g., Power Automate, AWS S3 Event Bridge, Azure Logic Apps).
* Physical hardware interrupts or low-level kernel-space file system drivers.
* File creation or deletion events (unless they are interpreted as modifications by a specific system).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Modification Event** | A discrete signal generated when a file's state transitions from State A to State B. |
| **Polling** | A pull-based mechanism where the monitoring system periodically queries the storage provider for changes. |
| **Webhook/Callback** | A push-based mechanism where the storage provider notifies the monitoring system immediately upon a change. |
| **Idempotency** | The property of a trigger ensuring that the same modification event does not result in multiple unintended executions. |
| **Metadata** | Data providing information about the file (e.g., size, owner, last modified date) rather than the file's actual content. |
| **Checksum/Hash** | A mathematical value representing the file's content, used to verify if the internal data has changed regardless of metadata. |
| **Debouncing** | The practice of delaying trigger execution to ensure a series of rapid modifications are treated as a single event. |

## Core Concepts

### Detection Methodologies
There are two primary ways a system identifies a modification:
1.  **Event-Driven (Push):** The storage system natively supports event notifications. When a write operation completes, the system broadcasts an event. This is generally preferred for low latency.
2.  **State Comparison (Pull/Polling):** The monitoring system maintains a "last known state" (e.g., a timestamp or ETag). It periodically scans the file and compares the current state to the last known state.

### Modification Criteria
A "modification" can be defined by three distinct layers:
*   **Content Change:** The binary or text data within the file has been altered.
*   **Attribute Change:** The file content remains the same, but metadata (permissions, tags, or names) has changed.
*   **Temporal Change:** The "Last Modified" timestamp has been updated by the file system, even if no data was altered (often called a "touch").

### State Persistence
To function accurately, the trigger must persist the state of the file from the previous check. If the persistence layer is lost, the trigger may fail to identify a change or, conversely, trigger a "false positive" by treating an existing file as newly modified.

## Standard Model

The standard model for a File Modified Trigger follows a four-stage lifecycle:

1.  **Observation:** The system monitors a target file or directory.
2.  **Detection:** A change is identified via a push notification or a polling mismatch.
3.  **Validation:** The system verifies if the change meets defined criteria (e.g., "Ignore metadata-only changes" or "Only trigger if file size > 0").
4.  **Emission:** The trigger fires, passing the file's context (path, metadata, content link) to the subscriber or workflow.

## Common Patterns

### The Sync Pattern
Used to keep two disparate systems in alignment. When a file is modified in System A, the trigger initiates a process to overwrite the corresponding file in System B.

### The Processing Pipeline
A file (such as a CSV or Image) is modified/uploaded. The trigger initiates a transformation service (e.g., converting CSV to JSON or resizing an image) and saves the output elsewhere.

### The Audit/Logging Pattern
Every modification event is captured and logged to a database to maintain a historical record of who changed what and when, providing a secondary layer of version control.

## Anti-Patterns

### The Infinite Loop
Occurs when a trigger is set to monitor a file, and the resulting action modifies that same file. Without a "recursion guard," the system will trigger itself indefinitely.

### High-Frequency Polling on Large Volumes
Setting a polling interval of seconds on a directory containing thousands of files. This leads to "API Exhaustion" and significant performance degradation of both the storage and the monitoring system.

### Over-Reliance on Timestamps
Relying solely on "Last Modified" timestamps in environments where system clocks may out of sync or where "touch" commands are common. This leads to unreliable triggering.

## Edge Cases

*   **Zero-Byte Files:** Some systems create a zero-byte file as a placeholder before streaming data. A trigger may fire before the content is actually present.
*   **Atomic Renames:** In some file systems, moving a file into a monitored folder is treated as a "Modification" of the folder but not the file.
*   **Rapid-Fire Saves:** An application may save a file five times in one second (e.g., during an auto-save). Without **debouncing**, the trigger will fire five times, potentially exhausting downstream resources.
*   **Locking Mechanisms:** If a file is modified but remains "locked" by the writing process, the trigger may fire, but the subsequent action may fail because it cannot read the file.

## Related Topics
*   **015 When a File is Created Trigger:** Often confused with modification; focuses on the initial appearance of an object.
*   **Change Data Capture (CDC):** The database-level equivalent of file modification triggers.
*   **Event-Driven Architecture (EDA):** The broader architectural pattern that encompasses file triggers.
*   **File System Watchers:** The OS-level implementation of modification detection.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |