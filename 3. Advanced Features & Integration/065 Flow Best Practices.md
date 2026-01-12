# 065 Flow Best Practices

Canonical documentation for 065 Flow Best Practices. This document defines concepts, terminology, and standard usage.

## Purpose
The 065 Flow Best Practices framework exists to provide a standardized methodology for designing, implementing, and maintaining declarative automation. As business logic grows in complexity, the risk of technical debt, performance degradation, and unmaintainable "spaghetti" logic increases. This documentation establishes a rigorous set of principles to ensure that automated flows remain scalable, performant, and transparent across the enterprise lifecycle.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative. While frequently applied to low-code orchestration platforms, these principles apply to any visual logic engine.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Logical architecture and flow topology.
* Resource management and variable handling.
* Error handling and exception strategies.
* Performance optimization and bulkification principles.
* Governance and documentation standards.

**Out of scope:**
* Specific vendor-specific syntax or UI navigation.
* Hardware-level performance tuning.
* Third-party integration authentication protocols.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **065 Flow** | A classification for enterprise-grade declarative automation that adheres to strict modularity and performance standards. |
| **Bulkification** | The design pattern of ensuring logic can process multiple records simultaneously without hitting governor limits or performance bottlenecks. |
| **Deterministic Logic** | Logic that, given the same initial state and input, will always produce the same output and side effects. |
| **Fault Path** | A dedicated execution branch designed to catch and handle exceptions during an element's execution. |
| **Idempotency** | The property of an automation where multiple executions with the same parameters result in the same state without unintended side effects. |
| **Orchestrator** | A high-level flow designed solely to manage the execution order of sub-modules or sub-flows. |

## Core Concepts

### 1. Modularity and Reusability
Logic should be decomposed into the smallest functional units possible. This reduces redundancy and allows for centralized updates to shared business rules.

### 2. Declarative-First, Code-Second
The 065 standard prioritizes declarative tools for visibility and maintainability but recognizes the "Extensibility Threshold"—the point where complex algorithmic logic should be offloaded to programmatic services.

### 3. State Awareness
Flows must be designed with an awareness of the data state before and after execution. This includes understanding the difference between "Before-Save" (fast field updates) and "After-Save" (actions requiring a record ID or external impact).

### 4. Resource Economy
Variables, collections, and constants should be used judiciously. Every resource in a flow consumes memory and impacts the execution context's limits.

## Standard Model

The Standard Model for 065 Flow follows a **Tiered Architecture**:

1.  **Trigger Layer:** Detects the change or event. It should contain minimal logic, acting primarily as a filter to prevent unnecessary executions.
2.  **Orchestration Layer:** Determines the sequence of operations. It evaluates high-level conditions to decide which functional modules to invoke.
3.  **Functional Layer (Sub-flows):** Performs specific business tasks (e.g., "Calculate Tax," "Notify Stakeholders"). These are independent and context-agnostic.
4.  **Data Access Layer:** Handles all Data Manipulation Language (DML) operations. Ideally, DML is consolidated at the end of the execution path to minimize database contention.

## Common Patterns

### The Collection Processor
Instead of performing actions inside a loop, items are added to a collection variable. A single DML operation is then performed on the collection outside the loop. This is the cornerstone of bulkification.

### The "Try-Catch" Fault Pattern
Every data-intensive element (Create, Update, Delete, Action) must have a Fault Path. This path should log the error to a persistent store and provide a graceful exit or notification, preventing "silent failures."

### The Gatekeeper Pattern
The initial element of a flow should be a decision node that verifies if the flow should run at all (e.g., checking a "Bypass Automation" toggle or verifying specific field changes).

## Anti-Patterns

*   **DML in Loops:** Placing a database query or update inside a loop. This leads to immediate scalability failure as record volumes increase.
*   **Hardcoded Identifiers:** Using static IDs (e.g., `0018000000XyZ`) for records, roles, or groups. This breaks when moving logic between environments (Sandbox to Production).
*   **The "Mega-Flow":** A single, massive flow that attempts to handle every possible scenario for an object. These are impossible to debug and prone to hitting element execution limits.
*   **Recursive Triggers:** Designing a flow that updates a record, which then triggers the same flow again, leading to an infinite loop or stack overflow.

## Edge Cases

### Race Conditions
When two flows are triggered by the same event, the order of execution may not be guaranteed unless explicitly defined by the platform. 065 standards require using "Trigger Order" values where available.

### High-Volume Asynchronous Processing
When processing thousands of records via an asynchronous path, standard limits may be exceeded. In these cases, the flow should be designed to hand off the payload to a dedicated batch processing system.

### Null Pointer Risks
Flows often fail when they attempt to reference a field on a related record that does not exist. Always implement "Null Checks" before traversing relationships (e.g., checking if `AccountID` is not null before accessing `Account.Name`).

## Related Topics
*   **066 Governance and Versioning:** Standards for naming conventions and deployment.
*   **042 Data Integrity Standards:** Principles for maintaining clean data that flows rely upon.
*   **089 Error Logging Frameworks:** Centralized systems for capturing flow exceptions.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |