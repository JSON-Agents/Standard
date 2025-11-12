# JSON Agents

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](./CHANGELOG.md)
[![License](https://img.shields.io/badge/license-Apache%202.0-green.svg)](./LICENSE)
[![JSON Schema](https://img.shields.io/badge/JSON%20Schema-2020--12-purple.svg)](https://json-schema.org/draft/2020-12/json-schema-core.html)
[![Standard](https://img.shields.io/badge/standard-RFC%208259-orange.svg)](https://datatracker.ietf.org/doc/html/rfc8259)
[![Media Type](https://img.shields.io/badge/media%20type-application%2Fagents%2Bjson-teal.svg)](#)
[![Status](https://img.shields.io/badge/status-draft-yellow.svg)](./json-agents.md)

> **A Universal JSON Specification for AI Agents**

---

> [!WARNING]
> **Draft Specification - Work in Progress**
> 
> This specification is currently in **draft status** and under active development. While the v1.0.0 release represents a complete and functional specification, it has not yet been formally adopted by any standards body or reached community consensus.
> 
> - The specification may change based on community feedback and implementation experience
> - Breaking changes are possible before final standardization
> - Early implementers should expect potential revisions
> - Contributions, feedback, and discussion are welcomed and encouraged
> 
> See [CONTRIBUTING.md](./CONTRIBUTING.md) for how to participate in the specification's development.

---

### 🌐 Overview

**JSON Agents** defines an open, JSON-native specification for describing AI agents, their capabilities, tools, runtimes, and governance in a single portable manifest called the **Portable Agent Manifest (PAM)**.

It enables frameworks, SDKs, and orchestrators to **interoperate seamlessly** — sharing agent definitions that are:
- **Human-readable**: Clear JSON structure with comprehensive documentation
- **Machine-validated**: Enforced through JSON Schema 2020-12
- **Framework-agnostic**: Works with LangChain, OpenAI, AutoGen, MCP, and more
- **Future-proof**: Extensible design with `x-*` namespaces and formal extension system

JSON Agents is based entirely on established JSON standards (RFC 8259, ECMA-404, ISO 21778) and includes formal specifications for URI schemes and policy expressions.

---

### 🧩 Core Principles

| Principle | Description |
|------------|--------------|
| **JSON-Native** | Derived from RFC 8259, ECMA-404, and ISO 21778. |
| **Schema-Validated** | Enforced through JSON Schema 2020-12. |
| **Profile-Based** | Modular profiles for `core`, `exec`, `gov`, and `graph`. |
| **Governance-Aware** | Security, policies, and observability included by design. |
| **Extensible** | `extensions` and `x-*` namespaces for safe innovation. |
| **Framework-Neutral** | Compatible with any agent runtime or framework. |
| **Formally Specified** | Complete URI scheme (`ajson://`) and policy expression language definitions. |

---

### ✨ Key Features

- **🎯 7 Standard Capabilities**: Summarization, routing, retrieval, QA, classification, extraction, and generation — all with formal schemas
- **🔗 URI Scheme**: Formal `ajson://` URI scheme with resolution mechanism and registry architecture
- **📜 Policy Language**: Complete expression language for declarative access control and governance
- **🔄 Framework Mappings**: Direct conversion paths for LangChain, OpenAI, AutoGen, MCP, and others
- **🌐 Multi-Agent Graphs**: Define orchestration topologies with conditional routing
- **🔒 Security First**: Built-in sandboxing, policies, and cryptographic signature support
- **📊 Observability**: Structured logging, metrics, and distributed tracing integration

---

### 📘 Specification

- **Main Specification:** [`json-agents.md`](./json-agents.md) — Complete PAM specification (888 lines)
- **Draft Format:** [`draft-jsonagents-spec-00.md`](./draft-jsonagents-spec-00.md) — IETF-style draft
- **Canonical Schema:** [`schema/json-agents.json`](./schema/json-agents.json) — JSON Schema 2020-12 validator

**Key Sections:**
- Section 16: [URI Scheme Definition](./json-agents.md#16-uri-scheme-definition) — `ajson://` syntax and resolution
- Appendix B: [Policy Expression Language](./json-agents.md#appendix-b-policy-expression-language) — Grammar and operators

---

### 📂 Repository Layout

```bash
/
├── README.md                      # This file
├── json-agents.md                 # Complete specification (888 lines)
├── draft-jsonagents-spec-00.md    # IETF-style draft
├── CHANGELOG.md                   # Version history
├── CONTRIBUTING.md                # Contribution guidelines
├── schema/
│   ├── json-agents.json           # Core manifest schema
│   ├── message-envelope.json      # Inter-agent message format
│   ├── capabilities/              # 7 capability schemas
│   │   ├── summarization.json
│   │   ├── routing.json
│   │   ├── retrieval.json
│   │   ├── qa.json                # Question answering
│   │   ├── classification.json    # Classification
│   │   ├── extraction.json        # Entity extraction
│   │   └── generation.json        # Content generation
│   └── extensions/                # Extension schemas
│       ├── audit.json
│       └── memory.json
├── examples/
│   ├── core.json                  # Minimal core profile
│   ├── core-exec.json             # With runtime
│   ├── core-exec-gov.json         # With governance
│   └── core-exec-gov-graph.json   # Complete multi-agent
├── registry/
│   ├── capabilities.json          # Canonical capability registry
│   ├── tool-types.json            # Standard tool types
│   ├── profiles.json              # Profile definitions
│   └── extensions.json            # Extension registry
├── validators/                    # Official validators
│   ├── python/                    # Python validator (v1.0.0) ✅
│   │   ├── jsonagents/            # Package source
│   │   ├── tests/                 # 47 tests (100% passing)
│   │   └── README.md              # Documentation
│   └── README.md                  # Validator overview
└── docs/
    ├── index.md                   # Documentation index
    ├── implementers-guide.md      # Implementation guide
    ├── mapping-frameworks.md      # Framework conversions
    └── extensions.md              # Extension development
```

---

### 🧪 Validators

**Official validators ensure manifests comply with the specification:**

| Language | Status | Version | Test Coverage | Location |
|----------|--------|---------|---------------|----------|
| **Python** | ✅ Production Ready | v1.0.0 | 47/47 (100%) | [`validators/python/`](validators/python/) |
| JavaScript/TypeScript | 🔜 Coming Soon | - | - | - |
| Rust | 🔜 Coming Soon | - | - | - |
| Go | 🔜 Coming Soon | - | - | - |

**Quick validation:**
```bash
cd validators/python/
pip3 install -r requirements.txt
python3 -m jsonagents.cli validate ../../examples/*.json
```

See [`validators/README.md`](validators/README.md) for details.

---

### 🔗 Specification Family

JSON Agents uses a **modular profile system** for progressive enhancement:

| Profile | Required | Description | Use Case |
|---------|----------|-------------|----------|
| **Core** | ✅ Yes | Agent identity, tools, capabilities, and context | All manifests |
| **Exec** | ❌ No | Runtime metadata, language, entrypoint, resources | Deployable agents |
| **Gov** | ❌ No | Security, policies, observability, audit trails | Enterprise/regulated |
| **Graph** | ❌ No | Multi-agent topology and message routing | Orchestration |

Each profile is independently implementable, allowing minimal or full-featured agents.

---

### 🧠 Quick Start Example

A minimal agent with all four profiles:

```json
{
  "manifest_version": "1.0",
  "profiles": ["core", "exec", "gov", "graph"],
  "agent": {
    "id": "ajson://example.com/agents/router-hub",
    "name": "Router Hub",
    "version": "1.0.0"
  },
  "capabilities": [
    { "id": "routing", "description": "Route messages by intent" }
  ],
  "runtime": { 
    "type": "node", 
    "entrypoint": "dist/router.js" 
  },
  "security": { 
    "sandbox": "process" 
  },
  "policies": [
    {
      "id": "deny-external",
      "effect": "deny",
      "action": "tool.call",
      "where": "tool.endpoint !~ 'internal.corp'"
    }
  ],
  "graph": {
    "nodes": [
      { "id": "router", "ref": "ajson://example.com/agents/router-hub" },
      { "id": "faq", "ref": "ajson://example.com/agents/faq" }
    ],
    "edges": [
      { 
        "from": "router", 
        "to": "faq", 
        "condition": "message.intent == 'faq'" 
      }
    ]
  }
}
```

**See [`examples/`](./examples/) for complete working examples.**

---

### 🚀 Use Cases

- **🔄 Framework Interoperability**: Convert between LangChain, OpenAI, AutoGen, and custom frameworks
- **📦 Agent Registries**: Build discoverable catalogs of reusable agents
- **🏗️ Multi-Agent Systems**: Orchestrate complex workflows with conditional routing
- **🔐 Enterprise Governance**: Enforce security policies and audit trails
- **📊 Agent Marketplaces**: Standardized format for distributing and monetizing agents
- **🧪 Testing & Validation**: Schema-based validation for CI/CD pipelines

---

### 🛠️ Framework Support

JSON Agents provides bidirectional conversion with major frameworks:

| Framework | Import | Export | Documentation |
|-----------|--------|--------|---------------|
| **LangChain** | ✅ | ✅ | [Mapping Guide](./docs/mapping-frameworks.md#langchain) |
| **OpenAI** | ✅ | ✅ | [Mapping Guide](./docs/mapping-frameworks.md#openai) |
| **AutoGen** | ✅ | ✅ | [Mapping Guide](./docs/mapping-frameworks.md#autogen) |
| **MCP** | ✅ | ⚠️ | [Mapping Guide](./docs/mapping-frameworks.md#mcp) |
| **Hugging Face** | ⚠️ | ⚠️ | [Mapping Guide](./docs/mapping-frameworks.md#hugging-face) |
| **CrewAI** | ⚠️ | ⚠️ | [Mapping Guide](./docs/mapping-frameworks.md#crewai) |

✅ = Fully documented | ⚠️ = Partial support

---

### 📚 Documentation

| Document | Purpose |
|----------|---------|
| [**Specification**](./json-agents.md) | Complete normative specification |
| [**Implementer's Guide**](./docs/implementers-guide.md) | How to parse, validate, and use manifests |
| [**Framework Mappings**](./docs/mapping-frameworks.md) | Convert to/from other agent formats |
| [**Extensions Guide**](./docs/extensions.md) | Create custom extensions with `x-*` |
| [**Examples**](./examples/) | Working manifest examples |
| [**Changelog**](./CHANGELOG.md) | Version history and roadmap |

---

### 🔧 Tools & Validation

**Coming Soon:**
- `ajv` schema validator
- Reference implementations (Node.js, Python)
- Framework converters
- Web-based manifest editor

**Manual Validation:**
```bash
# Install ajv-cli
npm install -g ajv-cli

# Validate manifest
ajv validate -s schema/json-agents.json -d examples/core.json
```

---

### 🌟 What's New in v1.0

**Recent Additions** (Unreleased):
- ✨ **URI Scheme Definition**: Formal `ajson://` specification with resolution mechanism
- 📜 **Policy Expression Language**: Complete grammar for `where` clauses
- 🎯 **Complete Capability Suite**: All 7 capabilities now have formal schemas
  - ✅ qa.json (Question Answering)
  - ✅ classification.json (Classification)
  - ✅ extraction.json (Entity Extraction)
  - ✅ generation.json (Content Generation)

See [CHANGELOG.md](./CHANGELOG.md) for details.

---

### 🤝 Community & Support

- **💬 Discussions**: [GitHub Discussions](https://github.com/Agents-Json/Standard/discussions) (coming soon)
- **🐛 Issues**: [GitHub Issues](https://github.com/Agents-Json/Standard/issues)
- **📧 Contact**: spec@agentsjson.org
- **📖 Contributing**: See [CONTRIBUTING.md](./CONTRIBUTING.md)

---

### 🎯 Roadmap

**v1.0 (Current)**:
- ✅ Core, Exec, Gov, Graph profiles
- ✅ 7 capability schemas
- ✅ URI scheme specification
- ✅ Policy expression language
- ✅ Framework mapping guide

**v1.1 (Planned)**:
- 🔨 Reference validator implementations
- 🔨 Framework converter tools
- 🔨 Additional capability schemas
- 🔨 Community extensions
- 🔨 Public registry service

**Future**:
- Real-time profile for streaming agents
- Evaluation profile for testing/benchmarking
- Enhanced policy expression functions
- Formal IETF/W3C standardization path

---

### ⚖️ License

JSON Agents is released under the **Apache 2.0 License**.
See [`LICENSE`](./LICENSE) for details.

---

### 🧭 Contributing

We welcome contributions! Whether you're:
- 🐛 Reporting bugs or issues
- 💡 Proposing new features
- 📝 Improving documentation
- 🔧 Building tools and validators
- 🌍 Creating framework integrations

See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for guidelines.

**Code of Conduct**: This project follows the [Contributor Covenant 2.0](./CODE_OF_CONDUCT.md).

---

### 📊 Project Status

| Aspect | Status |
|--------|--------|
| **Specification** | 🟢 v1.0.0 Complete |
| **Schema Coverage** | 🟢 7/7 Capabilities (100%) |
| **Documentation** | 🟢 Comprehensive |
| **Tooling** | 🟡 In Development |
| **Community** | 🟡 Growing |
| **Standards Track** | 🟡 Draft |

---

### 🏆 Design Goals

JSON Agents is designed to be:

1. **Simple**: Easy to read and write by humans
2. **Complete**: Covers all aspects of agent definition
3. **Flexible**: Modular profiles for different use cases
4. **Safe**: Built-in security and governance
5. **Interoperable**: Works with existing frameworks
6. **Extensible**: Room for innovation without breaking changes
7. **Standard**: Based on established JSON specifications

---

### 🧱 Standards Compliance

JSON Agents is built on solid foundations:

- ✅ [RFC 8259](https://datatracker.ietf.org/doc/html/rfc8259) — JSON Data Interchange Format
- ✅ [RFC 3986](https://datatracker.ietf.org/doc/html/rfc3986) — URI Generic Syntax
- ✅ [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119) — Requirement Levels
- ✅ [ECMA-404](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/) — JSON Data Interchange Syntax
- ✅ [ISO/IEC 21778:2017](https://www.iso.org/standard/71616.html) — JSON Standard
- ✅ [JSON Schema 2020-12](https://json-schema.org/draft/2020-12/json-schema-core.html) — Validation

---

### 🔗 Related Projects

- [Model Context Protocol (MCP)](https://github.com/modelcontextprotocol) — Tool/context protocol
- [OpenAI Agents](https://platform.openai.com/docs/agents) — Agent API format
- [LangChain](https://github.com/langchain-ai/langchain) — Agent framework
- [AutoGen](https://github.com/microsoft/autogen) — Multi-agent framework

---

### 📈 Quick Stats

- 📄 **888 lines** of specification
- 🎯 **7 capability schemas** (100% complete)
- 📋 **4 profiles** (core, exec, gov, graph)
- 🔧 **6 tool types** (http, function, plugin, system, mcp, custom)
- 🌐 **4 examples** covering all profile combinations
- 📚 **8 documentation files**

---

### 🙏 Acknowledgments

JSON Agents draws inspiration from:
- JSON Schema and JSON-LD communities
- OpenAPI and AsyncAPI specifications
- Agent framework developers (LangChain, AutoGen, CrewAI)
- Model Context Protocol contributors
- The broader open-source AI community

---

### 🧱 Versioning

Version identifiers follow [Semantic Versioning 2.0](https://semver.org/).
The default branch represents the **latest stable version** of the specification.

---

© 2025 JSON Agents. All rights reserved.
