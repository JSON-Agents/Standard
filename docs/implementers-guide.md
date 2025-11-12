# Implementer’s Guide

> A practical reference for developers integrating **JSON Agents** into their frameworks, SDKs, or orchestrators.

---

## Purpose

This guide explains **how to parse, validate, and operationalize JSON Agents manifests** within different environments.

It is written for developers building:
- Agent runtimes
- Orchestration layers
- SDKs, CLIs, and DevTools
- Memory or graph management systems

---

## 1. Core Structure

An **JSON Agents manifest** is a JSON object adhering to the canonical schema:  
[`/schema/json-agents.json`](../schema/json-agents.json)

At minimum, it MUST include:

| Field | Type | Required | Description |
|--------|------|-----------|-------------|
| `manifest_version` | string | ✅ | Version of the specification (e.g., `"1.0"`). |
| `profiles` | array | ✅ | List of profiles implemented (`["core"]`, etc.). |
| `agent` | object | ✅ | Identity, name, description, and metadata. |
| `capabilities` | array | ✅ | List of declared capability descriptors. |
| `tools` | array | ❌ | Optional list of callable functions or APIs. |

---

### Example (Core Manifest)

```json
{
  "manifest_version": "1.0",
  "profiles": ["core"],
  "agent": {
    "id": "ajson://example/core-agent",
    "name": "Core Agent",
    "description": "A minimal text agent"
  },
  "capabilities": [
    { "id": "echo", "description": "Echo input text" }
  ],
  "modalities": { "input": ["text"], "output": ["text"] }
}
````

---

## 2. Validation

JSON Agents uses **JSON Schema 2020-12**.
Validation ensures compatibility and predictable interoperability.

You can validate manifests using **AJV**, **Python jsonschema**, or similar tools.

### Example (Node.js + AJV)

```bash
npm install ajv-cli
ajv validate -s schema/json-agents.json -d examples/core.json
```

### Example (Python)

```python
from jsonschema import validate
import json

schema = json.load(open("schema/json-agents.json"))
manifest = json.load(open("examples/core.json"))

validate(instance=manifest, schema=schema)
print("Manifest is valid ✅")
```

---

## 3. Profiles and Conditional Fields

Each **profile** adds a set of properties and validation rules:

| Profile | Adds Fields                                               | Validation Notes                     |
| ------- | --------------------------------------------------------- | ------------------------------------ |
| `core`  | `agent`, `capabilities`, `tools`, `modalities`, `context` | Always required                      |
| `exec`  | `runtime`                                                 | Optional resource hints              |
| `gov`   | `security`, `policies`, `observability`                   | For enterprise-grade implementations |
| `graph` | `graph`, `nodes`, `edges`                                 | Enables multi-agent orchestration    |

### Conditional Validation

If a profile is declared, all its required fields MUST be present.
Schemas reference `/schema/` submodules for each section.

---

## 4. Linking Capabilities and Tools

Capabilities and tools can reference their respective schema definitions:

```json
{
  "id": "summarization",
  "schema": "https://jsonagents.org/json/schema/capabilities/summarization.json"
}
```

Tools should specify their type via the canonical registry:

```json
{
  "id": "tool://summarizer/local",
  "type": "function",
  "input_schema": { "type": "string" },
  "output_schema": { "type": "string" }
}
```

---

## 5. Using Extensions

JSON Agents supports **non-breaking innovation** via the `x-*` namespace:

```json
{
  "x-research": {
    "hypothesis": "Agents with memory outperform stateless agents by 20%"
  }
}
```

Implementers SHOULD ignore unknown `x-*` keys to preserve compatibility.

---

## 6. Security and Policies

When the **`gov`** profile is present, the following fields apply:

| Field              | Description                                             |
| ------------------ | ------------------------------------------------------- |
| `security.sandbox` | Runtime isolation method (`process`, `container`, etc.) |
| `policies`         | Declarative rules for restricting or auditing behavior  |
| `observability`    | Logging and tracing metadata                            |

Policies use simple condition expressions to define enforcement:

```json
{
  "id": "deny-external-http",
  "effect": "deny",
  "action": "tool.call",
  "where": "tool.endpoint !~ 'internal.corp'"
}
```

---

## 7. Message Exchange

Agents in a **graph** topology exchange structured messages using the canonical envelope:

```json
{
  "id": "msg-001",
  "from": "router",
  "to": "faq",
  "timestamp": "2025-11-09T12:34:56Z",
  "payload": { "question": "What is JSON Agents?" }
}
```

Envelope schema:
[`/schema/message-envelope.json`](../schema/message-envelope.json)

---

## 8. Interoperability Layers

JSON Agents manifests can be parsed by:

| System               | Compatibility Strategy                                           |
| -------------------- | ---------------------------------------------------------------- |
| **LangChain / LCEL** | Map each `tool` to a chain or tool function                      |
| **OpenAI Agents**    | Import as `manifest.json` equivalents                            |
| **AutoGen**          | Convert `graph` to conversation topology                         |
| **Custom Runtimes**  | Resolve `runtime.entrypoint` and inject capabilities dynamically |

---

## 9. Versioning and Upgrades

* Manifests specify `"manifest_version": "1.0"`.
* Validation tools SHOULD accept compatible minor versions (e.g. `1.x`).
* Deprecations are documented in [`CHANGELOG.md`](../CHANGELOG.md).

---

## 10. Best Practices

✅ Always include a `version` in `agent`.
✅ Use canonical capability IDs where possible.
✅ Store sensitive data references in a vault, not inline.
✅ Validate manifests in CI/CD pipelines.
✅ Keep context window sizes reasonable (`≤ 16k` tokens recommended).

---

## References

* [RFC 8259: JSON Data Interchange Syntax](https://datatracker.ietf.org/doc/html/rfc8259)
* [JSON Schema 2020-12](https://json-schema.org)
* [ISO/IEC 21778:2017](https://www.iso.org/standard/71616.html)

---

© 2025 JSON Agents Project. All rights reserved.


