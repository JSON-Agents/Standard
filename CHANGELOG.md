# Changelog  
All notable changes to the JSON Agents specification will be documented in this file.

This project adheres to [Semantic Versioning 2.0.0](https://semver.org/).

---

## [1.0.0] — 2025-11-10  
**Initial Publication**

### Added
- **JSON Agents Specification (`json-agents.md`)**  
  - Defines the **Portable Agent Manifest (PAM)** model.  
  - Includes normative structure for `core`, `exec`, `gov`, and `graph` profiles.  
  - Provides formal terminology, examples, and conformance sections.

- **Canonical JSON Schema (`schema/json-agents.json`)**  
  - Implements validation for all core fields and conditional profile requirements.  
  - Supports `extensions` and `x-*` namespaces.  
  - Ensures JSON Schema 2020-12 compatibility.

- **Repository Layout and Supporting Files**  
  - `README.md` with overview and quick reference.  
  - `LICENSE` under Apache 2.0.  
  - `CONTRIBUTING.md` for guidelines and validation workflow.  
  - `CODE_OF_CONDUCT.md` with Contributor Covenant 2.0.
  - Directory structure for `/schema`, `/examples`, `/registry`, `/docs`.

- **Extension System**
  - Extension registry (`registry/extensions.json`) with 4 registered extensions.
  - Schema definitions for `x-audit` and `x-memory` extensions.
  - Comprehensive extensions guide (`docs/extensions.md`).

- **Profile Registry**
  - Formal profile definitions (`registry/profiles.json`).
  - Documentation for extending with custom profiles.

- **Capability and Tool Registries**
  - Capabilities registry with 7 standard capabilities.
  - Tool types registry with 6 recognized types.
  - Schema definitions for summarization, routing, retrieval, qa, classification, extraction, and generation capabilities.

- **URI Scheme Definition (Section 16)**  
  - Formal specification of `ajson://` URI scheme syntax (RFC 3986 compliant).
  - Resolution mechanism with HTTPS transformation and well-known URI pattern.
  - Registry service requirements and API guidelines.
  - Comprehensive security considerations for URI resolution.
  - IANA registration considerations for URI scheme.

- **Policy Expression Language (Appendix B)**  
  - Formal grammar definition for policy `where` clauses.
  - Complete operator specification (comparison, string, collection, logical).
  - Operator precedence rules and evaluation semantics.
  - Context variable documentation.
  - Security and performance implementation guidance.
  - Examples for common policy patterns.

- **Interoperability Mappings**
  - Framework mapping guide (`docs/mapping-frameworks.md`).
  - Conversion patterns for LangChain, OpenAI, AutoGen, MCP, and others.
  - YAML ↔ JSON interoperability guidance.

- **IANA Considerations**
  - Media type: `application/agents+json`.
  - File extension: `.agents.json`.
  - Content negotiation guidelines.

### Notes
This release defines the foundation for all subsequent versions.  
Backward compatibility will be preserved whenever feasible.

---

## Versioning Policy

- **Patch** — editorial, clarification, or non-breaking corrections.  
- **Minor** — new optional fields or example additions.  
- **Major** — breaking schema or terminology changes.

---

**© 2025 JSON Agents. All rights reserved.**
