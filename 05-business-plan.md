# AI2Web - Business Plan

**Version 1.0 · 2026-07-09 · Working document**
Companion to the [Whitepaper](04-whitepaper.md) and [Positioning](03-positioning.md).

---

## 1. Executive Summary

AI2Web is an open protocol and commercial platform that makes any website understandable and actionable to AI agents. Websites implement one thing - a capability manifest at `/ai2w` - and AI2Web generates the integrations every AI ecosystem requires (MCP, ACP, feeds), lets agents discover the site, and measures the agent traffic that results.

The company operates an **open-core** model. The **AI2Web Foundation** stewards the free, vendor-neutral standard and reference implementations to drive adoption; **Polarize Ltd** monetises hosted infrastructure, capability-level analytics, an enterprise gateway, and certification. The protocol drives trust and adoption; the platform generates revenue; the discovery network and analytics create defensibility.

The opportunity is to become the **default way websites describe themselves to AI agents** - the capability, discovery and measurement layer of the agentic web. As with Auth0 over OAuth, Stripe over card networks, and Swagger over OpenAPI, the winner is not whoever invents the underlying standard but whoever makes it effortless to adopt and owns the layer above it.

## 2. Problem

The web was built for humans. AI agents now search, compare, buy, book, and support on users' behalf - but must operate a human interface by scraping HTML, guessing forms, and driving browsers. That is unreliable, insecure, and does not scale.

The standards meant to help are fragmented: MCP (tools), ACP (checkout), OpenAI feeds, Google's agent-payment work, `llms.txt`, `schema.org` - each solving a slice, each implemented differently per CMS and framework, with an eighth arriving next year. Three things are structurally missing: a vendor-neutral way to **describe** a whole business, a way to **discover** which sites are agent-ready and what they do, and any **measurement** of the agent economy hitting a site.

## 3. Solution and the Indispensable Core

A site declares its capabilities once; AI2Web generates the adapters, exposes discovery and negotiation, and (commercially) measures usage. Crucially, AI2Web **does not define a transport** - it defines the capability model above transports and negotiates which to use, so it is never obsoleted when a new transport wins.

Everything defensible hangs off one primitive:

```
Capability Model  →  Framework  →  Discovery Network  →  Analytics
  (the product)      (adoption)      (network moat)       (revenue moat)
```

If MCP and WebMCP were universally adopted tomorrow, what stays indispensable is what sits *above* transport and can't be MCP: the **canonical whole-site capability model**, the **cross-site Discovery Network** built on it, and **capability-level analytics** the model makes possible. MCP is plumbing; AI2Web is the model, the map, and the meter on top of it.

## 4. Product Suite

**Open (AI2Web Foundation) - drives adoption:**
- The `ai2w` protocol, JSON Schema, RFCs, and a portable **conformance suite**.
- **Validator** - `npx ai2web validate` (+ web): an AI Readiness Score /100. The first thing a developer touches and a standalone reason to visit.
- SDKs: **TypeScript, PHP, Python** (shipped), then Go/.NET/Rust.
- WordPress/WooCommerce plugin; framework adapters (React/Next/Astro/Laravel/Django).
- **Agent-side connector** (Claude / ChatGPT) - the "both sides" piece that defeats cold-start.

**Commercial (Polarize) - generates revenue:**
- **AI2Web Cloud** - hosted endpoint, logs, rate limiting, monitoring, event delivery, version management.
- **Capability-level analytics** - the durable moat (see §8).
- **Enterprise Gateway** - expose CRM/ERP/inventory/support behind approved, scoped capabilities.
- **Discovery-Network-Pro** - enhanced visibility/verification.
- **Certification** and professional services.

## 5. Market and Customers

**Primary:** ecommerce businesses, SaaS companies, agencies, CMS site owners, customer-support and booking platforms. **Secondary:** enterprise digital teams, government, education, healthcare, financial services, travel. **Platform partners:** AI vendors, browser vendors, voice assistants.

The wedge is **WooCommerce/WordPress** (the fastest route to installed base) with depth in a single vertical before breadth. Commerce is one module, not the centre - the model serves law firms, councils, publishers and SaaS equally, which is what expands the market beyond shops.

Sizing (top-down, illustrative): tens of millions of WordPress/WooCommerce sites and millions of SaaS/ecommerce businesses form the reachable base; converting even low-single-digit percentages to a paid tier at £30–£1,000/mo is a large recurring-revenue opportunity, with enterprise gateway and analytics contracts as the high-ACV layer.

## 6. Business Model

Open-core. The protocol, SDKs, plugins, validator, and conformance suite are free (MIT / CC-BY-4.0). Revenue accrues to managed services and intelligence:

| Tier | Price | Includes |
|---|---|---|
| **Free** | £0 | Protocol, SDKs, basic validator, self-hosting |
| **Pro** | £29–99/mo | Hosted endpoint, logs, basic analytics, version history |
| **Business** | £199–999/mo | Multi-site, advanced auth, monitoring, team access, richer analytics |
| **Enterprise** | £10k–100k+/yr | Private gateway, custom integrations, SLA, compliance, dedicated support |
| **Certification** | annual fee | "AI2Web Certified" verification + directory listing |
| **Services** | project | Implementation, migration, security review, training |

