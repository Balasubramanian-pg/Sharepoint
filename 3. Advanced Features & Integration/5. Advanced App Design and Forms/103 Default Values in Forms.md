# 103 Default Values in Forms

Canonical documentation for 103 Default Values in Forms. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of default values in forms is to streamline data entry, reduce cognitive load, and guide users toward preferred or common configurations. By pre-populating fields with sensible, high-probability data, systems minimize the "interaction cost" required to complete a task. This topic addresses the balance between user efficiency and data integrity, ensuring that pre-filled information assists the user without inducing errors or unintended submissions.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   The logic and theory behind pre-populating input fields.
*   Categorization of default value types (Static, Dynamic, Predictive).
*   User experience (UX) implications of pre-filled data.
*   Data integrity and validation rules regarding defaults.

**Out of scope:**
*   Specific HTML attributes (e.g., `value="..."`) or framework-specific state management (e.g., React `useState`).
*   Visual styling of form elements (CSS/Theming).
*   Placeholder text (which is a visual hint, not a data value).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Default Value** | A pre-existing data entry in a form field that will be submitted as part of the payload if the user does not modify it. |
| **Placeholder** | Non-persistent instructional text displayed within a field when it is empty; it does not constitute data. |
| **Static Default** | A fixed value defined at the schema or UI level that remains constant for all users (e.g., a "Country" field defaulting to the primary market). |
| **Dynamic Default** | A value calculated at runtime based on context, such as user profile data, session history, or environmental factors (e.g., current date). |
| **Predictive Default** | A value suggested by an algorithm or heuristic based on historical patterns or machine learning. |
| **Dirty State** | The condition of a form field once the user has modified it from its initial default value. |
| **Opt-in/Opt-out** | The application of defaults to binary choices (checkboxes/toggles) that determine a user's consent or preference by omission. |

## Core Concepts

### The Principle of Least Effort
Default values should be designed to satisfy the "80/20 rule," where the default value is the correct choice for at least 80% of the target audience. This reduces the number of physical interactions (clicks, keystrokes) required to complete a form.

### Data Integrity vs. Convenience
While defaults increase speed, they introduce the risk of "acquiescence bias," where users skip over pre-filled fields without verifying the accuracy of the data. Systems must weigh the benefit of speed against the cost of receiving incorrect data.

### Persistence and State
A default value exists in the data model from the moment the form is initialized. Unlike a placeholder, a default value is "real" data. If a user submits a form without touching a defaulted field, the system must treat that value as an intentional choice.

## Standard Model

The standard model for implementing default values follows a hierarchy of data sourcing:

1.  **Contextual/Session Data:** If the system knows the user's current context (e.g., they are viewing a specific category), the form should default to that category.
2.  **User Profile/Historical Data:** If the user has previously provided information (e.g., a shipping address), that data should serve as the default for subsequent interactions.
3.  **Environmental Data:** Using metadata such as IP-based geolocation, system language, or current timestamp to populate fields.
4.  **System-Wide Sensible Defaults:** If no specific data is available, the system uses the most common or "safest" value defined by the business logic.

## Common Patterns

### Smart Defaults
Automatically selecting the most likely option based on available metadata. For example, defaulting a "Currency" field based on the user's detected region.

### Last-Used Value
In repetitive data entry tasks, the system defaults a field to the value entered in the previous submission. This is common in accounting or inventory management systems.

### Safe Defaults
In high-stakes scenarios (e.g., deleting data, financial transfers), the default value is set to the most conservative or "null" option to force an intentional user action.

### The "None" or "Select" Default
Forcing a user to make a conscious choice by defaulting a required dropdown to a null/instructional state. While technically a "lack of a value," it is the standard pattern for preventing accidental submission of the first item in a list.

## Anti-Patterns

### The Placeholder-as-Value
Using placeholder text to represent a default value. This is a failure of both accessibility and data integrity, as placeholders disappear upon focus and are not submitted as data.

### Over-Presumption
Defaulting highly personal or sensitive fields (e.g., gender, religion, or marketing consent) in a way that assumes user agreement. This can lead to legal non-compliance (e.g., GDPR) and user frustration.

### Destructive Defaults
Setting the default focus or selection on a destructive action (e.g., "Format Disk" or "Delete All") in a modal dialog.

### Sticky Defaults in Shared Environments
Retaining a previous user's data as a default on a public or shared terminal, leading to privacy breaches.

## Edge Cases

*   **Multi-Step Forms:** If a user navigates back to a previous step, the "default" should be the value they just entered, not the original system default.
*   **Conditional Logic:** When a field's visibility depends on another field, the default value of the hidden field must be carefully managed to avoid submitting "invisible" data that contradicts the user's visible choices.
*   **Null vs. Empty String:** Systems must distinguish between a field that is intentionally left blank (if the default was blank) and a field that was never interacted with.
*   **Localization:** A default value that is sensible in one locale (e.g., Monday as the start of the week) may be incorrect in another.

## Related Topics
*   **101 Form Validation:** How defaults interact with required field constraints.
*   **105 User Session Management:** How session data persists to provide dynamic defaults.
*   **202 Accessibility in UI:** Ensuring screen readers correctly identify pre-filled values.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |