# 055 Monitoring and Troubleshooting

Canonical documentation for 055 Monitoring and Troubleshooting. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of 055 Monitoring and Troubleshooting is to provide a systematic framework for observing system behavior, detecting deviations from expected states, and restoring service through structured diagnostic processes. This topic addresses the fundamental need for visibility into complex systems to ensure reliability, performance, and security. It bridges the gap between passive data collection and active problem resolution.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Telemetry Fundamentals:** The collection and categorization of system data (metrics, logs, and traces).
* **Observability Principles:** The ability to infer internal states from external outputs.
* **Diagnostic Methodologies:** Systematic approaches to identifying and resolving faults.
* **Health Assessment:** Defining and measuring the "correct" state of a system.

**Out of scope:**
* **Specific vendor implementations:** Proprietary tools, specific SaaS platforms, or cloud-provider-specific dashboards.
* **Code-level debugging:** Language-specific syntax errors or IDE-based step-through debugging.
* **Hardware repair:** Physical maintenance of infrastructure components.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Telemetry** | The automated process of recording and transmitting data from remote or inaccessible sources to an IT system for monitoring. |
| **Observability** | A measure of how well internal states of a system can be inferred from knowledge of its external outputs. |
| **Metric** | A numerical measurement of a system attribute over a specific interval of time. |
| **Log** | An immutable, time-stamped record of discrete events that happened within a system. |
| **Trace** | A representation of a single request's journey through a distributed system. |
| **Alert** | A notification triggered when a specific condition or threshold is met, requiring human or automated intervention. |
| **Root Cause** | The fundamental, underlying reason for a failure or problem which, if resolved, prevents recurrence. |
| **MTTR** | Mean Time to Recovery/Resolution; the average time taken to restore service after a failure. |

## Core Concepts

### The Three Pillars of Observability
1.  **Metrics:** Aggregatable data points used to identify trends and trigger alerts (e.g., CPU usage, request count).
2.  **Logs:** Detailed contextual information about specific events, essential for post-facto analysis.
3.  **Traces:** Contextualized paths of requests across service boundaries, vital for identifying bottlenecks in distributed architectures.

### White-box vs. Black-box Monitoring
*   **White-box Monitoring:** Monitoring based on metrics exposed by the internals of the system (e.g., logs, internal state, private endpoints).
*   **Black-box Monitoring:** Monitoring the system from the outside as a user would (e.g., pinging an endpoint, checking for a 200 OK response).

### The Golden Signals
A standard set of metrics used to evaluate system health:
*   **Latency:** The time it takes to service a request.
*   **Traffic:** A measure of how much demand is being placed on the system.
*   **Errors:** The rate of requests that fail, either explicitly or implicitly.
*   **Saturation:** How "full" the service is, emphasizing the most constrained resources.

## Standard Model

The standard model for 055 Monitoring and Troubleshooting follows a cyclical lifecycle:

1.  **Instrumentation:** Integrating sensors or code to emit telemetry.
2.  **Collection & Aggregation:** Gathering telemetry from disparate sources into a centralized repository.
3.  **Detection:** Using thresholds or heuristics to identify anomalies or breaches of Service Level Objectives (SLOs).
4.  **Triage:** Determining the severity and impact of an issue to prioritize response.
5.  **Diagnosis (Troubleshooting):** Applying deductive reasoning to isolate the root cause.
6.  **Remediation:** Implementing a fix or workaround to restore service.
7.  **Analysis:** Conducting a post-incident review to improve future detection and prevention.

## Common Patterns

*   **Threshold-based Alerting:** Triggering notifications when a metric exceeds or falls below a predefined static value.
*   **Anomaly Detection:** Using historical baselines to identify behavior that deviates from the statistical norm.
*   **Distributed Tracing:** Correlating unique IDs across multiple services to visualize the flow of a single transaction.
*   **Heartbeat/Keep-alive:** Periodic signals sent by a component to indicate it is still functioning.
*   **Centralized Logging:** Shipping logs from all distributed components to a single searchable index.

## Anti-Patterns

*   **Alert Fatigue:** Flooding responders with low-priority or non-actionable alerts, leading to critical signals being ignored.
*   **Siloed Monitoring:** Collecting data in isolated tools that do not communicate, preventing cross-correlation during an incident.
*   **Monitoring Everything:** Collecting excessive data without a clear purpose, leading to high storage costs and "noise" that obscures "signal."
*   **Manual Remediation Only:** Relying solely on human intervention for well-known, repetitive failures that could be automated.
*   **Dashboard Narcissism:** Creating complex, visually appealing dashboards that do not provide actionable insights or reflect actual user experience.

## Edge Cases

*   **Transient Faults (Flapping):** Issues that appear and disappear rapidly, often causing "alert storms" if de-bouncing logic is not applied.
*   **Heisenbugs:** Problems that disappear or change behavior when being observed or when additional logging is enabled.
*   **Cascading Failures:** A failure in one component that triggers a series of failures in dependent components, often masking the original root cause.
*   **Clock Skew:** Discrepancies in system time across distributed nodes that make log correlation and trace sequencing difficult.
*   **Silent Failures:** Scenarios where a system continues to report "healthy" status while failing to perform its primary function (e.g., a web server returning 200 OK but an empty body).

## Related Topics

*   **Service Level Management (SLIs/SLOs/SLAs):** Defining the targets that monitoring is intended to measure.
*   **Incident Management:** The organizational process for responding to the alerts generated by monitoring.
*   **Capacity Planning:** Using historical monitoring data to predict future resource needs.
*   **Site Reliability Engineering (SRE):** The discipline that applies engineering principles to monitoring and operations.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |