# 059 Testing Best Practices

Canonical documentation for 059 Testing Best Practices. This document defines concepts, terminology, and standard usage.

## Purpose
The purpose of Testing Best Practices is to establish a framework for verifying that software systems meet specified requirements and function correctly under defined conditions. This topic addresses the inherent risks of software regression, the cost of late-stage bug discovery, and the need for sustainable velocity in development. By standardizing testing methodologies, organizations ensure high confidence in system stability and facilitate the safe evolution of complex codebases.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
*   Methodologies for automated and manual verification.
*   The hierarchy and distribution of test types (The Testing Pyramid).
*   Principles of test design, isolation, and maintainability.
*   Quality gates and integration into the software development life cycle (SDLC).

**Out of scope:**
*   Specific vendor implementations or testing frameworks (e.g., JUnit, Selenium, Jest).
*   Language-specific syntax for writing tests.
*   Hardware-specific validation procedures.

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Unit Test** | A test that verifies the smallest functional part of an application in isolation from its dependencies. |
| **Integration Test** | A test designed to verify the interface and interaction between two or more components or systems. |
| **End-to-End (E2E) Test** | A test that validates the entire software system from start to finish, including its integration with external interfaces. |
| **Regression Testing** | The practice of running previously executed tests against a new version of the software to ensure existing functionality remains intact. |
| **Determinism** | The property of a test where the same input and state always produce the same output and result. |
| **Test Double** | A generic term for any object used in place of a real dependency (includes Mocks, Stubs, and Fakes). |
| **Code Coverage** | A metric representing the percentage of the source code executed during testing. |
| **Flakiness** | A state where a test yields both passing and failing results without any changes to the code or environment. |

## Core Concepts

### Shift-Left Testing
The principle of performing testing activities as early as possible in the development process. By moving testing "left" on the project timeline, defects are identified when they are least expensive to fix.

### Test Isolation
Tests must be independent of one another. The outcome of one test should not influence the outcome of another, and tests should be executable in any order. This requires proper setup and teardown of state.

### The Feedback Loop
The primary value of testing is the speed and accuracy of the feedback it provides to the developer. High-quality testing suites minimize the time between a code change and the notification of a regression.

### Testability
A characteristic of software design that describes how easily a system can be tested. Highly testable systems exhibit low coupling, high cohesion, and clear separation of concerns.

## Standard Model

### The Testing Pyramid
The standard model for a healthy test suite is the Testing Pyramid, which dictates the volume of tests at different layers:

1.  **Unit Layer (Base):** The largest volume of tests. They are fast, inexpensive, and highly granular.
2.  **Service/Integration Layer (Middle):** Fewer tests than the unit layer. They verify the communication between modules or external services.
3.  **UI/E2E Layer (Apex):** The smallest number of tests. They are slow, expensive to maintain, and verify the system from the user's perspective.

### The AAA Pattern
The standard structural pattern for individual test cases:
*   **Arrange:** Set up the conditions and inputs required for the test.
*   **Act:** Execute the specific function or behavior being tested.
*   **Assert:** Verify that the outcome matches the expected result.

## Common Patterns

### Test-Driven Development (TDD)
A process where tests are written before the functional code. The cycle follows: Write a failing test (Red) -> Write the minimum code to pass (Green) -> Improve the code structure (Refactor).

### Behavior-Driven Development (BDD)
An extension of TDD that uses human-readable descriptions of software behaviors (often in "Given-When-Then" format) to bridge the gap between technical and non-technical stakeholders.

### Continuous Testing
The practice of executing automated tests as part of the software delivery pipeline to obtain immediate feedback on the business risks associated with a software release candidate.

## Anti-Patterns

### The Ice Cream Cone (Inverted Pyramid)
A scenario where the majority of tests are high-level E2E or manual tests, with very few unit tests. This leads to slow execution, high maintenance costs, and difficult debugging.

### Testing Implementation Details
Writing tests that rely on the internal workings of a function rather than its public API. This makes the test suite brittle, as refactoring the internal logic breaks the tests even if the output remains correct.

### Logic in Tests
Including complex conditional logic or loops within test code. Tests should be simple and declarative; complex test logic introduces the risk of bugs within the tests themselves.

### The "Slow Build"
Allowing the test suite to become so large or inefficient that it takes hours to run. This discourages frequent execution and destroys the rapid feedback loop.

## Edge Cases

### Non-Deterministic Dependencies
Testing code that relies on volatile factors such as system time, random number generators, or external network availability. These must be controlled via injection or doubles to ensure deterministic results.

### Asynchronous Operations
Testing logic that completes at an indeterminate time. This requires specialized handling (e.g., polling, callbacks, or promises) to avoid "race conditions" where the assertion runs before the action completes.

### Legacy Codebases
Systems without existing tests often lack the "seams" necessary for isolation. Testing in these environments usually requires high-level characterization tests before refactoring into more granular unit tests.

## Related Topics
*   **042 Continuous Integration/Continuous Deployment (CI/CD)**
*   **078 Software Architecture Patterns**
*   **102 Observability and Monitoring**
*   **115 Security Verification Standards**

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |