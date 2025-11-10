# JSON Agents  
## Portable Agent Manifest (PAM) Specification  
Category: Standards Track  
November 2025  

---

## Status of this Memo

This document defines the **JSON Agents - Portable Agent Manifest (PAM)**, a JSON-based standard for describing AI agents and agent graphs in a portable, schema-driven format.

This specification is distributed for public implementation, review, and discussion.  
Distribution is unlimited.

---

## Copyright Notice

Copyright © 2025 JSON Agents.  
All Rights Reserved.

---

## Abstract

**JSON Agents** defines the **Portable Agent Manifest (PAM)** — a universal, JSON-native format for representing AI agents, their capabilities, tools, runtimes, and governance metadata.  

PAM provides a single, interoperable structure that allows agents to be described, validated, and exchanged across frameworks, platforms, and ecosystems without code translation.

PAM is derived entirely from established **JSON and JSON Schema** standards and designed for compatibility, governance, and multi-agent orchestration.

---

## Table of Contents

1. Introduction  
2. Requirements and Conventions  
3. Terminology  
4. Design Overview  
5. Top-Level Manifest Structure  
6. Core Profile (`core`)  
7. Exec Profile (`exec`)  
8. Gov Profile (`gov`)  
9. Graph Profile (`graph`)  
10. Extensions and Namespaces  
11. Conformance  
12. Security Considerations  
13. Example Manifests  
14. Registry Considerations  
15. IANA Considerations  
16. URI Scheme Definition  
17. References  
Appendix A. Normative JSON Schema  
Appendix B. Policy Expression Language

---

## 1. Introduction

### 1.1 Motivation

The agent ecosystem has evolved through a patchwork of incompatible manifest formats — each framework defining unique metadata structures, tool schemas, and governance models.  
This fragmentation prevents portability, interoperability, and consistent validation.

JSON Agents introduces the **Portable Agent Manifest (PAM)** as a shared schema for defining AI agents declaratively.  
The goal is to make agents:

- **Framework-agnostic**
- **Interoperable across runtimes**
- **Governable at scale**
- **Composable into networks**

### 1.2 Scope

PAM defines:

- A top-level JSON structure for agent manifests.  
- Four modular profiles:
  - `core` — identity, capabilities, tools, context.
  - `exec` — runtime and environment.
  - `gov` — security, policies, observability.
  - `graph` — multi-agent orchestration and routing.
- Validation through JSON Schema.
- A reserved extension model for innovation.

### 1.3 Non-Goals

PAM does **not** define:

- A transport or API protocol.  
- Prompt formatting or reasoning algorithms.  
- Scheduling, execution order, or model architecture.

---

## 2. Requirements and Conventions

### 2.1 Requirements Language

The key words **MUST**, **SHOULD**, and **MAY** are to be interpreted as described in RFC 2119.

### 2.2 JSON and Encoding

- Manifests MUST conform to RFC 8259, ECMA-404, and ISO/IEC 21778.  
- Encoding MUST be UTF-8.  
- Validation MUST be compatible with JSON Schema 2020-12.

---

## 3. Terminology

| Term | Definition |
|------|-------------|
| **Agent** | A logical AI entity capable of processing input, invoking tools, and generating output. |
| **Manifest** | A JSON document describing an agent or network of agents. |
| **Profile** | A modular subset of the specification (e.g., core, exec, gov, graph). |
| **Capability** | A declared function or skill the agent provides. |
| **Tool** | An external callable resource (API, function, system, or plugin). |
| **Runtime** | The environment in which the agent executes. |
| **Policy** | A declarative constraint governing behavior. |
| **Security** | Configuration related to sandboxing and access control. |
| **Graph** | A collection of interconnected agents and message routes. |
| **Message Envelope** | A structured format for messages passed between agents. |
| **Extension** | A namespaced addition to the standard manifest fields. |

---

## 4. Design Overview

### 4.1 Profiles

Manifests MAY declare one or more profiles:

```json
"profiles": ["core", "exec", "gov", "graph"]
````

If omitted, `core` is assumed.

Each profile adds semantic layers:

* **Core**: Base identity and capabilities.
* **Exec**: Runtime execution context.
* **Gov**: Governance, security, and observability.
* **Graph**: Multi-agent orchestration.

### 4.2 Manifest and Instance Relationship

A manifest describes a **template**.
Implementations MAY instantiate one or more live agents based on it.

---

## 5. Top-Level Manifest Structure

A conforming manifest MUST include at least:

```json
{
  "manifest_version": "1.0",
  "profiles": ["core"],
  "agent": {},
  "capabilities": [],
  "tools": [],
  "modalities": {},
  "context": {}
}
```

Top-level fields:

| Field              | Type   | Description                       |
| ------------------ | ------ | --------------------------------- |
| `manifest_version` | string | Identifies specification version. |
| `profiles`         | array  | Declares active profiles.         |
| `agent`            | object | Agent metadata.                   |
| `capabilities`     | array  | Agent functions.                  |
| `tools`            | array  | External callable resources.      |
| `modalities`       | object | Input/output formats.             |
| `context`          | object | Memory and windowing hints.       |

---

## 6. Core Profile (`core`)

### 6.1 `agent`

```json
"agent": {
  "id": "ajson://example/echo",
  "name": "Echo Agent",
  "description": "Echoes back text.",
  "version": "1.0.0",
  "license": "Apache-2.0"
}
```

### 6.2 `capabilities`

```json
"capabilities": [
  { "id": "echo", "description": "Echo input text." }
]
```

### 6.3 `tools`

```json
"tools": [
  {
    "id": "tool://search",
    "name": "Search API",
    "type": "http",
    "endpoint": "https://api.example.com/search",
    "input_schema": { "type": "object" },
    "output_schema": { "type": "object" }
  }
]
```

### 6.4 `modalities`

```json
"modalities": {
  "input": ["text"],
  "output": ["text", "json"]
}
```

### 6.5 `context`

```json
"context": { "window": 4096, "persistent": false }
```

---

## 7. Exec Profile (`exec`)

### 7.1 `runtime`

```json
"runtime": {
  "type": "python",
  "entrypoint": "main:run",
  "env": { "MODE": "production" },
  "resources": { "cpu_cores_min": 1, "memory_mb_min": 512 }
}
```

---

## 8. Gov Profile (`gov`)

### 8.1 `security`

```json
"security": {
  "sandbox": "container",
  "network_zone": "trusted"
}
```

### 8.2 `policies`

```json
"policies": [
  {
    "id": "deny-external-http",
    "effect": "deny",
    "action": "tool.call",
    "where": "tool.endpoint !~ 'internal'"
  }
]
```

### 8.3 `observability`

```json
"observability": {
  "log_level": "info",
  "metrics_enabled": true
}
```

---

## 9. Graph Profile (`graph`)

### 9.1 `graph.nodes`

```json
"graph": {
  "nodes": [
    { "id": "router", "ref": "ajson://example/router" },
    { "id": "faq", "ref": "ajson://example/faq" }
  ]
}
```

### 9.2 `graph.edges`

```json
"graph": {
  "edges": [
    { "from": "router", "to": "faq", "condition": "message.intent == 'faq'" }
  ]
}
```

### 9.3 Message Envelope

```json
"graph": {
  "message_envelope": {
    "schema": "https://jsonagents.org/schema/message-envelope.json"
  }
}
```

---

## 10. Extensions and Namespaces

### 10.1 `extensions`

```json
"extensions": {
  "vendor.example/routing": { "shadow_mode": true }
}
```

### 10.2 `x-*`

```json
"x-internal": { "notes": "for testing only" }
```

Implementations MUST ignore unrecognized extensions.

---

## 11. Conformance

A manifest is conformant if:

* It validates against the JSON Agents schema.
* Declared profiles match included sections.
* No unknown non-extension fields are present.

---

## 12. Security Considerations

* Manifests MUST NOT embed secrets.
* Evaluators MUST NOT execute manifest content.
* Policy failures SHOULD fail closed.

---

## 13. Example Manifests

### 13.1 Minimal

```json
{
  "manifest_version": "1.0",
  "agent": { "id": "ajson://example/minimal", "name": "Minimal" }
}
```

### 13.2 Full Example

```json
{
  "manifest_version": "1.0",
  "profiles": ["core", "exec", "gov", "graph"],
  "agent": { "id": "ajson://example/router", "name": "Router" },
  "runtime": { "type": "node", "entrypoint": "dist/router.js" },
  "security": { "sandbox": "process" },
  "graph": {
    "nodes": [
      { "id": "router", "ref": "ajson://example/router" },
      { "id": "faq", "ref": "ajson://example/faq" }
    ],
    "edges": [
      { "from": "router", "to": "faq", "condition": "message.intent == 'faq'" }
    ]
  }
}
```

---

## 14. Registry Considerations

Implementations MAY maintain public registries for:

* Capabilities
* Tool types
* Profiles
* Envelope schemas

---

## 15. IANA Considerations

### 15.1 Media Type

JSON Agents manifests SHOULD use the media type:

```
application/agents+json
```

This media type is intended for registration with IANA and follows the structured syntax suffix convention defined in RFC 6838.

### 15.2 File Extension

The recommended file extension for JSON Agents manifests is:

```
.agents.json
```

This extension clearly identifies agent manifest files while maintaining compatibility with standard JSON tooling.

### 15.3 Content Negotiation

Servers and clients MAY use standard HTTP content negotiation to request or serve JSON Agents manifests:

```http
Accept: application/agents+json
Content-Type: application/agents+json
```

---

## 16. URI Scheme Definition

### 16.1 Purpose

The `ajson://` URI scheme provides a standardized method for uniquely identifying and referencing agent manifests, capabilities, and other JSON Agents resources.

