# [080 Error Handling in Batch Responses](4. Development & Deployment/REST API Mastery/6. Optimization Batches and Security/080 Error Handling in Batch Responses.md)

Canonical documentation for [080 Error Handling in Batch Responses](4. Development & Deployment/REST API Mastery/6. Optimization Batches and Security/080 Error Handling in Batch Responses.md). This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Error Handling in Batch Responses is to provide a structured mechanism for communicating the outcome of multiple operations processed within a single request-response cycle. In distributed systems and high-throughput APIs, batching reduces network overhead; however, it introduces complexity in error reporting. This topic addresses the "Partial Success" problem, where some operations within a batch succeed while others fail, necessitating a granular reporting strategy that prevents data ambiguity and ensures system state consistency.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   Structural requirements for reporting multi-item outcomes.
*   Distinction between transport-level and application-level failures.
*   Consistency models for batch processing (Atomic vs. Non-atomic).
*   Standardized status mapping for individual batch elements.

**Out of scope:**
*   Specific vendor implementations (e.g., AWS SQS Batch, Salesforce Bulk API).
*   Client-side retry logic or exponential backoff algorithms.
*   Network-layer protocols (TCP/IP) underlying the batch transmission.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Batch** | A collection of discrete operations grouped into a single request to be processed by a receiver. |
| **Partial Success** | A state where at least one operation in a batch succeeds and at least one operation fails. |
| **Atomic Batch** | A batch where all operations must succeed for any to be committed; if one fails, the entire batch is rolled back. |
| **Non-atomic Batch** | A batch where operations are processed independently; failures in one item do not inherently roll back others. |
| **Envelope** | The outer wrapper of a response that contains metadata about the batch as a whole. |
| **Item-Level Status** | A specific error code or success indicator assigned to an individual element within the batch. |
| **Correlation ID** | A unique identifier used to map a specific response item back to its corresponding request item. |

## Core Concepts

### The Multi-Level Error Hierarchy
Error handling in batches operates on two distinct levels:
1.  **Transport/Envelope Level:** Indicates whether the receiver successfully accepted and parsed the batch request.
2.  **Item Level:** Indicates the specific outcome of each individual operation within the batch.

### Granularity of Feedback
A robust batch response must provide enough detail for the caller to programmatically determine which items require correction, which can be retried, and which were successfully persisted. This requires a 1:1 mapping between request items and response items.

### Idempotency in Batching
Batch error handling relies on the principle that retrying a partially failed batch (or a subset thereof) should not result in duplicate side effects for the items that previously succeeded.

## Standard Model

The standard model for batch error handling utilizes a **Summary-Detail** structure.

1.  **The Summary (Envelope):** The response should indicate the overall result. In HTTP-based systems, a `207 Multi-Status` or a `200 OK` (if the batch was accepted for processing) is common, provided the body contains the granular details.
2.  **The Detail (Item Array):** The response body must contain an array of objects. Each object should include:
    *   An identifier (e.g., `id` or `correlation_id`).
    *   A status indicator (e.g., `code` or `status`).
    *   A descriptive error message (if applicable).
    *   An optional error category (e.g., `VALIDATION_ERROR`, `SYSTEM_ERROR`).

### Example Structure (Conceptual)
```json
{
  "batch_summary": {
    "total_count": 3,
    "success_count": 2,
    "failure_count": 1
  },
  "items": [
    { "id": "item_1", "status": 201, "message": "Created" },
    { "id": "item_2", "status": 400, "error": "Invalid format" },
    { "id": "item_3", "status": 201, "message": "Created" }
  ]
}
```

## Common Patterns

### 1. All-or-Nothing (Atomic)
The system treats the batch as a single transaction. If any item fails, the response returns a single error code for the entire batch, and no changes are committed.

### 2. Best-Effort (Non-atomic)
The system attempts to process every item in the batch regardless of individual failures. The response contains a mixture of success and failure statuses.

### 3. Stop-on-First-Error
The system processes items sequentially and halts execution as soon as a failure is encountered. Items following the failure are marked as "not processed" or "skipped."

## Anti-Patterns

*   **The "Silent Failure":** Returning a global `200 OK` or success code when some or all items within the batch failed.
*   **Ambiguous Mapping:** Returning a list of errors without correlation IDs, making it impossible for the client to know which specific item caused which error.
*   **HTTP Status Misuse:** Using a `500 Internal Server Error` at the envelope level for a validation error on a single item in a non-atomic batch.
*   **Inconsistent Types:** Returning an object on success but an array on failure, complicating the client's parsing logic.

## Edge Cases

*   **Empty Batches:** A request sent with an empty array. The system should define whether this is a `200 OK` (no-op) or a `400 Bad Request`.
*   **Timeouts Mid-Batch:** If a batch process times out after processing 50% of items, the system must define if the remaining items are implicitly failed or if the state is indeterminate.
*   **Malformed Envelopes:** If the outer JSON/XML is invalid, the system cannot provide item-level feedback and must return a transport-level error.
*   **Duplicate IDs:** If a batch contains two items with the same correlation ID, the error response must clarify which item the error refers to or reject the batch as malformed.

## Related Topics
*   **042 Idempotency Keys:** Ensuring retries of failed batch items do not create duplicates.
*   **115 Rate Limiting:** How batch sizes impact system throughput and error thresholds.
*   **090 Transactional Integrity:** Deep dive into atomic vs. eventual consistency.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |