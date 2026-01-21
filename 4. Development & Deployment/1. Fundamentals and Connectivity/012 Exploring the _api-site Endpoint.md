# 012 Exploring the api site Endpoint

Canonical documentation for 012 Exploring the api site Endpoint. This document defines concepts, terminology, and standard usage.

## Purpose
The `api/site` endpoint (or its architectural equivalent) exists to provide a centralized mechanism for **service discovery** and **environment introspection**. In complex distributed systems or public-facing APIs, clients require a method to programmatically determine the identity, configuration, capabilities, and operational status of the host environment without engaging with functional data resources.

This endpoint addresses the problem of "client blindness," where a consumer lacks context regarding the specific instance of the service they are interacting with, leading to version mismatches or inefficient feature probing.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative. While specific URI structures may vary (e.g., `/info`, `/site`, `/v1/metadata`), the underlying principles remain constant.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Metadata Exchange:** The definition of non-functional data describing the API host.
* **Capability Discovery:** Methods for the API to broadcast supported features or extensions.
* **Environmental Context:** Providing information regarding the deployment tier (e.g., production vs. staging).
* **Structural Standards:** The expected format and lifecycle of site-level metadata.

**Out of scope:**
* **Functional Data:** Business logic or resource-specific data (e.g., user profiles, product lists).
* **Authentication Implementation:** Specific methods of securing the endpoint (OAuth2, JWT, etc.).
* **Vendor-Specific Schemas:** Proprietary response formats unique to a single software provider.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Site Endpoint** | A specialized API resource that returns metadata about the API instance itself rather than the data it manages. |
| **Introspection** | The process by which a client queries an API to discover its schema, supported operations, or configuration. |
| **Capability Negotiation** | A handshake process where a client determines which optional features or modules are active on the server. |
| **Metadata** | Data that provides information about other data or the system environment (e.g., version numbers, maintenance status). |
| **HATEOAS** | Hypermedia as the Engine of Application State; a constraint of REST that allows the site endpoint to serve as a directory for other resources. |

## Core Concepts

### 1. Environmental Identity
The site endpoint serves as the "ID Card" for the API. It identifies the specific instance of the application, which is critical in multi-region or multi-tenant architectures. This includes the site name, description, and unique instance identifiers.

### 2. Versioning and Compatibility
One of the primary functions of exploring the site endpoint is to determine the API version and build metadata. This allows clients to adjust their behavior based on the features available in that specific version, ensuring backward and forward compatibility.

### 3. Feature Toggling and Discovery
Modern APIs are often modular. The site endpoint provides a list of enabled features or "capabilities." Instead of a client attempting a request and receiving a `404 Not Found` or `501 Not Implemented`, it can pre-emptively check the site endpoint to see if a specific module (e.g., "Search," "Beta-Analytics") is active.

### 4. Operational Status
The endpoint often reflects the high-level health or maintenance state of the site. It acts as a signal to clients whether the system is in a "Read-Only" mode, undergoing scheduled maintenance, or operating at full capacity.

## Standard Model
The standard model for a site endpoint follows a hierarchical, read-only structure.

1.  **Identity Block:** Contains `site_name`, `site_url`, and `organization`.
2.  **Version Block:** Contains `api_version`, `build_number`, and `schema_revision`.
3.  **Configuration Block:** Contains public-facing settings such as `supported_languages`, `timezone`, and `max_upload_size`.
4.  **Links Block (HATEOAS):** A collection of URIs pointing to major entry points (e.g., `/users`, `/products`, `/auth`).

## Common Patterns

### The Root Discovery Pattern
The API root (`/`) acts as the site endpoint. When a client performs a `GET` request on the base URL, the server returns a directory of resources and site metadata.

### The Metadata Sidecar
A dedicated sub-resource, often `/site` or `/info`, is used to separate environmental metadata from the primary resource tree. This is common in APIs that want to keep the root clean for functional resources.

### The Health-Check Hybrid
The site endpoint is combined with a health check, providing both "liveness" data and "identity" data in a single payload to reduce network overhead during client initialization.

## Anti-Patterns

*   **Leaking Sensitive Internals:** Including internal IP addresses, server file paths, or detailed stack traces in the site metadata.
*   **Hardcoding Metadata:** Manually updating the version or status in a static file rather than deriving it from the actual deployment environment.
*   **High-Frequency Polling:** Clients treating the site endpoint as a real-time heartbeat monitor (unless specifically designed for such), which can lead to unnecessary load.
*   **Mixing Concerns:** Including user-specific data (like "Current User Profile") within the global site endpoint response.

## Edge Cases

*   **Maintenance Mode:** When the site is in maintenance mode, the endpoint should still be reachable but should return a status indicating the restricted state, often with an estimated "retry-after" value.
*   **Multi-Tenancy:** In a multi-tenant system, the "site" endpoint may return different metadata depending on the `Host` header or the API key provided, reflecting the specific tenant's configuration.
*   **Localization:** The site name or description may change based on the `Accept-Language` header, requiring the endpoint to support content negotiation.
*   **Partial Outages:** The site endpoint may report "Healthy" even if a specific sub-service is down. Defining the "granularity of truth" for the site endpoint is a common architectural challenge.

## Related Topics
*   **API Versioning Strategies:** How the version numbers reported by the site endpoint are managed.
*   **Service Discovery (Consul/Etcd):** Externalized versions of site exploration for microservices.
*   **HATEOAS and Hypermedia:** The standard for linking resources within the site endpoint.
*   **Health Check API Patterns:** Specific patterns for monitoring liveness and readiness.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |