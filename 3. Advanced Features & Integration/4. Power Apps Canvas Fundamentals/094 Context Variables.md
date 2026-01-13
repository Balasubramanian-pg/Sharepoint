# 094 Context Variables

Canonical documentation for 094 Context Variables. This document defines concepts, terminology, and standard usage.

## Purpose
The 094 Context Variables standard addresses the challenge of managing "ambient state" across decoupled execution flows. In complex systems, certain metadata—such as authentication identities, correlation IDs, or localization preferences—must be accessible across multiple layers of an application without being explicitly passed through every function signature (a problem known as "parameter drilling").

Context Variables provide a mechanism for storing and retrieving values based on the current execution path, ensuring that state is isolated to specific logical flows (e.g., a single web request or a specific asynchronous task) while remaining globally accessible within that scope.

> [!NOTE]
> This documentation is intended to be implementation-agnostic and authoritative.

## Scope
Clarify what is in scope and out of scope for this topic.

**In scope:**
* The lifecycle and propagation of context-bound data.
* Logical isolation of state within concurrent or asynchronous execution environments.
* Mechanisms for context inheritance and snapshotting.
* Theoretical requirements for thread-safety and task-safety in context management.

**Out of scope:**
* Specific language implementations (e.g., Python `contextvars`, Node.js `AsyncLocalStorage`, Java `ThreadLocal`).
* Persistent storage mechanisms (databases, caches).
* Network-level protocol headers (though these may be sources for context variables).

## Definitions
Provide precise definitions for key terms.

| Term | Definition |
|------|------------|
| **Context** | The implicit environment associated with a specific execution flow or logical unit of work. |
| **Context Variable** | A named container for data that is scoped to the current context and isolated from other concurrent contexts. |
| **Propagation** | The process of ensuring context data follows the execution flow across asynchronous boundaries or thread hand-offs. |
| **Snapshot** | A point-in-time capture of all variables within a context, often used for restoration or forking. |
| **Reification** | The act of making an implicit context explicit, usually for the purpose of serialization or logging. |
| **Scope Leak** | A failure state where context data persists beyond its intended lifecycle or bleeds into an unrelated execution flow. |

## Core Concepts

### Implicit State Management
Context variables allow for the management of state that is "ambient." Unlike global variables, which are shared across the entire process, context variables are local to the execution path. This allows disparate parts of a system to share information without a direct dependency link.

### Execution Scoping
A context is bound to a lifecycle. In a synchronous environment, this is typically the lifetime of a thread. In asynchronous or event-driven environments, the context is bound to a logical task or "chain" of callbacks.

### Isolation
Isolation ensures that even if two tasks are executing the same code simultaneously, their context variables remain distinct. Modification of a variable in Context A must never affect the value of the same variable in Context B.

## Standard Model

The standard model for 094 Context Variables relies on a **Context Manager** and a **Context Store**.

1.  **Declaration:** Variables are declared as unique keys (often namespaced) within the system.
2.  **Assignment:** A value is bound to a variable within a specific scope. This binding is typically "copy-on-write" or "shadowed" to prevent side effects in parent scopes.
3.  **Access:** When a variable is accessed, the system traverses the current execution's context stack to find the most recent value associated with that key.
4.  **Propagation:** When the execution flow branches (e.g., spawning a sub-task), the current context is either shared or cloned to the new flow.

## Common Patterns

### Correlation and Traceability
Using context variables to store a `Request-ID` or `Trace-ID` at the entry point of a system. This ID is then automatically available to all downstream logging calls without being passed as an argument.

### Security Contexts
Storing the "Current User" or "Permissions Set" after authentication. This allows low-level data access layers to perform authorization checks by querying the context rather than requiring the business logic to pass user objects downward.

### Localization and Tenancy
Storing the preferred language (locale) or the Tenant ID in multi-tenant architectures. This ensures that formatting utilities or database routers can adapt their behavior based on the ambient context.

## Anti-Patterns

### The "Junk Drawer"
Using context variables as a primary method for passing required arguments. This obscures the API contract and makes unit testing difficult, as the "hidden" dependencies are not visible in function signatures.

### Heavy Object Storage
Storing large, complex objects or database connections in context variables. Context should ideally contain lightweight metadata. Storing heavy objects can lead to memory leaks if the context lifecycle is not strictly managed.

### Manual Propagation Failure
Failing to capture and restore context when moving across execution boundaries (e.g., from a thread pool to a main event loop). This results in "lost" context, where variables suddenly return default or null values.

## Edge Cases

### Context Forking
When an execution path splits into multiple parallel paths, the system must decide if the context is shared (mutations affect all branches) or cloned (mutations are isolated). The 094 standard recommends **cloning/immutability** to prevent race conditions.

### Long-Lived Background Tasks
Tasks that outlive the request that spawned them may hold onto a context snapshot. If that snapshot contains references to large objects, it may prevent garbage collection of the original request's resources.

### Nested Overrides
If a context variable is set to `Value A`, and a nested function sets it to `Value B`, the system must ensure that once the nested function exits, the variable reverts to `Value A` for the remainder of the outer function's execution.

## Related Topics
* **042 Distributed Tracing:** Context variables are the primary vehicle for propagating trace headers.
* **015 Asynchronous Programming Models:** Defines the boundaries across which context must be propagated.
* **108 Identity and Access Management (IAM):** Often the source of data stored within security contexts.

## Change Log
| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-01-12 | Initial AI-generated canonical documentation |