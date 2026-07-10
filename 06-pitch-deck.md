# AI2Web - Pitch Deck

*17 slides · v1.0 · 2026-07-09. Each `---` is a slide. Companion to the [Business Plan](05-business-plan.md).*

---

## 1 · AI2Web

**Describe your website once. AI2Web makes it understandable to every AI.**

The open capability, discovery and measurement layer for the agentic web.

`ai2web.dev` · AI2Web Foundation (open standard) + Polarize (commercial platform)

---

## 2 · The problem

The web was built for humans. AI agents now buy, book, and support on our behalf - but have to **scrape HTML, guess forms, and drive browsers.**

It's unreliable, insecure, and doesn't scale.

And the "fixes" are fragmented: MCP, ACP, WebMCP, product feeds, `llms.txt`, `schema.org` - each a slice, each different per platform, a new one every year.

---

## 3 · Why now

- Agents are finally capable of real tasks.
- MCP is supported across major assistants; ACP is live for commerce.
- The pieces exist - **nobody has unified them.**

The window: own the layer that describes, discovers, and measures - before it standardises around someone else.

---

## 4 · The solution

A website declares its capabilities once at **`/ai2w`**. Every agent instantly learns six things:

**Who are you · What do you know · What can you do · How do I auth · How do I communicate · What events can I subscribe to.**

AI2Web generates the rest (MCP, ACP, feeds), handles discovery + negotiation - and, deliberately, **never defines a transport**, so it's never obsoleted.

---

## 5 · How it works

```
GET /.well-known/ai2w   → discovery anchor
GET /ai2w               → the capability manifest
POST /ai2w/negotiate    → agree a capability set + transport
GET /ai2w/{module}      → content · products · actions · events
GET /ai2w/mcp           → MCP transport (generated)
```

One install → a manifest + an MCP endpoint that works in Claude & ChatGPT **today** - no vendor adoption required.

---

## 6 · The product is the capability model

```
Capability Model → Framework → Discovery Network → Analytics
   (the thing)      (adoption)    (network moat)     (revenue moat)
```

Everything defensible derives from one primitive. The framework generates from it, the network indexes by it, the analytics measure against it.

**MCP is plumbing. AI2Web is the model, the map, and the meter on top of it.**

---

## 7 · Both sides - no cold start

We ship the **website plugin** *and* the **agent-side connector** (a Claude/ChatGPT MCP app).

We don't need OpenAI or Anthropic to adopt `ai2w` - only to allow third-party connectors, which they already do.

→ We seed supply and demand together. This is the wedge.

---

## 8 · The moat

1. **Capability model** - the schema everything derives from; sticky once adopted.
2. **Discovery Network** - network effects + a verified index no transport provides.
3. **Capability-level analytics** - *"agents requested `checkout` 200×, you don't expose it → £X lost conversion."*

That last one is the killer: **semantic** insight only possible because we own the model + network + connector. Cloudflare sees packets; we see capabilities.

---

## 9 · We complement, we don't compete

| | |
|---|---|
| MCP | agent → tools |
| ACP | checkout |
| OAuth | auth |
| **AI2Web** | **the model + discovery + measurement above them** |

Any transport standard's success is our **tailwind** - because we generate it.

---

## 10 · Market

**Wedge:** WordPress/WooCommerce (tens of millions of sites) → go deep in one vertical for network density.

**Expand:** SaaS, agencies, publishers, bookings, enterprise - because commerce is just one module; the model fits any business.

Reachable base in the millions; low-single-digit paid conversion at £30–£1,000/mo is a large ARR base, with enterprise gateway + analytics as high-ACV anchors.

---

## 11 · Business model - open-core

Free protocol + SDKs + validator drive adoption. Revenue from managed services + intelligence:

- **Pro** £29–99/mo · hosted endpoint + analytics
- **Business** £199–999/mo · multi-site, monitoring, teams
- **Enterprise** £10k–100k+/yr · gateway, SLA, compliance
- **Certification** + services

Compounding engine: **capability analytics**, sold up-market as density grows.

---

## 12 · Go-to-market

1. **Credibility** - spec, GitHub, validator, WordPress PoC.
2. **Developers** - SDKs, content, community, MCP-directory listings.
3. **Business** - agencies + ecommerce, Cloud beta, vertical depth.
4. **Enterprise** - gateway, partnerships, analytics upsell.

Message: *"works in your assistant today"* - never *"the new standard."*

---

## 13 · Traction - already built & verified

- `ai2w` v0.1 spec + JSON Schema + **passing 8-case conformance suite**.
- **Three SDKs** (TypeScript, PHP, Python) - each independently reproduces the conformance contract.
- WordPress/WooCommerce plugin · validator · server (+ Cloudflare adapter) · MCP bridge · agent connector · Discovery Network stub.
- Live site + web validator. **Security review done** (token-exfil, SSRF, directory-poisoning - fixed).

Structured as independent `ai2web-foundation` repos, ready to open-source.

---

## 14 · Roadmap (6 months)

- **M1–2:** launch spec + validator + WP plugin; both-sided connector demo; design partners.
- **M3–4:** Cloud alpha; Discovery Network (Workers/D1); Go/.NET SDKs; RFC-0002–0006.
- **M5–6:** Cloud beta; **capability-analytics alpha**; Enterprise Gateway; certification.

---

## 15 · Team & governance

**AI2Web Foundation** stewards the neutral open standard; **Polarize Ltd** operates the commercial platform - the split that makes a standard trustworthy *and* a business fundable.

*(Founder, advisors, key hires - to complete.)*

---

## 16 · The ask

Funding deployed against: (1) engineering to ship Cloud + analytics + gateway, (2) developer relations for adoption, (3) go-to-market in the anchor vertical.

Early KPIs are adoption (SDK installs, AI-ready sites, connector installs); revenue follows the standard-adoption curve by 2–4 quarters.

---

## 17 · Vision

Every website today exposes an interface for browsers.

**Tomorrow, every website also exposes an interface for AI agents** - vendor-neutral, discoverable, measurable.

AI2Web is that interface. Describe once; understood by every AI.
