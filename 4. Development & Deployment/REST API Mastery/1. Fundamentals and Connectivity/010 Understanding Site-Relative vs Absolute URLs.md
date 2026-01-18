# 010 Understanding Site Relative vs Absolute URLs

Canonical documentation for 010 Understanding Site Relative vs Absolute URLs. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of URL (Uniform Resource Locator) addressing is to provide a standardized method for identifying and locating resources on a network. In web architecture, the distinction between relative and absolute URLs addresses the need for resource portability, maintenance efficiency, and contextual resolution. This topic establishes the theoretical framework for how systems interpret resource locations based on the presence or absence of a defined root and protocol.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The structural components of Absolute and Relative URLs.
* The logic of URL resolution (how a relative path becomes an absolute path).
* Use cases for internal vs. external resource linking.
* Impact on portability and environment migration.

**Out of scope:**
* Specific syntax for programming languages (e.g., Python’s `urllib` or JavaScript’s `URL` API).
* Web server configuration (e.g., Nginx rewrites or Apache `.htaccess`).
* SEO-specific ranking algorithms (though SEO implications are noted).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **URL** | Uniform Resource Locator; a reference to a web resource that specifies its location on a computer network and a mechanism for retrieving it. |
| **Absolute URL** | A URL that contains the entire address of the resource, including the protocol and domain name. |
| **Relative URL** | A URL that describes the path to a resource relative to the current document or the site root. |
| **Protocol/Scheme** | The initial part of a URL (e.g., `http`, `https`, `ftp`) indicating the set of rules used for data transfer. |
| **Host/Authority** | The domain name or IP address identifying the server where the resource resides. |
| **Document Relative** | A path starting from the current directory level of the requesting file. |
| **Root Relative** | A path starting from the top-level directory (root) of the current domain, typically denoted by a leading forward slash `/`. |
| **Base URL** | The reference point used by a system to resolve relative URLs into absolute ones. |

## Core Concepts

### The Anatomy of a Resource Reference
A resource reference is composed of several segments: `scheme://host:port/path?query#fragment`. 
* **Absolute URLs** provide all segments necessary to locate the resource from any point on the internet.
* **Relative URLs** omit the scheme and host, relying on the "context" of the current environment to fill in the missing information.

### Contextual Resolution
The fundamental concept of relative URLs is **Resolution**. When a client (such as a web browser) encounters a relative URL, it performs a calculation:
`Base URL + Relative Path = Resolved Absolute URL`.

### Portability
Portability refers to the ability to move a set of resources (a website or application) from one environment (e.g., `localhost`) to another (e.g., `production.com`) without breaking links. Relative URLs facilitate high portability, whereas absolute URLs often require manual or programmatic updates during migration.

## Standard Model

### The Absolute Model
An absolute URL is self-contained. 
* **Example:** `https://www.example.com/images/logo.png`
* **Behavior:** Regardless of where this link is placed (an email, a different website, or a local file), it will always point to the same resource on the internet.

### The Relative Model
Relative URLs are categorized into two primary types:

1.  **Site Root Relative:** Starts with a `/`. It tells the system to go to the top-level directory of the current host and follow the path from there.
    *   *Example:* `/assets/style.css`
    *   *Resolution:* If the site is `https://example.com/blog`, this resolves to `https://example.com/assets/style.css`.
2.  **Document Relative:** Does not start with a `/`. It tells the system to look relative to the current folder.
    *   *Example:* `images/photo.jpg` (looks in the current folder) or `../images/photo.jpg` (goes up one level, then into the images folder).

## Common Patterns

### Internal Linking (Relative)
For navigation within a single application or website, relative URLs are the standard. This ensures that the application remains functional across development, staging, and production environments without configuration changes to the content.

### External Resource Referencing (Absolute)
When linking to resources hosted on a different domain (e.g., a third-party API, a CDN, or a different website), absolute URLs are mandatory.

### Protocol-Relative URLs
A pattern where the scheme is omitted but the host is preserved (e.g., `//cdn.example.com/script.js`). This allows the resource to be fetched using the same protocol (`http` or `https`) as the host document. *Note: This pattern is becoming less common as `https` becomes the universal standard.*

## Anti-Patterns

### Hardcoding Domains for Internal Links
Using `https://production-site.com/about` inside the source code of the "About" page.
* **Consequence:** Links break in development environments, and moving to a new domain requires a full database or codebase search-and-replace.

### Over-reliance on Document Relative Paths in Deep Hierarchies
Using `../../../assets/logo.png` in deeply nested folder structures.
* **Consequence:** High fragility. Moving the file one folder deeper breaks the link. Root-relative paths (`/assets/logo.png`) are preferred for stability.

### Mixing Protocols
Using an absolute `http` URL for an image on an `https` site.
* **Consequence:** "Mixed Content" security warnings and browser blocks.

## Edge Cases

### The `<base>` Tag
The HTML `<base>` element can change the resolution logic for all relative URLs in a document. If `<base href="https://archive.org/v1/">` is set, a relative link to `contact.html` will resolve to `https://archive.org/v1/contact.html` regardless of the actual URL of the page.

### Single Page Applications (SPAs)
In SPAs, "URLs" are often handled by client-side routing. While they look like standard URLs, the resolution of assets (JS/CSS) must be strictly managed (usually via root-relative paths) to ensure that deep-linked routes (e.g., `example.com/user/settings/profile`) don't attempt to load assets from non-existent subdirectories.

### Canonical Tags
In SEO, the `rel="canonical"` link should almost always be an **Absolute URL**. This provides an unambiguous instruction to search engines regarding the primary version of a page, preventing issues with duplicate content across subdomains or protocols.

## Related Topics
* **011 URL Encoding and Percent-Encoding:** How special characters are handled within these URL structures.
* **015 Cross-Origin Resource Sharing (CORS):** How absolute URLs impact security boundaries.
* **022 DNS and Hostname Resolution:** The process of turning the "Host" part of an absolute URL into an IP address.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-18 | Initial AI-generated canonical documentation |