### 16.2 Syntax

The `ajson://` URI scheme follows the generic URI syntax defined in RFC 3986:

```
ajson-uri = "ajson://" authority path [ "?" query ] [ "#" fragment ]

authority = [ userinfo "@" ] host [ ":" port ]
path      = "/" segment *( "/" segment )
segment   = *pchar
```

**Examples**:
```
ajson://jsonagents.org/examples/router-hub
ajson://example.com/agents/summarizer/v2.1.0
ajson://registry.internal/capabilities/routing
ajson://localhost:8080/test/agent#metadata
```

### 16.3 Identifier Construction

#### 16.3.1 Authority Component

The authority SHOULD represent:
- A DNS domain under the manifest publisher's control, OR
- A well-known registry service, OR
- `localhost` for local/development manifests

#### 16.3.2 Path Component

The path MUST:
- Begin with a forward slash (`/`)
- Uniquely identify the resource within the authority's namespace
- Use URL-safe characters (RFC 3986)

The path MAY include:
- Organizational hierarchy (e.g., `/org/team/agent`)
- Version information (e.g., `/agent/v1.2.3`)
- Resource type indicators (e.g., `/capabilities/summarization`)

#### 16.3.3 Fragment Component

The fragment identifier MAY be used to reference:
- Specific sections within a manifest (e.g., `#runtime`)
- Individual capabilities (e.g., `#capability/summarization`)
- Graph nodes (e.g., `#node/router`)

### 16.4 Resolution Mechanism

#### 16.4.1 Resolution Protocol

Implementations SHOULD resolve `ajson://` URIs through one of the following methods:

1. **HTTPS Transformation** (RECOMMENDED):
   ```
   ajson://example.com/agents/router
   → https://example.com/.well-known/agents/router.agents.json
   ```

2. **Direct Registry Lookup**:
   Query a configured registry service using the full URI as an identifier.

3. **Local Cache**:
   Check local manifest cache before attempting network resolution.

#### 16.4.2 Well-Known URI

Implementations using HTTPS transformation SHOULD:
- Map `ajson://` to `https://` with `.well-known/agents/` path prefix
- Append `.agents.json` extension if not present
- Follow HTTP redirects (3xx status codes)
- Cache responses according to HTTP caching headers

#### 16.4.3 Resolution Algorithm

```
function resolve(ajsonURI):
  1. Parse URI into components (authority, path, fragment)
  2. Check local cache for authority + path
  3. If cached and not expired, return cached manifest
  4. Transform to HTTPS well-known URI
  5. Perform HTTP GET with Accept: application/agents+json
  6. Validate response against json-agents.json schema
  7. Cache response if valid
  8. If fragment present, extract referenced component
  9. Return resolved manifest or component
```

#### 16.4.4 Error Handling

Implementations MUST handle resolution failures gracefully:

| Condition | Behavior |
|-----------|----------|
| Network unavailable | Use cached version if available, otherwise fail |
| 404 Not Found | Return resolution error with original URI |
| Invalid manifest | Return validation error with details |
| Timeout | Retry with exponential backoff, max 3 attempts |

### 16.5 Registry Service Requirements

#### 16.5.1 Registry Responsibilities

A conformant registry service MUST:
- Accept `ajson://` URIs as identifiers
- Store and serve valid JSON Agents manifests
- Implement versioning for manifest updates
- Provide discovery endpoints for browsing
- Support content negotiation (JSON/YAML)

#### 16.5.2 Registry API (Non-Normative)

Example registry interface:

```http
GET /resolve?uri=ajson://example.com/agents/router
Accept: application/agents+json

Response:
200 OK
Content-Type: application/agents+json
{
  "manifest_version": "1.0",
  ...
}
```

#### 16.5.3 Metadata API

Registries SHOULD provide metadata endpoints:

```http
GET /metadata?uri=ajson://example.com/agents/router

Response:
{
  "uri": "ajson://example.com/agents/router",
  "versions": ["1.0.0", "1.0.1", "1.1.0"],
  "latest": "1.1.0",
  "published": "2025-11-09T12:00:00Z",
  "deprecated": false
}
```

### 16.6 Versioning and Immutability

#### 16.6.1 Version Semantics

URIs MAY encode version information:
- **Immutable**: `ajson://example.com/agent/v1.2.3` (exact version)
- **Mutable**: `ajson://example.com/agent` (latest version)
- **Major**: `ajson://example.com/agent/v1` (latest v1.x.x)

#### 16.6.2 Immutability Recommendation

For production deployments, manifests SHOULD reference immutable versioned URIs to ensure reproducibility.

### 16.7 Security Considerations

#### 16.7.1 Transport Security

- All resolution over networks MUST use TLS 1.2 or higher
- Certificate validation MUST be performed
- HTTPS redirects MUST NOT downgrade to HTTP

#### 16.7.2 Manifest Integrity

Implementations SHOULD:
- Verify manifest signatures if `signatures` field is present
- Validate Content-Type header matches expected media type
- Reject manifests with invalid schemas
- Implement Content Security Policy for web-based resolvers

#### 16.7.3 URI Validation

Implementations MUST:
- Validate URI syntax before resolution attempts
- Reject URIs with embedded credentials (userinfo)
- Sanitize path components to prevent directory traversal
- Implement rate limiting for resolution requests

#### 16.7.4 Cache Poisoning Prevention

- Use cryptographic hashes for cache keys
- Implement cache-control header validation
- Provide cache invalidation mechanisms
- Log resolution attempts for audit trails

#### 16.7.5 Privacy

- Do not include sensitive information in URIs
- Implement access control for private manifests
- Consider DNS privacy implications
- Provide opt-out for telemetry/analytics

### 16.8 IANA Considerations for URI Scheme

The `ajson://` URI scheme is intended for registration with IANA under the Uniform Resource Identifier (URI) Schemes registry.

**Scheme Name**: ajson  
**Status**: Provisional  
**Applications/Protocols**: JSON Agents Portable Agent Manifest  
**Contact**: JSON Agents Working Group  
**Change Controller**: JSON Agents Steering Committee  

---

## 17. References

* RFC 8259 — The JavaScript Object Notation (JSON) Data Interchange Format
* ECMA-404 — The JSON Data Interchange Syntax
* ISO/IEC 21778:2017 — JSON Data Interchange Syntax
* JSON Schema 2020-12
* JSON-LD 1.1
* RFC 6838 — Media Type Specifications and Registration Procedures
* RFC 3986 — Uniform Resource Identifier (URI): Generic Syntax
* RFC 2119 — Key words for use in RFCs to Indicate Requirement Levels

---

## Appendix A. Normative JSON Schema

See [`schema/json-agents.json`](./schema/json-agents.json) for the normative schema definition.

---

## Appendix B. Policy Expression Language

### B.1 Purpose

This appendix defines the expression language used in `policy.where` clauses for declarative access control and behavior constraints.

### B.2 Design Philosophy

The policy expression language is:
- **Simple**: Minimal syntax for common use cases
- **Safe**: No code execution or side effects
- **Deterministic**: Same input always produces same result
- **JSONPath-like**: Familiar to developers using JSON query languages

### B.3 Grammar

```ebnf
expression     = comparison | logical_expr | literal

comparison     = accessor operator value
logical_expr   = expression logical_op expression
               | "(" expression ")"
               | "not" expression

accessor       = identifier *("." identifier | "[" index "]")
identifier     = ALPHA *(ALPHA | DIGIT | "_")
index          = DIGIT+ | STRING

operator       = "==" | "!=" | ">" | "<" | ">=" | "<="
               | "~" | "!~" | "in" | "not in"
               | "contains" | "starts_with" | "ends_with"

logical_op     = "&&" | "||" | "and" | "or"

value          = STRING | NUMBER | BOOLEAN | NULL | array
array          = "[" [value *("," value)] "]"

STRING         = "'" *CHAR "'"
NUMBER         = ["-"] DIGIT+ ["." DIGIT+]
BOOLEAN        = "true" | "false"
NULL           = "null"
```

### B.4 Operators

#### B.4.1 Comparison Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `==` | Equality | `tool.type == 'http'` |
| `!=` | Inequality | `tool.type != 'system'` |
| `>` | Greater than | `message.priority > 5` |
| `<` | Less than | `context.window < 8192` |
| `>=` | Greater or equal | `runtime.memory_mb_min >= 512` |
| `<=` | Less or equal | `runtime.cpu_cores_min <= 4` |

