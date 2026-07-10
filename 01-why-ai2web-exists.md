# Why AI2Web Exists

> The go/no-go gate. If this argument doesn't hold, we don't build.
> Status: v1 · 2026-07-09

This document exists to answer one question honestly, because every hour of engineering after it is wasted if the answer is weak:

> **If MCP and WebMCP were universally adopted by every website and every AI assistant tomorrow, what part of AI2Web would still be indispensable?**

If the honest answer is *"not much - it mostly wraps existing standards,"* AI2Web is a nice developer tool, not a company, and we should ship a small library and stop. If the honest answer names things that **cannot be MCP no matter how universal MCP becomes**, we have a foundation.

We believe there are three such things. This document argues them, and - just as importantly - argues the counter-cases so we don't fool ourselves.

---

## 1. The four sub-questions

### Q1. Why doesn't MCP / WebMCP / WP Abilities / OpenAI Commerce already solve this?

They solve **connection and invocation**, not **discovery, description, or measurement**.

- **MCP** is a point-to-point tool-invocation transport. An agent that already knows a server's URL can call its tools. MCP has no concept of *"find every site that can do X,"* no shared description of *what a business is*, and no observability. It is plumbing.
- **WebMCP** exposes browser-side tools from a running page. It is frontend-coupled (JavaScript, page state, browser support) and, like MCP, is about *calling tools*, not describing a whole business or discovering sites across the web.
- **WordPress Abilities API + MCP Adapter** turns WordPress abilities into MCP tools. Great - for WordPress, and only for tool exposure. It is one implementation of one CMS producing one transport.
- **OpenAI Commerce / ACP** handles *products and checkout* inside one vendor's assistant. It is commerce-only and vendor-shaped. (ACP is already live on WooCommerce via Instant Checkout - so "sell products in ChatGPT" is a solved, commoditised problem. We must not build that.)

The gap none of them fills: **there is no universal, vendor-neutral way for a website to say "here is everything I am and everything I can do," no cross-site index of who is AI-capable, and no measurement of the agent economy hitting a site.**

### Q2. Why would a developer choose AI2Web over implementing those directly?

Because the alternative is implementing - and re-implementing - a moving target. Today a developer who wants to be "AI-ready" is told to consider MCP, WebMCP, ACP, OpenAI product feeds, Google AP2/UCP, OAuth for agents, llms.txt, and schema.org. Each is different per framework and CMS. Next year there will be an eighth.

AI2Web is the abstraction: **describe your site's capabilities once; we generate the adapters.** This is the proven shape of Auth0 (over OAuth), Swagger (over OpenAPI), Stripe (over card networks), Laravel/React (over PHP/DOM). None invented the underlying standard; each made it painless and became the default entry point.

The developer value is real and available **day one, without any vendor adopting `ai2w`**:
- one install → a valid capability manifest + an MCP endpoint that works in Claude/ChatGPT connectors today;
- structured feeds (product, content, RSS/JSON) for free;
- a validator that tells them if they're AI-ready and how they compare.

### Q3. What can AI2Web generate automatically that would otherwise be significant engineering effort?

From one declared source of truth:
- the `/.well-known/ai2w` discovery manifest;
- `/ai2w/*` module/action routes (content, products, actions, events, search);
- an **MCP server** (via `@ai2web/mcp-bridge`) - usable in assistants immediately;
- **OpenAI-style product feeds** and RSS/JSON content feeds;
- an **event/subscription** surface (order.shipped, price.drop, new_article.published…);
- OAuth+PKCE scaffolding for scoped, auditable agent actions;
- later: ACP and WebMCP adapters, without the site changing.

Hand-rolling and *maintaining* that matrix across CMSs and frameworks is exactly the duplicated effort the ecosystem is drowning in.

### Q4. If WebMCP/MCP win tomorrow, what stays indispensable?

Three things that sit **above** any transport and therefore survive it. This is the crux - see §2.

---

## 2. The indispensable core - and its spine

**Everything defensible hangs off one primitive: the canonical whole-site capability model. It *is the product*.** The framework, the Discovery Network, and the analytics are things the model *powers* - none of them can exist without it.

```
Capability Model  →  Framework  →  Discovery Network  →  Analytics
  (the thing)         (generates      (indexes             (measures
                       FROM it)        BY it)               AGAINST it)
```

- The **framework** has nothing to generate adapters *from* without the model.
- The **Discovery Network** has nothing to index *by* without the model.
- The **analytics** has nothing to measure *against* without the model - which is exactly what separates our defensible, capability-level analytics from Cloudflare's commodity packet-counting.

**MCP is plumbing: it describes a *tool*, never a *business*.** The capability model is the missing description layer - "schema.org for the agentic web." Own it, and the map (Discovery Network) and the meter (analytics) become yours by construction. This is why we build the model first.

### Layer 0 - the primitive: the canonical whole-site capability model
A vendor-neutral schema for *what a website is and can do* - identity, content, commerce, actions, events, auth, agent service - independent of which transport carries it, plus a **capability-negotiation handshake** (agent advertises what it supports, site advertises what it supports, they agree). The handshake is what makes `ai2w` a *protocol* rather than a mere format. MCP describes a tool; it never describes a business or negotiates a capability set.
*Honest risk:* schema value is adoption-dependent - a schema nobody uses is worthless. Mitigation: the model is useful *locally* (it drives the generators) before it's a standard, and the connector + Discovery Network create first-party demand, so we never wait on the ecosystem to adopt it.

### Layer 1 - the framework (derived from the model)
Because the site is described in one model, the framework can *generate* every adapter from it - the `/ai2w` manifest, `/ai2w/*` routes, an MCP server, product/content feeds, OAuth scaffolding, later ACP/WebMCP. This is the day-one developer wedge and the thing that manufactures the traffic the later layers need. Copyable in isolation - which is exactly why the model (Layer 0) and the network/analytics (Layers 2–3), not the framework, are the moat.

### Layer 2 - the Discovery Network (derived from the model)
MCP is point-to-point: you must already know a server to use it. There is no registry answering *"find AI-ready sites in category X that can do Y."* A verified **Discovery Network** of AI-capable sites - **public metadata only** (name, URL, category, capabilities, endpoints, verification, health, version; never customer data) - is a genuine, MCP-shaped gap and a network-effects asset. It only exists because sites are described in the shared model.
*Honest risk:* discovery networks need **density in a vertical**, not raw count. Mitigation: go deep in one WooCommerce sub-vertical first; we own the agent-side connector, so we seed both sides.

### Layer 3 - capability-level analytics / observability (derived from the model)
A protocol ships no measurement. No MCP or WebMCP tells a site owner which agents hit them, what those agents *tried* to do, what converted, or what capabilities agents wanted that the site doesn't expose. That intelligence lives above the transport - the Google-Analytics-of-the-agentic-web position - and only exists because the model gives every request a *capability* to attribute it to.

**Critical distinction (this is where most people would get it wrong):**
- ❌ *Commoditised:* generic AI-bot traffic ("GPTBot hit you 400×"). Cloudflare and other infra players already give this away - they see packets. If our analytics is this, we lose.
- ✅ *Defensible:* **semantic, capability-level analytics that only exists because we own the model + Discovery Network + connector** - "agents requested `checkout` 200× this month; you don't expose it → estimated lost conversion"; demand-vs-supply of capabilities across the network; category benchmarking ("83% of AI-ready stores in your vertical expose `booking`; you don't"); AI-readiness scoring. Cloudflare cannot produce this - they see traffic, not capabilities.

*Honest risk:* analytics is a **compounding moat, not a day-one wedge** - it needs traffic + Discovery-Network density before it's worth paying for. Mitigation: lead with "works in Claude today"; analytics is what makes the platform sticky and monetisable at scale (the Polarize commercial engine), not the opening pitch.

---

## 3. What we are deliberately NOT

To keep the argument honest, the anti-scope:
- **Not a new transport protocol.** We never invent `POST /execute`. MCP/REST/GraphQL exist; we point to them. Inventing another transport is the 3/10 idea.
- **Not a commerce plugin.** ACP-on-WooCommerce is solved and commoditised. Commerce is one module, not the centre. Our edge is breadth (law firms, councils, SaaS, publishers - not just shops) + the non-commerce surface (support, bookings, forms, events).
- **Not frontend-coupled.** Backend-first. WebMCP is an optional later adapter, not the core.
- **Not vendor-locked.** If any standard wins, adoption *strengthens* us because we generate it.

---

## 4. The honest verdict

Two products live under the "AI2Web" name, with different odds:

1. **The framework + validator + agent connector** - worth building with confidence. The fragmentation pain is real, it needs no one's permission, and it delivers value on day one. This is the **wedge that manufactures traffic**.
2. **The capability model + Discovery Network + capability-analytics** - the **moat the traffic makes valuable**, and the answer to the gate question. The model is the primitive; the network and analytics derive from it. More speculative and network-dependent, but *within our control* because we own both sides of the market (site plugin + agent connector) and don't depend on a big vendor adopting `ai2w`.

**Go decision holds** on one condition, proven cheaply before heavy engineering: a **both-sided spike** - a WooCommerce demo store serving `/.well-known/ai2w` + MCP, and an AI2Web connector loaded into Claude/ChatGPT that discovers it and performs one authenticated action live. If that works, the answer to "why not just MCP" writes itself: *because we own the connective tissue on both ends and unify N sites behind one agent connection, then measure the whole thing.*

**Lead the story with:** "Make your existing site work with AI agents in five minutes - and here's it working in Claude today." Never lead with "the new standard for the AI web." Let the standard earn its status underneath.

---

## 5. Kill criteria (what would make us stop)

Intellectual honesty means naming what would falsify the thesis:
- The both-sided spike can't produce a compelling live demo in a real assistant.
- After building the connector, the only site capabilities agents actually want are read-only content that assistant browsing already handles well.
- A platform vendor (Anthropic/OpenAI/Cloudflare/WordPress/Shopify) ships the capability-model + Discovery Network + neutral analytics natively before we reach vertical density.
- Developers try the framework and conclude "this is just an MCP wrapper" - i.e., §2 didn't materialise in the product.

If two or more of these hit, revert to shipping the framework + validator as a focused open-source tool and drop the platform ambition.
