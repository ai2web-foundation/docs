# AI2Web: An Open Capability, Discovery and Measurement Layer for the Agentic Web

**Whitepaper · Version 1.0 · 2026-07-09**
**AI2Web Foundation** · Protocol identifier: `ai2w` · [ai2web.dev](https://ai2web.dev)

> Describe your website once. AI2Web makes it understandable to every AI.

---

## Abstract

The web was built for humans. As autonomous AI agents begin to search, compare, transact and support on behalf of users, they are forced to operate an interface designed for eyes and clicks - scraping HTML, guessing forms, driving browsers. This is unreliable, insecure and does not scale. A parallel ecosystem of standards has emerged to help - the Model Context Protocol (MCP) for tool invocation, the Agentic Commerce Protocol (ACP) for checkout, product feeds, `llms.txt`, `schema.org` - but each solves a slice, each is implemented differently across platforms, and none provides a universal, vendor-neutral way for a *website* to describe **what it is and everything it can do**, to be **discovered** across the web, and to be **measured**.

AI2Web is that layer. It is a small protocol (`ai2w`) plus a reference framework. A site declares its capabilities once in a machine-readable manifest; agents discover it, negotiate a shared capability set, and act through whatever transport already exists. AI2Web deliberately **does not define a transport** - it defines the *capability model* above transports and generates adapters (MCP, ACP, REST, GraphQL, feeds, and more) from a single source of truth. This paper describes the problem, the design, the protocol, the security and privacy model, the discovery network, the defensible analytics layer, and the governance model that keeps the standard open while a commercial platform sustains it.

> **In one sentence:** AI2Web is the **interoperability layer for AI-enabled websites**. It provides a common discovery and capability model across multiple agent protocols - including MCP, ACP, and future standards - while adding the governance, analytics, validation, and developer tooling that protocol specifications intentionally do not address. It is not "the MCP for websites" and not an alternative to any protocol; it is the layer that makes whichever protocols win easy to adopt.

**Contents:** 1. The Problem · 2. Design Goals & Non-Goals · 3. Architecture · 4. The Protocol · 5. Capability Negotiation · 6. Security & Consent · 7. Transport Strategy · 8. The Discovery Network · 9. Capability-Level Analytics · 10. Reference Implementation · 11. Why Now · 12. Governance & Licensing · 13. Comparison · 14. Conclusion · Appendix.

---

## 1. The Problem

Every website exposes HTML, CSS and JavaScript intended for a browser. An AI agent asked to "buy these shoes," "check my order," "book a table," or "open a support ticket" must reverse-engineer that human interface. Today it has a poor menu of options:

- **Scraping and browser automation** - fragile, breaks on redesign, defeated by dynamic UIs, and slow.
- **Bespoke APIs** - inconsistent per site, undiscoverable, rarely designed for agents.
- **Vendor-specific integrations** - a merchant integrates with one assistant's commerce programme, then another's, then the next.

Meanwhile the standards landscape has fragmented. A developer who wants to be "AI-ready" is told to consider MCP, WebMCP, ACP, OpenAI product feeds, Google's agent-payment work, OAuth for agents, `llms.txt`, and `schema.org` - each different across WordPress, Shopify, a custom stack. Next year there will be another. The result is duplicated engineering, uneven coverage, and no universal way to answer the first question any agent has: *"What is this website, and what can it do?"*

There is also a cost the industry rarely counts. Having every AI scrape, download, and render a full web page to extract a few facts is enormously wasteful: megabytes of HTML, CSS, and JavaScript parsed and executed for what could be a few kilobytes of structured data. As AI usage grows, so does that waste, in compute, bandwidth, and energy. A shared capability layer is the green-internet argument applied to AI: every request that avoids rendering a page is compute and carbon saved.

Three capabilities are structurally absent from the current ecosystem:

1. **Description** - a vendor-neutral model of a whole business (identity, content, commerce, actions, events, auth), independent of transport.
2. **Discovery** - a way to find *which* sites are agent-ready and *what* they can do, across the web. MCP is point-to-point; it presumes you already know the server.
3. **Measurement** - any observability of the agent economy hitting a site: which agents, seeking which capabilities, converting or churning.

## 2. Design Goals and Non-Goals

**Goals.** Be small enough for a one-person business to adopt and structured enough for an enterprise to trust. Be vendor-neutral. Complement existing standards, never compete with them. Make safe things easy and dangerous things explicit. Remain durable as the ecosystem changes.

**Non-goals (the permanent boundary).** AI2Web **does not define a transport protocol**. Transports (MCP, REST, GraphQL, and their successors) evolve independently; AI2Web defines the capability model that sits *above* them and negotiates which to use. This single boundary is what future-proofs the standard: when a new transport wins, AI2Web adds an adapter - it is never made obsolete. AI2Web also does not, in v0.1, define payment execution, identity verification, or marketplace governance; these layer on top of a validated discovery model.

## 3. Architecture

AI2Web is organised around one primitive from which everything else is derived:

```
Capability Model  →  Framework  →  Discovery Network  →  Analytics
  (the product)      (generates      (indexes             (measures
                      FROM it)        BY it)               AGAINST it)
```

The **capability model** is the product. The framework has nothing to generate adapters *from* without it; the Discovery Network has nothing to index *by* without it; the analytics has nothing to measure *against* without it. This is the reason the model, not any individual adapter, is the defensible core.

Positioned against prior layers of web infrastructure:

```
robots.txt   → a site exists          schema.org → its entities
sitemap.xml  → its pages              OpenAPI    → its API
OAuth        → identity               MCP        → callable tools
AI2Web       → the whole business: model + discovery + negotiation + events
```

Every AI2Web site answers six questions the moment an agent reads its manifest: **Who are you?** (identity) · **What do you know?** (content, products, docs) · **What can you do?** (actions) · **How do I authenticate?** (auth) · **How do I communicate?** (transports + negotiation) · **What events can I subscribe to?** (events).

## 4. The Protocol (`ai2w`)

**Discovery.** A site serves its manifest at the canonical home `GET /ai2w`, and MUST also serve `GET /.well-known/ai2w` as a discovery anchor (RFC 8615) that returns the manifest or a pointer to it - guaranteeing an agent with zero prior knowledge can always find it. Live module and action routes live under `/ai2w/*`.

**The manifest** is a JSON serialisation of the capability model. Required fields: `protocol` (`"ai2w"`), `version`, `site` (name/url/type), and `capabilities`. Recommended: `identity`, `transports`, `auth`, `consent`, `events`, `agent_service`, `contact`. Capabilities are declared as a map of module name → boolean or structured object across the canonical modules: **content, commerce, actions, events, communication, identity, search, agent, extensions**. Commerce is one module among equals - the architecture is not commerce-centric; AI2Web works equally for a law firm, a council, a SaaS product or a publisher.

**Actions** are declared structurally - `name`, `description`, `method`, `endpoint`, `requires_auth`, `requires_user_approval`, `risk`, and an `input_schema`. Declaring an action never executes it: discovery is not execution.

**Events** let a site publish structured, subscribable notifications - `order.shipped`, `price.drop`, `new_article.published`, `booking.confirmed`. This reframes the newsletter: instead of "enter your email," a user tells their assistant to *follow* a site, and the assistant filters what it surfaces. The website publishes events; the agent decides what matters.

**Agent service** optionally exposes a site's own AI agent, enabling agent-to-agent (A2A) interaction - a user's assistant talking to a business's assistant rather than scraping.

**Support & post-purchase** is the highest-value agentic use case and its own capability: order tracking, returns, refunds, cancellations and issue reporting. A user simply asks their assistant - *"where's my order?"* or *"these arrived damaged, I want a refund"* - and AI2Web fulfils it through whichever path the site exposes (direct actions, a checkout transport's post-purchase operations, or the brand agent). Reads (tracking) execute instantly; anything that moves money (refunds, cancellations) is high-risk and always returns a preview for the user to approve before it runs. This is defined in RFC-0007.

The full normative definition, JSON Schema, and machine-readable conformance suite are maintained in the AI2Web Specification (`ai2web-spec`).

## 5. Capability Negotiation

What makes `ai2w` a *protocol* rather than a static manifest format is **negotiation**, modelled on HTTP content negotiation. The site advertises its full capability set; the agent uses the subset it understands. An agent posts what it supports (`transports`, `capabilities`, `auth`) to `/ai2w/negotiate`; the site replies with the intersection and the chosen transport and endpoints. Crucially, **new agent capabilities require no website change** - the site already exposes everything, and a more capable agent simply begins consuming more of it. This mirrors how durable internet standards evolve: servers advertise, clients consume the subset they support.

## 6. Security and Consent

The security model rests on one principle: **discovery is safe; execution is controlled.**

Actions are classified by risk. **Low** (read content, search, public pricing) requires neither auth nor approval. **Medium** (create a ticket, check an order, reserve stock) should require authentication. **High** (purchase, payment, change or export personal data, confirm a booking, cancel an account, accept terms) MUST require authentication *and* explicit user approval. The approval flow is: the agent prepares an action, the site returns a **preview**, the user approves, the site executes and returns a confirmation with an audit reference.

Authentication uses **OAuth 2.1 with PKCE** for delegated agent actions - short-lived, scoped, revocable tokens - never static consumer API keys. Implementations log agent identity, action, approval status and result. The default posture is read-only; write capability is explicit. In short: make safe things easy and dangerous things explicit.

## 7. Transport Strategy

AI2Web is a **multi-protocol interoperability layer**. A site declares its capabilities once; AI2Web exposes them through whichever protocols the site supports and the agent understands. The set is open and additive:

```
        AI Assistant
              │
              ▼
           AI2Web            ← one discovery + capability model
              │
              ├── MCP
              ├── ACP
              ├── REST APIs
              ├── GraphQL
              ├── OpenAPI
              ├── WordPress / CMS abilities
              ├── Webhooks
              └── Future protocols
```

This is how a web browser works: it supports HTTP/1.1, HTTP/2, HTTP/3, WebSockets and WebRTC behind one consistent experience, without being tied to any of them. AI2Web plays the same role for AI-enabled websites - it abstracts the protocols behind a common capability model.

Guidance on which transport suits which module (not an exhaustive list - see the adapter profiles):

| Module | Typical transport(s) | Rationale |
|---|---|---|
| content, search | REST + feeds (JSON/RSS), MCP, GraphQL | read-oriented, cacheable |
| actions, agent | **MCP** (+ REST/GraphQL) | headless, reliable invocation |
| commerce | a **checkout transport** (e.g. ACP) + product feed | AI2Web does not reimplement checkout; it advertises the checkout transport |
| events | webhooks / SSE / poll | push and subscribe |

MCP and ACP are backend, API-driven, and therefore reliable, headless, transactional and auditable - the right default for a business-capability layer. **WebMCP**, which exposes browser-side tools from a running page, is frontend-coupled and is an **optional adapter, disabled by default**, generated later for frontend-only sites and agentic-browser contexts. Backend-first core; WebMCP deferred, never rejected. Because AI2Web does not bet on a transport winner, the success of *any* transport standard is a tailwind - it simply becomes another adapter AI2Web generates. The same holds for **discovery itself**: `/.well-known/ai2w` is a bootstrap anchor, and if a neutral discovery standard (e.g. `/.well-known/ai`) emerges, AI2Web is carried by it rather than competing with it.

## 8. The Discovery Network

MCP is point-to-point - you must already know a server to use it. There is no registry answering *"find AI-ready sites in category X that can do Y."* The AI2Web **Discovery Network** fills that gap: a verified index of AI-capable sites storing **only public metadata** - name, URL, category, capabilities, endpoints, verification, health, version. It never stores customer data, orders, or message bodies. It exists only because sites are described in a shared capability model.

The network also resolves the classic cold-start problem through a **both-sided** design. AI2Web ships not only a website-side plugin/SDK but an **agent-side connector** - an MCP server (loadable as a Claude connector or a ChatGPT App) that fronts the Discovery Network. This requires *no vendor to adopt `ai2w`* - only that assistants permit third-party connectors, which they already do. Owning both sides lets the Foundation and its ecosystem seed supply and demand together, going deep in one vertical for density rather than chasing breadth.

## 9. Capability-Level Analytics

A protocol ships no measurement, and this is where the durable, monetisable value sits - *above* the transport, surviving any transport war. The distinction between commodity and defensible analytics is critical:

- **Commodity** - generic AI-bot traffic ("GPTBot hit you 400×"). Infrastructure providers already give this away; they see packets. Building here loses.
- **Defensible** - *semantic, capability-level* insight that exists only because AI2Web owns the model, the network and the connector: *"agents requested `checkout` 200× this month; you don't expose it → estimated lost conversion"*; demand-versus-supply of capabilities across the network; category benchmarking (*"83% of AI-ready stores in your vertical expose `booking`; you don't"*); AI-readiness scoring. Infrastructure players cannot produce this - they see traffic, not capabilities.

Analytics is a compounding moat rather than a day-one wedge: it needs traffic and network density first. The go-to-market therefore leads with "works in your assistant today," and analytics becomes the sticky, monetisable layer at scale.

## 10. Reference Implementation and Ecosystem

The Foundation maintains open reference implementations so the standard is immediately usable: a TypeScript framework (`@ai2web/core`, `@ai2web/validator`, `@ai2web/server` with a Cloudflare Workers adapter, `@ai2web/mcp-bridge`, `@ai2web/connector`); a PHP SDK; a WordPress/WooCommerce plugin that auto-derives a manifest from a live site; the Discovery Network service; and a public **validator**. The validator (`npx ai2web validate <url>`, and on the web) is a primary product in its own right - the first thing a developer touches - scoring a manifest per capability and returning an **AI Readiness Score /100** and a compliance tier (Basic / Standard / Enterprise). A portable **conformance suite** lets any implementation, in any language, prove it is AI2Web-compatible.

## 11. Why Now

Agents have become capable enough to perform real tasks, and the shared substrate for them to reach services - MCP - is now supported across major assistants, with ACP live for commerce. The pieces exist; what is missing is the layer that unifies them into one description a site writes once, a way to discover those sites, and a way to measure them. Adjacent solutions are commerce-only, vendor-specific, or platform-bound (a single CMS). AI2Web's differentiation is breadth (the whole business, not just products), unification (one model → many adapters), the events and A2A surface that browsing cannot replicate, and the discovery-plus-measurement layers no transport provides.

## 12. Governance and Licensing

AI2Web separates the open standard from the commercial platform so the protocol can be trusted as neutral while a business sustains it. The **AI2Web Foundation** stewards the specification, JSON Schema, RFCs, reference SDKs, the WordPress plugin, the validator and the conformance suite. **Polarize Ltd** operates the commercial platform - hosted Cloud, capability-level analytics, the Enterprise Gateway, Discovery-Network-Pro, and certification - building on the standard with no special protocol privileges. The specification and documentation are licensed **CC-BY-4.0**; the reference code is **MIT**. All normative change flows through a public RFC process that updates the spec, the schema and the conformance suite together.

## 13. Comparison with Existing Standards

| Standard | Purpose | What it does not do | AI2Web relationship |
|---|---|---|---|
| **MCP** | Tool-invocation transport | Cross-site discovery; describe a business; measurement | Generated as one transport module |
| **ACP** | Products + checkout | Non-commerce; vendor-neutral breadth | Emitted as the commerce transport |
| **WebMCP** | Browser-side tools | Backend/headless; whole-site model | Optional, deferred frontend adapter |
| **OpenAPI / REST / GraphQL** | Describe/serve an API | Which site; business capability model | Referenced/reused as transports |
| **OAuth 2.1 / PKCE** | Delegated auth | Everything else | Used as-is |
| **`llms.txt` / `schema.org`** | Hints / entity markup | Actions; auth; discovery index | Structured superset; vocab reused |

AI2Web sits *above* all of these. It complements; it does not replace.

## 14. Conclusion

Every website today exposes an interface for browsers. Tomorrow, every website should also expose an interface for autonomous agents - vendor-neutral, discoverable, measurable, and durable across a changing standards landscape. AI2Web provides that interface as a small protocol and a generating framework: MCP is plumbing; AI2Web is the model, the map and the meter on top of it. A business describes what it can do once, and stays compatible as the AI ecosystem keeps changing.

---

## Appendix A - Endpoints at a glance

```
GET  /.well-known/ai2w   discovery anchor (required) → points to /ai2w
GET  /ai2w               the manifest (canonical home)
POST /ai2w/negotiate     agree a capability set + transport
GET  /ai2w/{module}      content · products · actions · events · search · agent
GET  /ai2w/mcp           generated MCP transport
```

## Appendix B - Minimal manifest

```json
{
  "protocol": "ai2w",
  "version": "0.1",
  "site": { "name": "Example Store", "url": "https://example.com", "type": "ecommerce" },
  "capabilities": { "content": true, "commerce": { "enabled": true, "checkout": true } }
}
```

## Appendix C - Glossary

**Capability model** - the vendor-neutral schema describing what a site is and can do; the primitive everything derives from. **Manifest** - the JSON serialisation served at `/ai2w`. **Negotiation** - the handshake agreeing a shared capability set and transport. **Discovery Network** - the public-metadata index of AI-ready sites. **Connector** - the agent-side MCP server fronting the network. **Compliance tier** - Basic / Standard / Enterprise, as scored by the validator. **A2A** - agent-to-agent interaction via a site's own agent service.

---

*This whitepaper is a companion to the AI2Web Specification (`ai2web-spec`) and the positioning documents in `docs/`. It is licensed CC-BY-4.0 by the AI2Web Foundation.*
