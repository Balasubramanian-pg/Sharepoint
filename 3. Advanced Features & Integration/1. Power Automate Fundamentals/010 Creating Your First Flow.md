# 010 Creating Your First Flow

Canonical documentation for 010 Creating Your First Flow. This document defines concepts, terminology, and standard usage.

## Purpose
The creation of an initial flow represents the foundational transition from static data or manual processes to automated, event-driven logic. This topic exists to establish the fundamental principles of workflow orchestration, ensuring that the first iteration of an automated sequence is built upon a scalable, maintainable, and logical framework. It addresses the problem of process inconsistency and manual intervention by defining how discrete tasks are sequenced to achieve a deterministic outcome.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The logical architecture of a basic workflow.
* The relationship between triggers, actions, and data flow.
* Fundamental requirements for a viable flow (initiation, execution, and termination).
* Conceptual validation of flow logic.

**Out of scope:**
* Specific vendor implementations (e.g., Power Automate, Zapier, Apache NiFi).
* Advanced error-handling frameworks (covered in separate documentation).
* Complex multi-system state synchronization.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| Flow | A defined sequence of automated steps or operations triggered by a specific event to achieve a goal. |
| Trigger | The specific event or condition that initiates the execution of a flow. |
| Action | A discrete operation or task performed within the flow (e.g., data transformation, notification, API call). |
| Payload | The data packet carried through the flow, often modified or augmented by various actions. |
| Condition | A logic gate that determines the path of the flow based on specific criteria. |
| Termination | The point at which a flow completes its execution, whether successfully or via a handled exit. |

## Core Concepts

### The Event-Driven Paradigm
Flows are fundamentally reactive. A flow does not exist in a vacuum; it responds to a change in state or a specific signal (the Trigger). Understanding that the flow is a servant to the event is critical for proper design.

### Linear vs. Non-Linear Logic
*   **Linear Logic:** A straight sequence where Step A leads to Step B, with no deviations.
*   **Non-Linear Logic:** Incorporates decision points (Conditions) where the flow can branch into different paths based on the data within the payload.

### Data Context and Mapping
As a flow progresses, each step generates output that becomes part of the "context." Creating a flow requires mapping the output of a previous step to the input of a subsequent step, ensuring data integrity throughout the lifecycle.

## Standard Model

The standard model for creating a first flow follows the **Trigger-Action-Result (TAR)** framework:

1.  **Initiation (Trigger):** Define the "When." This must be a singular, identifiable event (e.g., a timer, a webhook, or a state change in a database).
2.  **Processing (Action/Logic):** Define the "What." This involves one or more steps that manipulate data or interact with external systems.
3.  **Outcome (Result/Termination):** Define the "Goal." Every flow must have a clear end state, providing feedback or updating a system of record to signal completion.

## Common Patterns

### The Notification Pattern
The simplest flow pattern where a trigger (e.g., a form submission) leads directly to a communication action (e.g., an email or log entry).

### The Data Synchronization Pattern
A flow designed to ensure two systems remain consistent. When data is updated in System A (Trigger), the flow updates the corresponding record in System B (Action).

### The Pass/Fail Branch
A pattern where a condition checks the validity of the trigger data. If valid, the flow proceeds to the primary action; if invalid, it proceeds to an alternative "exception" path.

## Anti-Patterns

*   **The Infinite Loop:** Creating a flow where an action inadvertently triggers the same flow again (e.g., a flow that updates a record, where the update itself is the trigger).
*   **The "God" Flow:** Attempting to solve every business problem within a single flow, leading to unmanageable complexity and high failure rates.
*   **Hardcoding Values:** Embedding specific IDs or strings that should be dynamic, making the flow brittle and difficult to migrate between environments.
*   **Silent Failures:** Designing a flow that terminates without any log or notification when an action fails.

## Edge Cases

*   **Race Conditions:** When two instances of the same flow are triggered simultaneously and attempt to modify the same resource.
*   **Empty Payloads:** A trigger occurs, but the expected data is missing or null. A robust first flow should account for the existence of data before processing.
*   **Transient Failures:** Temporary unavailability of a downstream system. While advanced handling is out of scope, the initial design should consider what happens if an action cannot be completed immediately.

## Related Topics
*   **011 Intermediate Logic and Branching:** Expanding flows with complex conditional logic.
*   **020 Error Handling and Retries:** Standardizing how flows respond to failures.
*   **030 Data Transformation and Mapping:** Deep dive into manipulating payloads between steps.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |