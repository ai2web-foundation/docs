# AI2Web - Competitive & Ecosystem Landscape

> Where AI2Web sits relative to everything adjacent. Complement, don't compete.
> Status: v1 · 2026-07-09 · Revisit quarterly - this space moves fast.

## The one-line map

> **robots.txt** told crawlers a site exists. **sitemap.xml** listed its pages. **schema.org** described its entities. **OpenAPI** described its API. **OAuth** handled identity. **MCP** lets agents call its tools. **AI2Web sits above all of them** - it describes the *whole business* to agents, indexes who's AI-capable, and measures the agent economy.

## Layer-by-layer

| Standard / product | What it does | What it does NOT do | AI2Web's relationship |
|---|---|---|---|
| **MCP** (Anthropic) | Tool-invocation transport; agents call tools on a known server | Discovery across sites; describe a business; measurement; events | **Generate it** via `@ai2web/mcp-bridge`. MCP is one output adapter + one transport module. |
| **WebMCP** | Expose browser-side tools from a running page | Backend/headless use; whole-site description; cross-site discovery | Optional **later adapter**. Frontend-coupled → out of core MVP. |
| **ACP / OpenAI Commerce / Instant Checkout** | Products + checkout inside ChatGPT | Anything non-commerce (support, bookings, content, events); vendor-neutral | Commerce is **one module**; ACP an optional adapter. We do NOT rebuild checkout. |
| **OpenAI product feed** | Structured product data → OpenAI index | Whole-site; actions; events; other vendors | **Generate it** as one feed output. |
| **Google AP2 / UCP** | Agent payments / universal commerce | Non-commerce; discovery/description | Future adapter if it gains traction. |
| **WordPress Abilities API + MCP Adapter** | Register WP abilities → MCP tools | Non-WP; whole-site model; directory; analytics | Our WP plugin can **build on / coexist with** it; we add the model + adapters + directory. |
| **OpenAI Apps SDK** | Build ChatGPT apps (needs an MCP server) | The website side; neutrality | Our **agent-side connector** ships as an Apps-SDK app; the MCP server it needs is what AI2Web sites expose. |
| **llms.txt** | Markdown hint file for LLMs at site root | Structured capabilities; actions; auth; events; machine schema | We can emit it; AI2Web is the structured superset. |
| **schema.org / JSON-LD** | Entity markup for search engines | Actions; auth; agent transport; discovery index | Reuse its vocab where sensible; AI2Web is the *capability + action* layer. |
| **OpenAPI / REST / GraphQL** | Describe/serve an API | Discovery of *which* site; business description; agent-oriented capability model | Point to them from the manifest; reuse as transports. |
| **OAuth 2.1 / PKCE** | Delegated auth | Everything else | **Use as-is** for scoped agent actions. Never invent auth. |
| **Cloudflare AI bot analytics** | Show AI crawler/bot traffic | Capability-level/semantic insight; demand-vs-supply; benchmarking | The commodity floor. Our analytics is defensible only *above* this (capability-level). |

## Who might build this and why we can still win

- **Anthropic / OpenAI** - incentivised toward *their* assistant, not a neutral cross-vendor layer. Neutrality is our wedge; a site won't want to bet its AI surface on one vendor.
- **Cloudflare** - owns traffic + could add a directory/analytics. Real threat on the *analytics floor*. We defend by owning the **capability schema** they don't have (they see packets, not capabilities). Also a potential partner/acquirer.
- **WordPress / Automattic / Shopify** - platform-shaped; each covers its own island. We're the cross-platform layer. Risk if one ships a great neutral model first → mitigate with speed + multi-CMS breadth.
- **A generic "MCP directory" startup** - plausible, but a directory without the capability model + both-sided connector + analytics is thin. Our edge is the full stack, not just the list.

## Positioning takeaways

1. **Always "complement, never replace."** Every adjacent standard's success is our tailwind because we generate it.
2. **Lead with breadth + "works today," not standard-setting.** The demo beats the manifesto.
3. **Guard the capability schema** - it's the thing infra players can't cheaply copy and the root of the directory + analytics moat.
4. **Neutrality is a feature.** Vendor-agnostic is the reason a business adopts us over a single assistant's native path.
