# Site Columns

Canonical documentation for Site Columns. This document defines concepts, terminology, and standard usage.

## Purpose

Site Columns are a fundamental concept in content management and information architecture, enabling the creation of reusable and standardized metadata across multiple sites, lists, and libraries. This topic exists to address the problem space of managing and organizing site-wide metadata, providing a common framework for understanding and implementing site columns. By standardizing site columns, organizations can improve data consistency, reduce errors, and enhance the overall user experience.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope

**In scope:**
* Definition and purpose of site columns
* Site column types and configurations
* Best practices for creating and managing site columns

**Out of scope:**
* Tool-specific implementations (e.g., SharePoint, Drupal)
* Vendor-specific behavior and customizations
* Low-level technical details (e.g., database schema, API calls)

## Definitions

| Term | Definition |
|------|------------|
| Site Column | A reusable column definition that can be used across multiple sites, lists, and libraries to store and manage metadata. |
| Column Type | The data type of a site column, such as text, number, date, or choice. |
| Column Configuration | The settings and options associated with a site column, including validation rules, default values, and display settings. |

> [!TIP]
> Definitions should be stable over time; avoid contextual language.

## Core Concepts

### Site Column Definition
A site column definition is a blueprint for a column that can be used across multiple sites, lists, and libraries. It includes the column type, name, and configuration settings.

### Site Column Types
Site columns can be based on various data types, including text, number, date, choice, and more. Each column type has its own set of configuration options and validation rules.

## Standard Model

The standard model for site columns involves creating a centralized repository of site column definitions that can be reused across the organization. This approach ensures consistency and reduces duplication of effort. The standard model includes the following components:
* Site column definitions
* Column types and configurations
* Validation rules and default values
* Display settings and formatting options

> [!IMPORTANT]
> Deviations from the standard model should be explicitly documented and justified.

## Common Patterns

* Using site columns to store metadata that is common across multiple lists and libraries
* Creating site columns for specific business processes or workflows
* Using column types and configurations to enforce data validation and consistency

## Anti-Patterns

> [!WARNING]
> These anti-patterns often lead to maintenance or scalability issues.

* Creating duplicate site columns with similar definitions and configurations
* Using site columns to store large amounts of unstructured data
* Failing to document and maintain site column definitions and configurations

## Edge Cases

> [!CAUTION]
> Edge cases are frequently overlooked and may cause incorrect assumptions.

* Handling site columns with complex data types (e.g., nested lists, hierarchical data)
* Managing site columns with unique validation rules or default values
* Integrating site columns with external data sources or systems

## Related Topics

* Content Types
* Lists and Libraries
* Metadata Management
* Information Architecture

## References

* [Microsoft SharePoint Documentation: Site Columns](https://docs.microsoft.com/en-us/sharepoint/dev/schema/site-column-element)
* [W3C Metadata Standards](https://www.w3.org/standards/techs/metadata)

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial documentation |
| 1.1 | 2026-02-01 | Added section on common patterns and anti-patterns |
| 1.2 | 2026-03-01 | Updated definitions and standard model sections |