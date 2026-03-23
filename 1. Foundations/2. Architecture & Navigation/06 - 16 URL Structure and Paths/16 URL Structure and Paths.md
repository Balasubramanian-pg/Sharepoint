# URL Structure and Paths

Canonical documentation for URL Structure and Paths. This document defines concepts, terminology, and standard usage.

## Purpose

The URL Structure and Paths topic exists to provide a comprehensive understanding of how Uniform Resource Locators (URLs) are constructed, interpreted, and utilized in various contexts. This documentation addresses the problem space of inconsistent URL handling, which can lead to issues such as broken links, security vulnerabilities, and difficulties in maintaining web applications. By establishing a clear understanding of URL structure and paths, developers, administrators, and users can ensure that their systems and applications interact correctly with URLs, promoting reliability, scalability, and security.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

The scope of this topic includes the fundamental concepts, terminology, and standard practices related to URL structure and paths.

**In scope:**
* URL syntax and components (scheme, authority, path, query, fragment)
* Path resolution and normalization
* URL encoding and decoding
* URL scheme and protocol handling

**Out of scope:**
* Tool-specific implementations (e.g., programming language libraries, web frameworks)
* Vendor-specific behavior (e.g., browser quirks, server configurations)
* Domain-specific URL conventions (e.g., URL schemes for specific protocols or applications)

## Definitions

The following terms are used throughout this documentation:

| Term | Definition |
|------|------------|
| URL | A Uniform Resource Locator, a string that identifies a resource on the web |
| URI | A Uniform Resource Identifier, a string that identifies a resource, which may or may not be accessible |
| Scheme | The protocol part of a URL (e.g., http, https, ftp) |
| Authority | The network location part of a URL (e.g., domain, hostname, port) |
| Path | The hierarchical sequence of directories and files in a URL |
| Query | The set of key-value pairs appended to a URL, separated by '?' and '&' |
| Fragment | The part of a URL that identifies a secondary resource or a specific part of a primary resource |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

The fundamental ideas that make up the topic of URL Structure and Paths are:

### Concept One: URL Syntax
A URL consists of several components, including the scheme, authority, path, query, and fragment. Each component has a specific purpose and is separated by a specific delimiter.

### Concept Two: Path Resolution
Path resolution refers to the process of determining the absolute path of a URL, taking into account the base URL, relative paths, and any redirects or aliases.

## Standard Model

The standard model for URL Structure and Paths is based on the specifications outlined in RFC 3986 (Uniform Resource Identifier (URI): Generic Syntax) and RFC 7230 (Hypertext Transfer Protocol (HTTP/1.1): Message Syntax and Routing). This model recommends the following:

* Using the http and https schemes for web resources
* Encoding URLs using UTF-8
* Normalizing URLs to avoid ambiguity and ensure consistency
* Handling redirects and aliases correctly

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified.

## Common Patterns

The following patterns are commonly associated with URL Structure and Paths:

* Using relative paths to simplify URL construction and reduce maintenance
* Employing URL rewriting or routing to hide internal implementation details
* Utilizing query parameters to pass data between pages or resources

## Anti-Patterns

The following anti-patterns are discouraged when working with URL Structure and Paths:

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues.

* Hardcoding URLs or paths in application code
* Using non-standard or proprietary URL schemes
* Failing to handle URL encoding and decoding correctly

## Edge Cases

The following edge cases are related to URL Structure and Paths:

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions.

* Handling URLs with non-ASCII characters or special characters
* Dealing with URLs that contain fragments or anchors
* Resolving URLs with relative paths or redirects

## Related Topics

The following topics are related to URL Structure and Paths:

* HTTP Protocol
* Web Application Security
* URI Schemes and Protocols

## References

The following external references are relevant to this topic:

* RFC 3986: Uniform Resource Identifier (URI): Generic Syntax
* RFC 7230: Hypertext Transfer Protocol (HTTP/1.1): Message Syntax and Routing
* W3C URL Specification

## Change Log

The following notable changes have been made to this topic:

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-02-01 | Added section on common patterns and anti-patterns |
| 1.2 | 2026-03-01 | Updated references to include W3C URL Specification |