The compounding engine is **capability-level analytics**, sold up-market once traffic and network density exist.

## 7. Go-to-Market

**Both-sided from day one.** AI2Web ships the website plugin *and* an agent-side connector, so it does not depend on any AI vendor adopting `ai2w` - only on assistants allowing third-party connectors, which they already do. Owning both sides lets us seed supply and demand together.

- **Phase 1 - Credibility:** publish the spec, GitHub, the website + validator, reference manifests, and the WordPress/WooCommerce proof of concept.
- **Phase 2 - Developer adoption:** SDKs, technical content, example integrations, community, listings in MCP/agent directories.
- **Phase 3 - Business adoption:** agencies and ecommerce, implementation packages, CMS plugins, hosted Cloud beta; go deep in one vertical for Discovery-Network density.
- **Phase 4 - Enterprise:** Enterprise Gateway, compliance messaging, partnerships with support/commerce/CRM providers; analytics upsell.

Lead every message with *"works in your assistant today,"* never with *"the new standard for the AI web"* - earn that status through adoption.

## 8. Competitive Landscape and Moat

AI2Web complements, never competes: any transport standard's success is a tailwind because AI2Web generates it. MCP = agent-to-tool; ACP = checkout; OAuth = auth; AI2Web = the model + discovery + measurement above them.

**Moat, in order of durability:**
1. **The capability model** - the schema everything else derives from; hard to displace once adopted.
2. **The Discovery Network** - network effects from an installed base + a verified index no transport provides.
3. **Capability-level analytics** - *semantic* insight ("agents requested `checkout` 200×, you don't expose it → lost conversion"; category benchmarking) that only exists because we own the model + network + connector. This is the key differentiator from commodity AI-bot analytics that infrastructure players (e.g. Cloudflare) already give away - they see packets, not capabilities.

## 9. Traction / Current State

Built and verified: the `ai2w` v0.1 spec + JSON Schema + a passing 8-case conformance suite; three reference SDKs (TypeScript, PHP, Python) that independently reproduce the conformance contract; a WordPress/WooCommerce plugin; the framework (validator, server with a Cloudflare Workers adapter, MCP bridge, agent connector); a Discovery Network stub; a live marketing site + web validator; and a completed security review (token-exfiltration, SSRF, and directory-poisoning findings fixed). Each component is structured as an independent repository under the `ai2web-foundation` organisation.

## 10. Roadmap (next 6 months)

- **M1–2:** public launch of spec + validator + WordPress plugin; both-sided connector demo in Claude/ChatGPT; first design partners.
- **M3–4:** hosted Cloud alpha; Discovery Network production (D1/Workers); vertical-depth push; Go/.NET SDKs; RFC-0002–0006.
- **M5–6:** Cloud beta; capability-analytics alpha; Enterprise Gateway design; certification framework; Shopify research.

## 11. Financial Model (assumptions)

Costs in the first year are dominated by engineering and developer relations; infrastructure is modest (edge-hosted, mostly stateless). Unit economics favour the model: near-zero marginal cost per hosted endpoint, high gross margin on Cloud/analytics, and enterprise contracts (gateway + analytics + support) providing high-ACV anchors. Revenue ramps on the standard adoption curve - developer adoption precedes business monetisation by 2–4 quarters, so early KPIs are adoption metrics (see §13), not revenue.

## 12. Risks and Mitigations

- **"Just an MCP wrapper."** → Lead with breadth (whole-site model + events + directory + A2A) and the analytics moat; the positioning doc must win this. Mitigated by the indispensable-core argument (§3).
- **A big vendor ships a competing standard.** → Stay vendor-neutral and adapter-based; adoption of any standard strengthens us.
- **Ecosystem already moved** (ACP-on-Woo, WP Abilities). → Differentiate on unification + non-commerce breadth; ship the validator as immediate standalone value.
- **Cold-start / network density.** → Both-sided ownership + vertical depth; don't chase breadth early.
- **Analytics commoditisation by infra players.** → Compete only at the capability level, which requires the schema they lack.
- **Protocol scope creep.** → Keep the spec tiny; push everything into adapters and the framework.

## 13. Success Metrics

- **Developer:** GitHub stars, SDK downloads, validator runs, plugin installs, conformant implementations.
- **Ecosystem:** AI-ready sites in the Discovery Network (by vertical), connector installs, AI platforms referencing AI2Web.
- **Business:** Cloud signups, paid conversions, certified sites, enterprise pipeline, analytics ARR.

## 14. Governance and The Ask

The Foundation/Polarize split (see [GOVERNANCE.md](../GOVERNANCE.md)) keeps the standard credibly neutral while the company captures value. Funding, if raised, is deployed against: (1) engineering to complete Cloud + analytics + gateway, (2) developer relations to drive adoption, and (3) go-to-market in the anchor vertical. The strategic goal is unchanged: **become the default interface between websites and AI agents.**