#### B.4.2 String Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `~` | Regex match | `tool.endpoint ~ '^https://internal\\.corp'` |
| `!~` | Regex non-match | `tool.endpoint !~ 'external'` |
| `contains` | Substring test | `message.payload contains 'urgent'` |
| `starts_with` | Prefix test | `agent.id starts_with 'ajson://internal'` |
| `ends_with` | Suffix test | `tool.endpoint ends_with '.internal.corp'` |

#### B.4.3 Collection Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `in` | Membership test | `tool.type in ['http', 'function']` |
| `not in` | Non-membership | `tool.type not in ['system', 'plugin']` |

#### B.4.4 Logical Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `&&` / `and` | Logical AND | `tool.type == 'http' && tool.auth.method == 'none'` |
| `\|\|` / `or` | Logical OR | `message.priority > 8 \|\| message.urgent == true` |
| `not` | Logical NOT | `not (tool.type in ['system', 'plugin'])` |

### B.5 Operator Precedence

From highest to lowest:

1. **Parentheses**: `( )`
2. **Unary**: `not`
3. **Comparison**: `==`, `!=`, `>`, `<`, `>=`, `<=`
4. **String/Collection**: `~`, `!~`, `in`, `not in`, `contains`, `starts_with`, `ends_with`
5. **Logical AND**: `&&`, `and`
6. **Logical OR**: `||`, `or`

### B.6 Context Variables

Expressions evaluate against a **context object** containing:

```javascript
{
  "tool": {          // Current tool being invoked
    "id": "...",
    "type": "...",
    "endpoint": "...",
    ...
  },
  "message": {       // Current message envelope
    "from": "...",
    "to": "...",
    "payload": {...},
    "intent": "...",
    ...
  },
  "agent": {         // Current agent manifest
    "id": "...",
    "name": "...",
    ...
  },
  "runtime": {       // Runtime context
    "environment": "production",
    "timestamp": "2025-11-10T00:00:00Z",
    ...
  }
}
```

### B.7 Examples

#### B.7.1 Simple Comparison
```json
{
  "id": "deny-http-no-auth",
  "effect": "deny",
  "action": "tool.call",
  "where": "tool.type == 'http' && tool.auth.method == 'none'"
}
```

#### B.7.2 Regex Pattern Matching
```json
{
  "id": "deny-external-endpoints",
  "effect": "deny",
  "action": "tool.call",
  "where": "tool.endpoint !~ '^https://.*\\.internal\\.corp'"
}
```

#### B.7.3 Complex Logic
```json
{
  "id": "audit-sensitive-messages",
  "effect": "audit",
  "action": "message.send",
  "where": "(message.payload contains 'password' || message.payload contains 'api_key') && not (message.to starts_with 'ajson://internal')"
}
```

#### B.7.4 Collection Membership
```json
{
  "id": "allow-safe-tools",
  "effect": "allow",
  "action": "tool.call",
  "where": "tool.type in ['function', 'http'] && tool.id in ['tool://safe/search', 'tool://safe/summarize']"
}
```

### B.8 Evaluation Semantics

#### B.8.1 Type Coercion

- Comparisons between incompatible types evaluate to `false`
- String comparisons are case-sensitive
- Numbers are compared numerically
- Booleans compare by value (`true` > `false`)

#### B.8.2 Missing Fields

- Accessing non-existent fields returns `null`
- `null == null` evaluates to `true`
- `null` compared to any non-null value evaluates to `false`

#### B.8.3 Short-Circuit Evaluation

- `&&`: If left operand is `false`, right operand is not evaluated
- `||`: If left operand is `true`, right operand is not evaluated

### B.9 Implementation Guidance

#### B.9.1 Security

- MUST NOT execute arbitrary code
- MUST limit recursion depth (recommended: 10 levels)
- MUST prevent regex denial-of-service (ReDoS)
- SHOULD implement expression complexity limits

#### B.9.2 Performance

- Evaluate expressions in constant or linear time when possible
- Cache compiled expressions
- Optimize common patterns (e.g., simple equality)

#### B.9.3 Error Handling

- Syntax errors SHOULD fail policy evaluation (fail-closed)
- Runtime errors (e.g., type mismatch) MAY log warnings
- Invalid regex patterns MUST fail at compile time

### B.10 Future Extensions

Future versions MAY add:
- Functions: `length()`, `upper()`, `lower()`, `trim()`
- Array operations: `map()`, `filter()`, `any()`, `all()`
- Temporal operators: `before`, `after`, `within`
- Quantifiers: `exists`, `forall`

Extensions MUST maintain backward compatibility with this grammar.

