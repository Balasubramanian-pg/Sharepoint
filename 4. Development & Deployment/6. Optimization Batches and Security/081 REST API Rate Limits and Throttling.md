# [081 REST API Rate Limits and Throttling](4. Development & Deployment/REST API Mastery/6. Optimization Batches and Security/081 REST API Rate Limits and Throttling.md)

Canonical documentation for [081 REST API Rate Limits and Throttling](4. Development & Deployment/REST API Mastery/6. Optimization Batches and Security/081 REST API Rate Limits and Throttling.md). This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of REST API Rate Limits and Throttling is to protect the availability, reliability, and security of a web service. By controlling the frequency and volume of incoming requests, service providers prevent resource exhaustion, mitigate Denial of Service (DoS) attacks, ensure equitable distribution of resources among consumers (Fair Use), and manage infrastructure costs. This mechanism acts as a contract between the provider and the consumer regarding the expected load.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   **Mechanisms:** Algorithms for counting and limiting requests.
*   **Communication:** Standardized HTTP responses and headers for communicating limits.
*   **Identification:** Strategies for identifying the entities being limited.
*   **Policy Tiers:** Conceptual frameworks for varying limit levels.

**Out of scope:**
*   **Specific vendor implementations:** (e.g., AWS API Gateway, NGINX modules, or specific library configurations).
*   **Network-layer DDoS mitigation:** (e.g., BGP Anycast or hardware-level packet filtering).
*   **Authentication protocols:** (e.g., OAuth2 or JWT internals), though they are used for identification.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Rate Limit** | The hard maximum number of requests a consumer is permitted to make within a specific time window. |
| **Throttling** | The process of restricting or slowing down requests once a threshold is met to prevent system degradation. |
| **Quota** | A long-term limit (e.g., daily or monthly) often tied to billing or subscription tiers. |
| **Burst** | A temporary allowance for a consumer to exceed their steady-state rate limit for a short duration. |
| **Window** | The discrete or rolling time interval used to calculate the request count (e.g., per second, per minute). |
| **429 Too Many Requests** | The standard HTTP status code used to indicate a consumer has exceeded their rate limit. |
| **Backoff** | The strategy employed by a client to wait for a period before retrying a failed request. |

## Core Concepts

### Identification Strategies
To apply limits, the system must identify the requester. Common strategies include:
*   **API Key/Token:** Limits are tied to a specific credential. This is the most precise method for authenticated traffic.
*   **IP Address:** Limits are tied to the source IP. Used for unauthenticated traffic, though susceptible to issues with NAT and shared proxies.
*   **User/Account ID:** Limits are applied at the organizational or user level, regardless of the number of keys or IPs used.

### Granularity
Limits can be applied at different levels of the API hierarchy:
*   **Global:** A single limit for all requests to the entire API.
*   **Endpoint-specific:** Different limits for "heavy" operations (e.g., complex searches) vs. "light" operations (e.g., health checks).
*   **Method-specific:** Different limits for `GET` (read) vs. `POST/DELETE` (write) operations.

## Standard Model

### The Response Protocol
When a rate limit is triggered, the server must inform the client using the **HTTP 429 Too Many Requests** status code. To provide a machine-readable way for clients to recover, the following headers are standard:

*   `X-RateLimit-Limit`: The maximum number of requests permitted in the current window.
*   `X-RateLimit-Remaining`: The number of requests left in the current window.
*   `X-RateLimit-Reset`: The time at which the current window resets (expressed as a Unix timestamp or seconds remaining).
*   `Retry-After`: A standard HTTP header indicating how long the client should wait before making a new attempt (in seconds or an HTTP-date).

### Enforcement Algorithms
1.  **Fixed Window:** Resets at specific intervals (e.g., the start of every minute). Simple but allows "bursting" at the window boundaries.
2.  **Sliding Window Log:** Tracks the timestamp of every request. Highly accurate but memory-intensive.
3.  **Sliding Window Counter:** A hybrid approach that uses the current window and a percentage of the previous window to smooth out spikes.
4.  **Token Bucket:** Tokens are added to a bucket at a fixed rate. Each request consumes a token. This allows for bursts up to the bucket's capacity.
5.  **Leaky Bucket:** Requests enter a "bucket" and are processed at a constant rate. If the bucket overflows, requests are discarded. This ensures a steady outflow of requests.

## Common Patterns

### Tiered Limits
Providing different rate limits based on the consumer's profile (e.g., Free, Bronze, Gold, Enterprise). This is often used as a monetization strategy.

### Graceful Degradation
Instead of a hard 429 error, the system may serve cached data or simplified responses when limits are reached, though this is less common in strict REST environments.

### Exponential Backoff
A client-side pattern where the wait time between retries increases exponentially (e.g., 1s, 2s, 4s, 8s) to prevent a "thundering herd" effect when the limit resets.

## Anti-Patterns

*   **Returning 500 Internal Server Error:** This incorrectly implies a server-side failure rather than a client-side rate violation.
*   **Silent Dropping:** Dropping requests without returning a response or status code, making debugging impossible for the consumer.
*   **Inconsistent Headers:** Using non-standard header names or inconsistent time formats (e.g., mixing milliseconds and seconds) across different endpoints.
*   **Global-Only Limiting:** Applying the same limit to a lightweight "Ping" endpoint and a resource-intensive "Report Generation" endpoint.
*   **Hard-Coded Client Delays:** Clients waiting a fixed amount of time regardless of the `Retry-After` header.

## Edge Cases

*   **Distributed Systems & Race Conditions:** In a distributed environment, multiple API nodes may receive requests simultaneously. Without a centralized counter (e.g., Redis), a client might exceed their limit because nodes are not synchronized.
*   **Clock Skew:** If the server and client clocks are out of sync, `X-RateLimit-Reset` timestamps may be misinterpreted. Using "seconds until reset" instead of an absolute timestamp is a common mitigation.
*   **Pre-flight (OPTIONS) Requests:** In CORS-enabled environments, browsers send `OPTIONS` requests. These are generally excluded from rate limit counts to avoid breaking legitimate browser-based integrations.
*   **Infrastructure-Level Limits:** A request might be throttled by a Load Balancer or WAF before it even reaches the API application logic, potentially resulting in different error formats or headers.

## Related Topics
*   **012 API Security:** General security practices including DoS protection.
*   **045 Distributed Systems:** Challenges of maintaining state across nodes.
*   **092 HTTP Protocol Standards:** The underlying communication layer.
*   **104 API Monetization:** Using limits to define product tiers.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-19 | Initial AI-generated canonical documentation |