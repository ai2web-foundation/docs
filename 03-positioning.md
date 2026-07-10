# AI2Web - Canonical Positioning, Naming & Governance

> The single source of truth for what things are called and how they relate.
> Everything else (spec, code, site copy) must conform to this doc.
> Status: v2 · 2026-07-09

## What AI2Web is (one framing)

> **AI2Web is a protocol + specification + framework + reference implementation** for making websites understandable and actionable to AI agents.
>
> - **Protocol (`ai2w`)** - defines *discovery*, the *capability contract*, and the *capability-negotiation handshake* between agents and websites. It is the foundation for AI↔web communication.
> - **Specification** - the human-readable + machine-readable (JSON Schema) definition of the capability model.
> - **Framework** - `@ai2web/*` packages + CMS plugins that let a developer describe a site once and auto-generate every adapter.
> - **Reference implementation** - WordPress/WooCommerce plugin, validator, connector, demo sites.

> **Interoperability-layer framing (use this externally):** AI2Web is the **interoperability layer for AI-enabled websites**. It provides a common discovery and capability model across multiple agent protocols - including MCP, ACP, and future standards - while adding the governance, analytics, validation, and developer tooling that protocol specifications intentionally do not address. Like a browser supporting HTTP/1.1/2/3, WebSockets and WebRTC behind one experience, AI2Web abstracts the protocols - it is **not** "the MCP for websites" and **not** an alternative to any protocol.

### The one primitive everything hangs off

> **The canonical whole-site capability model *is the product*.** Everything defensible is downstream of it:
>
> ```
> Capability Model  →  Framework  →  Discovery Network  →  Analytics
>   (the thing)         (generates      (indexes             (measures
>                        FROM it)        BY it)               AGAINST it)
> ```
>
> No model → nothing to generate from, nothing to index by, nothing to measure against. Build the model first; the rest are things it powers.

### Deliberate scope boundary (the future-proofing rule)

> **AI2Web intentionally does not define transport protocols.** It exposes the capability model across an **open, additive set** - MCP, ACP, REST, GraphQL, OpenAPI, WordPress/CMS abilities, webhooks, and future protocols - and negotiates which to use. This single boundary protects AI2Web from every future protocol: when a new one wins, AI2Web adds an adapter - it never gets obsoleted.
>
> **The same applies to discovery.** `/.well-known/ai2w` is a *bootstrap* anchor; the capability model, not the location, is the product. If a neutral discovery standard emerges (e.g. `/.well-known/ai`, or a reference from `llms.txt`/`robots.txt`), AI2Web is carried by it rather than competing - maximising adoption. (RFC-0000 §11, RFC-0001 §3.4.)

## Naming (locked)

| Thing | Value | Notes |
|---|---|---|
| **Brand / product** | **AI2Web** | Public name, marketing, docs, org. |
| **Wordmark** | **AI²W** | Existing logo asset (white on black). |
| **Protocol / wire id** | **`ai2w`** | "AI-to-Web." In manifest (`"protocol": "ai2w"`), paths, negotiation. Room for `ai2ai`, `ai2d`. |
| **Protocol home (canonical)** | **`/ai2w`** | `GET /ai2w` returns the manifest; `/ai2w/*` are the live routes. Primary, developer-facing. |
| **Discovery anchor (required)** | **`/.well-known/ai2w`** | RFC 8615 machine-discovery. Points/redirects to `/ai2w`. Guarantees zero-knowledge agents can always find the manifest. Required *because* it's a protocol. |
| **Live routes** | **`/ai2w/*`** | `/ai2w/content`, `/ai2w/products`, `/ai2w/actions`, `/ai2w/events`, `/ai2w/negotiate`, `/ai2w/mcp`, … |
| **Friendly alias** | `GET /ai` (or `/.ai`) | Optional convenience → `/ai2w`. Non-normative. |
| **Domain** | ai2web.dev | Docs + validator + Discovery Network. |
| **GitHub org** | ai2web-foundation | Open repos. |
| **npm scope** | `@ai2web` | `@ai2web/core`, `/server`, `/validator`, `/mcp-bridge`, … |
| **Commercial entity** | **Polarize Ltd** | Operates Cloud / Gateway / analytics / Discovery-Network-Pro. |

## Capability negotiation (the protocol's beating heart)

Modelled on HTTP content negotiation. This is what makes `ai2w` a *protocol*, not just a manifest format.

```
Agent → Site:   I support  { transports: [mcp, rest], capabilities: [content, products, checkout, events, oauth] }
Site  → Agent:  I support  { transports: [rest, mcp],  capabilities: [content, products, events],  auth: oauth2+pkce }
Result:         Negotiated { transport: mcp, capabilities: [content, products, events] }
```

- The **site advertises** its full capability set (from the model); the **agent uses the subset it understands**. Exactly how durable internet standards evolve - servers advertise, clients consume the subset they support.
- New agent capabilities require **no website change** - the site already exposes everything; the agent simply starts consuming more.
- Exposed at `/ai2w/negotiate` (and embedded in the manifest for static negotiation).

## The six questions the manifest must answer

Every agent that reads `/ai2w` should immediately learn:
1. **Who are you?** - identity, type, jurisdiction.
2. **What do you know?** - content, products, services, docs, policies.
3. **What can you do?** - actions (input schemas, auth + approval requirements).
4. **How do I authenticate?** - none / OAuth2+PKCE / signed / session.
5. **How do I communicate?** - transports available + negotiation.
6. **What events can I subscribe to?** - order.shipped, price.drop, new_article.published, …

## Governance (open-core)

- **AI2Web Foundation** stewards the open standard: protocol, spec, JSON Schema, RFCs, reference SDKs, CMS plugins, validator, connector. Neutral, trust-building, community-contributable.
- **Polarize Ltd** builds the commercial layer: hosted Cloud, capability-level analytics, Enterprise Gateway, Discovery-Network-Pro visibility, certification, consulting.
- **Why the split:** a would-be standard needs credible neutrality; a business needs a monetisable moat. The split gives both.

## Licensing (locked)

- **Protocol, spec, schema, RFCs, docs:** **CC-BY 4.0**.
- **Code (SDKs, plugins, validator, adapters, connector):** **MIT**.
- **Commercial services (Cloud, Gateway, analytics):** proprietary, Polarize-owned.

## Mission & positioning statements

**Mission:**
> Give every website a single, vendor-neutral interface that any AI agent can discover, understand, and act on - so businesses describe what they can do once, and stay compatible as the AI ecosystem keeps changing.

**Killer line (external, primary):**
> **Describe your website once. AI2Web makes it understandable to every AI.**

**One-liner (technical/investor):**
> AI2Web is the capability, discovery, and measurement layer for the agentic web. It defines the model above the transports - MCP is plumbing; AI2Web is the model, the map, and the meter on top of it.

**Complements, never competes:**
> This is the version where AI2Web stopped competing with MCP and started complementing it. Any transport standard's success is AI2Web's tailwind, because AI2Web generates it.

**What NOT to say:**
- ❌ "A new *transport* protocol / a replacement for MCP." (It's a protocol that defines a *model + handshake*, not a wire, and it *generates* MCP.)
- ❌ "AI commerce plugin." (Commerce is one module; that space is commoditised.)
- ❌ "The new standard for the AI web" *as the opening pitch.* (Earn it; lead with the working demo.)

## Product surface (canonical list)

**Open (Foundation):**
- **The protocol + capability model** (spec · JSON Schema · RFCs) - *the product everything hangs off.*
- **Validator** - *primary product, not an afterthought.* `npx ai2web validate <url>` → per-capability pass/warn + **AI Readiness Score /100**. Web version on ai2web.dev. This is the first thing a developer touches.
- `@ai2web/core` · `@ai2web/server` · `@ai2web/mcp-bridge` · SDKs (TS → PHP → Python → …).
- WordPress/WooCommerce plugin · reference manifests · docs site.
- **Agent-side connector** (Claude / ChatGPT) - fronts the Discovery Network.

**Commercial (Polarize):** AI2Web Cloud (hosted endpoint, logs, rate-limit, monitoring, event delivery) · **capability-level analytics** · Enterprise Gateway · **Discovery-Network-Pro** · Certification · Consulting.

## Discovery Network (formerly "Directory")

> Renamed from "Directory" → **Discovery Network.** It's not a passive list; it's the network layer that lets agents discover AI2Web sites by capability. Public metadata only (name, URL, category, capabilities, endpoints, verification, health, version - never customer data).

## Modules (canonical set)

`content · commerce · actions · events · communication · identity/auth · search · agent · extensions`

Commerce is **one** module. The architecture centre is the **capability model**, not commerce.
