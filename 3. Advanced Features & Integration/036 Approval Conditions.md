# 036 Approval Conditions

Canonical documentation for 036 Approval Conditions. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of 036 Approval Conditions is to provide a deterministic framework for evaluating whether a business process, transaction, or state change requires formal authorization. By defining specific logical predicates, organizations can automate the routing of requests, enforce compliance standards, and mitigate risk without requiring manual oversight for every action. 

This topic addresses the problem of "Approval Fatigue" and "Governance Gaps" by ensuring that human intervention is only requested when predefined criteria—based on value, risk, or complexity—are met.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Logical Predicates:** The Boolean logic used to trigger approval workflows.
* **Evaluation Context:** The data inputs and environmental factors considered during evaluation.
* **Threshold Management:** The definition of quantitative and qualitative boundaries.
* **Governance Frameworks:** The theoretical structures governing how conditions are maintained.

**Out of scope:**
* **Specific vendor implementations:** (e.g., SAP Flexible Workflow, ServiceNow Flow Designer, or Salesforce Advanced Approvals).
* **User Interface (UI) Design:** How the approval request appears to the end-user.
* **Notification Protocols:** The methods (Email, SMS, Push) used to alert an approver.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Condition** | A logical statement that evaluates to True or False based on provided input data. |
| **Predicate** | The specific property or attribute being tested (e.g., "Total Amount"). |
| **Threshold** | A specific value or limit that, when crossed, changes the outcome of a condition. |
| **Evaluator** | The engine or logic layer that processes inputs against conditions to determine the next state. |
| **Quorum** | The minimum number of approvals required when a condition triggers a multi-user approval step. |
| **Contextual Data** | The metadata surrounding a request (e.g., requester's department, time of day) used in evaluation. |

## Core Concepts

### Deterministic Evaluation
Approval conditions must be deterministic. Given the same set of input data and the same system state, the evaluation of an approval condition must always yield the same result. This ensures auditability and predictability in business operations.

### Conditional Branching
Approval conditions serve as the primary mechanism for branching in a workflow. A "True" evaluation typically routes the process to a manual intervention step, while a "False" evaluation may allow for "Auto-Approval" or "Straight-Through Processing" (STP).

### Hierarchy of Logic
Conditions are often nested or chained. The evaluation order (Sequential vs. Short-circuit) is critical to ensuring that the most restrictive or most relevant conditions are addressed first.

## Standard Model

The standard model for 036 Approval Conditions follows a **Trigger-Evaluate-Action** lifecycle:

1.  **Input Acquisition:** The system gathers data from the transaction (e.g., a Purchase Order) and the environment (e.g., the user's spending limit).
2.  **Predicate Matching:** The system identifies which conditions are applicable to the current transaction type.
3.  **Logical Evaluation:**
    *   **Simple Conditions:** A single comparison (e.g., `Amount > 5000`).
    *   **Composite Conditions:** Multiple comparisons joined by logical operators (`AND`, `OR`, `NOT`).
4.  **State Resolution:** The evaluator returns a status:
    *   **Bypass:** No conditions met; proceed without approval.
    *   **Required:** One or more conditions met; route to designated approver.
    *   **Rejected:** A "Hard Stop" condition met; terminate the process immediately.

## Common Patterns

### Threshold-Based Pattern
The most common pattern where an approval is triggered when a numerical value exceeds a predefined limit. These are often tiered (e.g., Level 1 approval for >$1k, Level 2 for >$10k).

### Attribute-Based Pattern
Approvals triggered by specific qualitative data points, such as "Project Code," "Risk Rating," or "Geographic Region."

### Exception-Based Pattern
The system assumes auto-approval unless a specific "out-of-bounds" condition is met, such as a request made outside of business hours or a request that deviates from a standard template.

### Consensus (Quorum) Pattern
A condition that requires a specific percentage or number of a group to approve before the condition is considered "Satisfied."

## Anti-Patterns

### Hardcoding Conditions
Embedding approval logic directly into the application code rather than using a configurable rules engine. This prevents business agility and requires code deployments for simple policy changes.

### Circular Dependencies
Defining conditions that rely on each other in a way that creates an infinite loop (e.g., Condition A requires Condition B to be True, but Condition B is only evaluated if Condition A is False).

### Approval Bloat
Setting thresholds so low that nearly every transaction requires manual approval. This leads to "rubber-stamping," where approvers approve requests without scrutiny due to high volume.

### Ghost Conditions
Conditions that rely on data fields that are optional or frequently null, leading to unpredictable workflow behavior or stuck processes.

## Edge Cases

### Null or Missing Data
If a condition evaluates a field that is empty, the system must have a "Fail-Safe" default. Usually, this defaults to "Approval Required" to ensure security, though this can vary by risk appetite.

### Mid-Process Policy Changes
If an approval condition is modified while a transaction is currently "In-Flight," the system must determine whether to apply the "Legacy" logic or the "Current" logic.

### Self-Approval Paradox
When the requester of a transaction is also the person defined by the condition as the approver. Standard governance requires a "Delegate" or "Escalation" path to prevent conflict of interest.

### Tie-Breakers in Parallel Logic
In scenarios where multiple conflicting conditions are met simultaneously, the system must have a priority ranking to determine which condition takes precedence.

## Related Topics
* **012 Workflow Orchestration:** The broader system that executes the paths determined by approval conditions.
* **045 Identity and Access Management (IAM):** Defining who has the authority to act when a condition is met.
* **088 Audit Logging:** The recording of how and why a condition was evaluated for compliance purposes.
* **Delegation of Authority (DoA):** The legal or organizational framework that informs threshold values.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |