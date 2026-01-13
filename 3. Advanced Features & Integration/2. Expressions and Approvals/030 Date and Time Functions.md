# 030 Date and Time Functions

Canonical documentation for 030 Date and Time Functions. This document defines concepts, terminology, and standard usage.

## Purpose
Date and Time Functions provide a standardized computational framework for the representation, manipulation, and calculation of temporal data. Because time is subject to complex astronomical, political, and social variables (such as leap years, time zones, and daylight saving adjustments), these functions exist to abstract that complexity into a reliable set of operations. They ensure that temporal arithmetic remains consistent across different systems and that data integrity is maintained regardless of the observer's geographic location.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* Core logic for temporal arithmetic (addition, subtraction, differences).
* Extraction of temporal components (year, month, day, hour, etc.).
* Formatting and parsing logic based on international standards.
* Handling of time zones, offsets, and normalization.
* Theoretical boundaries of precision and scale.

**Out of scope:**
* Specific syntax for SQL, Python, JavaScript, or other programming languages.
* Hardware-level clock synchronization protocols (e.g., NTP).
* Historical calendar systems prior to the Gregorian calendar.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Epoch** | A fixed point in time used as a reference (zero-point) for calculating temporal values (e.g., Unix Epoch: 1970-01-01T00:00:00Z). |
| **UTC** | Coordinated Universal Time; the primary time standard by which the world regulates clocks and time. |
| **Offset** | The difference in hours and minutes from UTC for a particular place or time (e.g., +05:30). |
| **Timestamp** | A specific point in time, usually represented as a count of units from an epoch or a formatted string. |
| **Interval** | A measurement of the time elapsed between two specific points in time. |
| **Duration** | An amount of time independent of any specific start or end point (e.g., "4 hours"). |
| **Precision** | The smallest unit of time a system can represent (e.g., milliseconds, microseconds, nanoseconds). |
| **ISO 8601** | The international standard for the representation of dates and times. |

## Core Concepts

### Temporal Representation
Temporal data is generally represented in two ways:
1.  **Absolute Time:** A specific point on the global timeline (e.g., a UTC timestamp).
2.  **Wall Time (Local Time):** The time as shown on a clock in a specific location, which may or may not include an offset.

### Immutability and Determinism
Date and time functions should ideally be deterministic. Given the same input and the same system state (time zone database), the output must be identical. In modern functional paradigms, temporal objects are treated as immutable; operations return a new temporal object rather than modifying the original.

### Resolution and Scale
Functions must account for varying levels of granularity. While many business applications operate at the second or millisecond level, scientific applications may require nanosecond or picosecond precision. The "Scale" refers to the range of dates supported (e.g., the Year 2038 problem in 32-bit systems).

## Standard Model

The standard model for date and time functions relies on the **UTC-Normalization Pattern**:

1.  **Ingestion:** Parse incoming temporal strings or values into a standardized internal format (usually UTC).
2.  **Storage:** Store all absolute points in time as UTC or as a displacement from the Epoch.
3.  **Transformation:** Perform arithmetic (adding intervals) or extraction (getting the "Month") on the standardized format.
4.  **Presentation:** Apply a Time Zone offset only at the "View" or "Output" layer to convert UTC to the user's local Wall Time.

### Functional Categories
*   **Constructors:** Functions that create a timestamp from parts (Year, Month, Day) or from a string.
*   **Accessors (Getters):** Functions that extract specific components (e.g., `EXTRACT(DAY FROM timestamp)`).
*   **Arithmetic:** Functions for adding or subtracting durations (e.g., `DATE_ADD`).
*   **Difference:** Functions that calculate the span between two points (e.g., `DATEDIFF`).
*   **Formatting:** Functions that convert internal representations into human-readable strings.

## Common Patterns

### Normalization to UTC
Always convert local timestamps to UTC before performing comparisons or storage. This prevents data corruption when the system or the user moves across time zone boundaries.

### Truncation (Floor Logic)
The practice of "rounding down" a timestamp to the nearest significant unit (e.g., truncating `2023-05-12 14:30:05` to the `DAY` results in `2023-05-12 00:00:00`). This is essential for grouping data in analytical queries.

### Interval Arithmetic
Using typed durations (e.g., `INTERVAL '1 MONTH'`) rather than raw integers. This allows the function to handle the varying number of days in a month or leap years automatically.

## Anti-Patterns

### Storing Local Time Without Offsets
Storing "12:00 PM" without knowing the time zone or the UTC offset makes the data mathematically useless for global systems and impossible to sort chronologically against other regions.

### Manual Leap Year Calculation
Attempting to calculate leap years or days-in-month using custom logic (e.g., `if year % 4 == 0`) often ignores the complexities of the Gregorian calendar (the 100/400 year rule) and should be avoided in favor of built-in functions.

### String-Based Comparison
Comparing dates as strings (e.g., "12/01/2022" vs "01/12/2022") leads to incorrect sorting and logical errors. Temporal data should always be cast to a proper Date/Timestamp type before comparison.

## Edge Cases

### Daylight Saving Time (DST) Transitions
*   **The Missing Hour:** When clocks move forward, an hour of "Wall Time" does not exist. Functions must handle errors or "jump" logic when a user attempts to create a timestamp in this gap.
*   **The Ambiguous Hour:** When clocks move back, the same "Wall Time" occurs twice. Functions must have a strategy (usually picking the earlier or later UTC equivalent) to resolve this.

### Leap Seconds
Occasional adjustments made to UTC to keep it in sync with the Earth's rotation. Many systems "smear" the leap second (gradually adjusting the clock) to avoid repeating a second or breaking software that expects 60 seconds in a minute.

### The Year 2038 Problem
Systems using signed 32-bit integers to store Unix timestamps will overflow on January 19, 2038. Canonical implementations should utilize 64-bit integers to mitigate this.

### Floating Time
Events that occur at the same local time regardless of the time zone (e.g., "New Year's Day" or "Breakfast at 8:00 AM"). These should be stored without time zone offsets to preserve their intent.

## Related Topics
*   **031 Time Zone Databases (IANA/Olson):** The data underlying time zone calculations.
*   **032 ISO 8601 Standards:** The specific string formats for exchange.
*   **045 Calendar Systems:** Non-Gregorian temporal logic.

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |