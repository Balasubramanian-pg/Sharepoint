# 031 String Functions

Canonical documentation for 031 String Functions. This document defines concepts, terminology, and standard usage.

## Purpose
String functions exist to provide a standardized interface for the manipulation, inspection, and transformation of character sequences (strings). In computational systems, strings represent the primary medium for human-readable data and structured text-based protocols. The 031 String Functions domain addresses the requirement for deterministic operations that allow systems to parse, format, and validate textual data efficiently while maintaining data integrity across different character encodings and locales.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* **Transformation:** Operations that modify the content or structure of a string.
* **Inspection:** Operations that return metadata or boolean states about a string.
* **Extraction:** Operations that retrieve specific segments of a string.
* **Comparison:** Logic for determining equality, ordering, and similarity.
* **Search:** Algorithms for locating patterns within a character sequence.

**Out of scope:**
* Specific vendor implementations (e.g., specific syntax for Python, SQL, or C++).
* Hardware-level character rendering or font rasterization.
* Network protocol-specific framing (though the payload strings are in scope).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **String** | An ordered sequence of characters, typically terminated or length-prefixed. |
| **Substring** | A contiguous sequence of characters within a larger string. |
| **Index** | A numerical representation of a character's position within the sequence. |
| **Delimiter** | A character or sequence used to define boundaries between independent regions in text. |
| **Concatenation** | The operation of joining two or more strings end-to-end. |
| **Padding** | The addition of non-significant characters to a string to reach a required length. |
| **Normalization** | The process of converting a string into a standard format (e.g., case folding or Unicode decomposition). |
| **Immutability** | A property where a string cannot be modified after creation; operations return a new string instead. |

## Core Concepts
### Character Representation
Strings are composed of characters, which are abstract symbols mapped to numerical values via an encoding (e.g., ASCII, UTF-8). String functions must account for the underlying encoding, particularly when dealing with multi-byte characters where one "character" may not equal one "byte."

### Indexing Models
Functions rely on an indexing model to reference positions.
*   **Zero-based:** The first character is at position 0.
*   **One-based:** The first character is at position 1.
*   **Negative Indexing:** Referencing positions from the end of the string (e.g., -1 is the last character).

### Predicate vs. Transformative Functions
*   **Predicate Functions:** Return a boolean value based on a condition (e.g., `startsWith`, `contains`).
*   **Transformative Functions:** Return a modified version of the input string (e.g., `trim`, `replace`).

## Standard Model
The standard model for 031 String Functions categorizes operations into five functional groups:

1.  **Structural Modification:**
    *   `Concatenate`: Joins $S_1, S_2, ... S_n$.
    *   `Trim`: Removes whitespace or specified characters from the boundaries.
    *   `Pad`: Extends string length by appending/prepending characters.
2.  **Sub-string Extraction:**
    *   `Slice/Substring`: Extracts characters between index $A$ and $B$.
    *   `Split`: Divides a string into an array based on a delimiter.
    *   `Left/Right`: Extracts $N$ characters from the beginning or end.
3.  **Search and Localization:**
    *   `Find/IndexOf`: Returns the first occurrence of a pattern.
    *   `LastIndexOf`: Returns the final occurrence of a pattern.
    *   `Match`: Validates the string against a regular expression or pattern.
4.  **Transformation:**
    *   `Replace`: Substitutes occurrences of $Pattern_A$ with $Pattern_B$.
    *   `ChangeCase`: Converts to Upper, Lower, Title, or Camel case.
    *   `Reverse`: Inverts the order of characters.
5.  **Inspection:**
    *   `Length`: Returns the count of characters.
    *   `IsEmpty`: Returns true if length is zero.
    *   `IsNumeric/IsAlpha`: Validates the character set of the content.

## Common Patterns
### Method Chaining
Applying multiple transformations in a single fluent sequence (e.g., `String.Trim().ToLower().Replace(" ", "_")`).

### Sanitization
The use of string functions to strip potentially harmful characters (e.g., HTML tags or SQL injection vectors) before processing data in a sensitive context.

### Template Interpolation
Replacing placeholders within a static "boilerplate" string with dynamic values (e.g., `Hello, {name}`).

## Anti-Patterns
*   **Manual Iteration for Built-ins:** Writing custom loops to find a character or change case instead of using optimized standard library functions.
*   **Hard-coded Indices:** Using "magic numbers" for substring extractions which break when the input format changes slightly.
*   **Ignoring Locale:** Assuming case-folding (Upper/Lower) is identical across all languages (e.g., the Turkish "I" problem).
*   **Repeated Concatenation in Loops:** In immutable string systems, joining strings inside a loop creates excessive memory allocations; using a "Builder" or "Join" pattern is preferred.

## Edge Cases
*   **The Empty String (""):** Functions must define whether an empty string is a valid input and what it returns (e.g., `split` on an empty string).
*   **Null/Undefined References:** Distinguishing between a string with no characters and the absence of a string object.
*   **Multi-byte/Surrogate Pairs:** Functions that count "length" based on bytes rather than grapheme clusters may return incorrect results for emojis or complex scripts.
*   **Boundary Overlap:** When a `Replace` operation creates a new sequence that matches the search pattern again (Recursive vs. Non-recursive replacement).

## Related Topics
*   **032 Regular Expressions:** Advanced pattern matching and complex string manipulation.
*   **014 Data Encoding:** Standards for character sets (UTF-8, UTF-16).
*   **055 Internationalization (i18n):** Handling locale-specific string behavior.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |