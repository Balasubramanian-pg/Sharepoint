# 027 Using Dynamic Content

Canonical documentation for 027 Using Dynamic Content. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Dynamic Content is to enable the delivery of personalized, contextually relevant, and data-driven information within a standardized framework. By decoupling content structure from specific data values, systems can generate unique outputs for different recipients or environments without requiring manual intervention for each variation. This addresses the problem of scalability in communication, user experience, and data presentation, ensuring that the right information reaches the right entity at the right time.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Theoretical frameworks for variable substitution and conditional logic.
* Data-to-template mapping principles.
* Structural requirements for dynamic rendering.
* Governance and fallback strategies.

**Out of scope:**
* Specific programming language syntax (e.g., Jinja2, Liquid, Handlebars).
* Vendor-specific platform limitations (e.g., specific CRM or CMS constraints).
* Database administration or ETL (Extract, Transform, Load) processes.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Placeholder / Token** | A reserved string or symbol within a template that is replaced by actual data during the rendering process. |
| **Conditional Logic** | Rules (e.g., If/Then/Else) that determine whether specific content blocks are displayed based on data attributes. |
| **Fallback Content** | Default content or "safe" values used when the primary data source is missing or null. |
| **Rendering Engine** | The logical component that processes templates and data to produce the final output. |
| **Data Mapping** | The process of associating specific data fields from a source to their corresponding placeholders in a template. |
| **Iteration** | The repeated rendering of a content block for each item in a data collection (e.g., a list of products). |

## Core Concepts
The fundamental ideas of Dynamic Content revolve around the separation of concerns:

1.  **Separation of Content and Data:** The template defines the structure and "look and feel," while the data provides the specific values. This allows for updates to the design without altering the underlying data, and vice versa.
2.  **Context Awareness:** Dynamic content responds to the context of the request, which may include user profile data, behavioral triggers, temporal factors (time/date), or environmental variables (location/device).
3.  **Deterministic Logic:** Given the same input data and the same template, the rendering engine should produce a predictable and consistent output.
4.  **Extensibility:** The system should allow for the addition of new data points or logic rules without necessitating a complete redesign of the existing content architecture.

## Standard Model
The standard model for Dynamic Content follows a linear processing flow:

1.  **Data Retrieval:** The system fetches the necessary data objects from a source (API, Database, User Session).
2.  **Template Parsing:** The rendering engine identifies placeholders and logic blocks within the template.
3.  **Logic Evaluation:** The engine evaluates conditional statements and loops against the retrieved data.
4.  **Substitution:** Placeholders are replaced with the corresponding data values.
5.  **Validation/Sanitization:** The final output is checked for integrity (e.g., ensuring no broken tags or missing mandatory fields).
6.  **Output Delivery:** The rendered content is served to the end-user or destination system.

## Common Patterns
*   **Simple Substitution:** Replacing a single token (e.g., `{{first_name}}`) with a string.
*   **Conditional Visibility:** Showing or hiding entire sections of content based on a boolean or value-based check (e.g., "Show 'Discount Code' only if 'Member_Status' is 'Gold'").
*   **Dynamic Lists:** Using loops to render an unknown number of items, such as an order summary or a list of recommended articles.
*   **Localized Formatting:** Dynamically adjusting the display of dates, currencies, and measurements based on the recipient's locale data.

## Anti-Patterns
*   **Hardcoding Logic:** Embedding business logic directly into the content layer rather than the data or application layer, making updates difficult.
*   **Missing Fallbacks:** Failing to provide default values for missing data, leading to "Hello [NULL]" or broken layouts.
*   **Over-nesting:** Creating excessively complex conditional logic (e.g., five levels of nested IF statements) that is difficult to audit or debug.
*   **Data Over-fetching:** Retrieving massive datasets when only a few specific fields are required for the dynamic placeholders.
*   **Implicit Type Conversion:** Relying on the rendering engine to guess data types (e.g., treating the string "01" as the integer 1), which can lead to logic errors.

## Edge Cases
*   **Null vs. Empty String:** Distinguishing between a data field that is intentionally empty and one that is missing entirely.
*   **Circular References:** Scenarios where dynamic content calls a data point that triggers another dynamic content block, potentially leading to infinite loops.
*   **Character Encoding:** Handling special characters or HTML entities within data that might break the final rendered output (e.g., an ampersand in a URL).
*   **Race Conditions:** When the data used for personalization changes between the time the content is triggered and the time it is rendered.
*   **Overflow Content:** When dynamic data is significantly longer than the space allocated in the template design (e.g., a very long name breaking a button layout).

## Related Topics
*   **012 Data Governance:** Standards for maintaining the quality of data used in dynamic content.
*   **045 Template Versioning:** Managing changes to the structures that house dynamic content.
*   **088 Personalization Strategy:** The high-level business logic governing *why* specific content is made dynamic.
*   **102 Localization and Internationalization:** The specific application of dynamic content for global audiences.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |