# AI2Web and the agentic-web ecosystem

Several projects appeared in the same period because everyone sees the same trend: the web was built for humans, and agents need an explicit contract. That convergence is evidence the problem is real, not that AI2Web is redundant. AI2Web's identity is not a unique manifest format; it is execution, breadth and developer experience: one capability model, shipped across many protocols and languages, with validation and tooling.

## Where each project sits

| Project | Primary focus | AI2Web relationship |
|---|---|---|
| **MCP** | Tool transport for agents | We generate it. One of our adapters. |
| **WebMCP** | Browser / DOM tools | Optional frontend adapter, deferred by design. |
| **OpenAPI** | API description | We generate it (descriptive adapter). |
| **WordPress Abilities** | CMS-native capabilities | We map it into the model (via the plugin). |
| **OpenAI Commerce / ACP** | Agentic checkout | We advertise and drive it (ACP adapter). |
| **AIWebIndex** | AI crawling / indexing | Complementary discovery input. |
| **A2WF** | Agent policy / governance | Complementary; our consent and risk model aligns. |
| **A2UI (Google)** | Agent-generated UI | Downstream of capabilities; out of our scope. |
| **IntentWeb** | Agent manifest + framework (Internet-Draft) | Convergent. Interop by reading its `agent.json`. |

Across all of them, AI2Web is the layer that connects them, not another one to replace them.

## IntentWeb, specifically

IntentWeb is the closest match: a manifest at a `/.well-known/` anchor, capabilities, policies, consent, audit, "connect existing standards, do not replace them," and a framework (AgentLayer) that generates adapters. The concept is genuinely convergent.

The difference is maturity. Their public roadmap lists the framework, WordPress plugin, MCP adapter, OpenAPI adapter and index as **Planned** or **Exploratory**, with the manifest **in review**. AI2Web has these built today: a spec plus JSON Schema and RFCs with a conformance suite, SDKs in five languages that reproduce one contract, MCP / GraphQL / ACP / OpenAPI adapters, a WordPress plugin, a validator with an AI Readiness Score, a discovery network, and React plus a framework-free web component.

They also lead harder on one idea worth adopting: **verifiable, signed results**. AI2Web already returns an `audit_ref` and lists signed receipts as an open question (RFC-0009); this is a good prompt to elevate it.

## Positioning takeaways

- **Same** as the field: the manifest, capability, consent and audit shape.
- **Different**: shipped implementation across five languages, a validator and readiness score, and a developer-experience-first framework (`npm install @ai2web/core`, a WordPress plugin, React). The concept is convergent; the working framework is the moat.
- **Build on, not against**: we generate MCP / ACP / OpenAPI, map WordPress Abilities, and can be carried by a neutral discovery anchor or bridge to `agent.json` if one wins. Discovery is a replaceable envelope; the capability model is the durable contract.

**Bottom line:** being right and shipped beats being first and a draft. Let others own "the standards proposal"; AI2Web owns "the one you can deploy today."
