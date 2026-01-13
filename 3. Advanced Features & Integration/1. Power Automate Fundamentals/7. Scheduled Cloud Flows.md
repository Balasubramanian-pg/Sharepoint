# 007 Scheduled Cloud Flows

Canonical documentation for 007 Scheduled Cloud Flows. This document defines concepts, terminology, and standard usage.

## Purpose
Scheduled Cloud Flows exist to provide temporal automation within distributed systems. They address the requirement for tasks that must execute based on a predefined chronological cadence rather than in response to an external event or user interaction. This allows for the automation of maintenance, reporting, data synchronization, and periodic state checks without manual intervention.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Core logic of time-based recurrence and scheduling.
* Theoretical boundaries of periodic execution.
* Management of execution windows and concurrency.
* Temporal consistency and time zone handling.

**Out of scope:**
* Specific vendor implementations (e.g., Power Automate, Azure Logic Apps, AWS Step Functions).
* Event-driven or manual trigger mechanisms.
* UI-specific configuration steps.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Recurrence** | The rule-based definition of how often and at what specific times a flow should execute. |
| **Interval** | The numeric value representing the gap between executions (e.g., "every 5"). |
| **Frequency** | The unit of time applied to the interval (e.g., "Minutes", "Days", "Months"). |
| **Idempotency** | The property of a flow where multiple executions produce the same result without unintended side effects. |
| **Jitter** | Small, intentional variations in the start time to prevent "thundering herd" problems on shared resources. |
| **Backlog** | A queue of missed executions that may occur if the system is offline during a scheduled window. |
| **Concurrency Control** | The mechanism that limits how many instances of a scheduled flow can run simultaneously. |

## Core Concepts

### Temporal Triggers
Scheduled flows rely on a central orchestrator's clock to initiate execution. Unlike event-driven flows, the "trigger" is the passage of time. The system evaluates the current timestamp against the defined recurrence pattern to determine if an execution instance should be instantiated.

### Statelessness and Statefulness
By default, scheduled flows are stateless; each execution is independent of the previous one. However, many implementations require "state awareness" (e.g., knowing which records were processed in the last run). This typically requires an external persistence layer (database or log) to track the "Last Run Date."

### Time Zone Sensitivity
Scheduling is inherently tied to a temporal reference frame. Flows may be scheduled based on Coordinated Universal Time (UTC) or local time zones. Local time zone scheduling must account for Daylight Saving Time (DST) shifts, which can result in "skipped" or "double" hours.

## Standard Model

The standard model for a Scheduled Cloud Flow follows a four-stage lifecycle:

1.  **Evaluation:** The scheduler compares the current system time against the recurrence rule.
2.  **Instantiation:** If the criteria are met, a new flow instance is created.
3.  **Execution:** The flow performs its defined logic (e.g., Fetch -> Process -> Update).
4.  **Finalization:** The flow logs its completion status and the scheduler calculates the next occurrence.

## Common Patterns

### The "Delta" Pattern (Incremental Processing)
The flow retrieves only the data that has changed since the last successful execution. This is achieved by storing the `LastRunTimestamp` and using it as a filter in the data retrieval step.

### The "Batch Processor"
Used for high-volume data operations. The flow triggers at a low-traffic time (e.g., 02:00 AM) to process a large queue of items accumulated during business hours, reducing the load on live systems.

### The "Heartbeat" or "Watchdog"
A high-frequency scheduled flow (e.g., every 5 minutes) that checks the health of another system or service. If the service is unresponsive, the flow triggers an alert or recovery action.

### The "Cleanup" Pattern
A periodic flow designed to delete temporary files, expire old sessions, or archive logs to maintain system performance and storage limits.

## Anti-Patterns

### Overlapping Executions
Scheduling a flow to run more frequently than its average execution time without concurrency controls. This can lead to resource exhaustion and race conditions.

### Hardcoding Time Offsets
Calculating time logic based on a fixed offset from UTC (e.g., UTC+5) without accounting for seasonal changes (DST). This leads to flows running an hour early or late half the year.

### The "Monolithic" Scheduled Flow
Attempting to process an entire global dataset in a single scheduled run. This increases the risk of timeouts and makes error recovery difficult.

### Lack of Idempotency
Designing a flow that, if run twice due to a system error, duplicates data or performs an action twice (e.g., sending the same invoice twice).

## Edge Cases

### The "2:00 AM" Problem (DST)
In regions with DST, the clock may jump from 1:59 AM to 3:00 AM (skipping an hour) or fall back from 2:00 AM to 1:00 AM (repeating an hour). Scheduled flows set to run during these windows must have defined behavior (e.g., skip, run once, or run twice).

### Leap Years
Flows scheduled for the 29th of the month must have logic to handle February in non-leap years (usually by defaulting to the 28th or the 1st of the next month).

### System Downtime/Backlog Processing
If the scheduling engine is offline during a scheduled window, the system must decide whether to "fire" all missed instances immediately upon recovery (bursting) or simply wait for the next scheduled window.

### Long-Running Instances
If a flow instance is still running when the next scheduled instance is triggered, the system must either queue the new instance, run it in parallel, or skip it based on the defined concurrency policy.

## Related Topics
* **008 Event-Driven Architectures:** Comparison between time-based and event-based triggers.
* **015 Error Handling and Retries:** How scheduled flows handle transient failures.
* **022 Data Consistency Models:** Ensuring data integrity during batch processing.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |