# Best Free/Freemium AI CMS for Astro / React (2026) — with facts

I evaluated every CMS on:

1. AI capabilities
2. Astro/React compatibility
3. Free tier value
4. Developer experience
5. Performance
6. Workflow
7. Ecosystem
8. Hosting flexibility

My goal: **best free AI-enabled CMS for startups like [Hypotenuse Analytics](https://www.hypotenuseanalytics.com?utm_source=chatgpt.com)**

---

# 1. [DatoCMS](https://www.datocms.com/?utm_source=chatgpt.com) — **My #1 Pick**

Why?

### AI

Built-in:

* AI translation
* alt text generation
* bulk content generation

Fact:

* includes **~1,200 AI credits** for alt-text on free tier.

---

### Free tier

* **3 projects**
* **5,000 records**
* **1 GB storage**
* **100,000 API calls/month**

That is enough for:

* ~500 blogs
* ~50 landing pages
* full startup website

---

### Performance

* global CDN
* image optimization built in
* static Astro builds = **<100ms content delivery**

---

### Why #1:

No extra plugins. No extra hosting.

**Score: 9.5/10**

---

# 2. [Sanity](https://www.sanity.io/?utm_source=chatgpt.com) — Best for developers

Why?

### AI

Built-in AI Assist:

* generation
* translation
* structured suggestions

Fact:

* **100 AI credits/month free**

---

### Free tier

* **20 users**
* **10,000 documents**
* **100,000 CDN requests/month**

Huge advantage:
team collaboration.

---

### Why not #1?

Schema-as-code means:

* more setup
* steeper learning curve

For dev teams: amazing.

**Score: 9/10**

---

# 3. [Directus](https://directus.io/?utm_source=chatgpt.com) — Best self-hosted

Why?

### Cost

* open source
* **$0 license**
* unlimited records

Fact:
free under **$5M annual revenue**.

---

### AI

Directus MCP:

* transcription
* translation
* alt text
* prompts

No monthly AI cap if self-hosted.

---

### Tradeoff

Need:

* PostgreSQL
* server
* DevOps time

Good if you can manage infra.

**Score: 8.8/10**

---

# 4. [Cosmic](https://www.cosmicjs.com/?utm_source=chatgpt.com) — Best AI agents

Why unique?

Built-in agents:

* content agent
* code agent
* browser automation

Not many CMS do this.

---

### Free tier

* **1 bucket**
* **2 users**
* **1,000 objects**

Great for MVP.
Bad for scale.

**Score: 8.7/10**

---

# 5. [Builder.io](https://www.builder.io/?utm_source=chatgpt.com) — Best for marketers

Why?

* visual editor
* AI generated UI blocks

Free:

* unlimited projects
* **2,000 API calls/month**

Issue:
API cap is small for growing sites.

**Score: 8.5/10**

---

# 6. [Strapi](https://strapi.io/?utm_source=chatgpt.com)

Good:

* unlimited self-hosted
* strong ecosystem

Bad:
AI plugin only on paid plan:

* starts around **$15/seat/month**

So not truly “free AI”.

**Score: 8/10**

---

# 7. [WordPress](https://wordpress.org/?utm_source=chatgpt.com)

People think free.

Reality:

* hosting: ~$25–100/month
* premium plugins: $50–500/year
* AI plugins mostly paid

Hidden cost = high.

**Score: 6.5/10**

---

# 8. [Ghost](https://ghost.org/?utm_source=chatgpt.com)

Great for:

* blogs
* newsletters

Hosted starts:

* **$29/month**

No native AI.

Needs third-party tools.

**Score: 6/10**

---

# My recommendation by scenario

### If you’re a startup:

→ **DatoCMS**
Why:
5k records + AI + zero infra.

---

### If you’re engineering-heavy:

→ **Sanity**
Why:
20 users free + 10k docs.

---

### If you want full control:

→ **Directus**
Why:
unlimited + open source.

---

### If you want AI automation:

→ **Cosmic**
Why:
actual AI agents.

---

### If marketing owns content:

→ **Builder.io**
Why:
visual editing.

---

# What I’d use for Hypotenuse Analytics

Stack:

* Astro
* React
* [DatoCMS](https://www.datocms.com/?utm_source=chatgpt.com)

Why exactly?

Because:

* 5,000 records is enough for years initially
* 100k API calls handles strong traffic
* 1GB media enough for startup site
* built-in AI saves tooling cost
* $0 hosting overhead

That’s the best **ROI**.

---

## Final ranking

🥇 DatoCMS — best balance
🥈 Sanity — best dev flexibility
🥉 Directus — best self-hosted
4. Cosmic
5. Builder.io

**My bet for 2026: DatoCMS wins for most teams.**


# AI-Enhanced Free/Freemium CMS Options for Astro / React Sites  

Below is a side-by-side assessment of the most viable head-less CMS platforms that offer **free or freemium AI capabilities** and work well with static-site generators such as Astro or component-based frameworks like React. The comparison follows the eight dimensions you asked for.

---  

## 1. Sanity  

| Dimension | Findings |
|-----------|----------|
| **AI features** | Sanity ships **AI Assist** for content generation, alt-text, translation and structured-content suggestions directly in the Studio. The **Content Agent** can run bulk edits, audits and scheduled automation via serverless functions. [1][2] |
| **Astro/React compatibility** | Provides a **schema-as-code** model and a fully-featured **JavaScript/TypeScript SDK**. The SDK works with any framework (React, Vue, Astro) and supports both REST and GROQ queries. Real-time listeners make preview-first development easy. [3][2] |
| **Cost / free tier** | **Free tier**: 20 users, 10 K documents, 100 K CDN requests, 100 AI credits per month. Pay-as-you-go for extra usage; growth plan is $15 / seat / mo. [2] |
| **Setup & DX** | CLI (`sanity`) creates a project in minutes; the Studio is fully customizable via React code. Documentation is extensive and includes starter schemas. Learning curve is moderate because of the need to define schemas in code. [3] |
| **Performance & SEO** | Because content is delivered from the **Content Lake** (global CDN) and can be statically queried at build time, Astro sites get sub-100 ms responses and full control over meta tags. No server-side rendering overhead. [1] |
| **Content modeling & workflow** | Schema-as-code, versioned documents, preview drafts, scheduled releases (Enterprise) and granular role-based permissions. Real-time collaboration is built-in. [1] |
| **Community & ecosystem** | Large open-source plugin ecosystem, official App SDK for custom apps, active Discord and GitHub community. [1] |
| **Hosting / CDN** | Managed SaaS; assets are stored in the Content Lake and served through Sanity’s CDN. No self-hosting required, but you can also self-host the Studio if desired. [2] |

**Strengths** - Robust AI Assist, excellent SDK, fast CDN, strong versioning.  
**Weaknesses** - Free tier limits on documents and AI credits; schema-as-code may be heavy for non-developers.

---  

## 2. WordPress (headless)  

| Dimension | Findings |
|-----------|----------|
| **AI features** | Core WordPress does **not** include built-in AI, but AI plugins (e.g., Ghost-style agents, third-party editors) can be added. The ecosystem is massive, yet most AI tools are paid or require custom integration. |
| **Astro/React compatibility** | Exposes a **REST API** and, when WPGraphQL is installed, a GraphQL endpoint that Astro can query at build time. Guides show fetching posts with `fetch` or `graphql-request`. [4][5] |
| **Cost / free tier** | WordPress core is free, but production-grade hosting, premium plugins, and AI extensions quickly add cost (typical managed WP hosting $25-$100 / mo, plus plugin licences). No built-in AI-credit limits, but hidden fees are common. [6] |
| **Setup & DX** | Requires a separate PHP server, plugin installation, and possibly a GraphQL plugin. Documentation is abundant but the stack can be fragile; updates may break custom code. [7][5] |
| **Performance & SEO** | Headless WordPress adds latency (REST/GraphQL calls) and often needs caching plugins to reach Lighthouse scores >90. Static-site generation via Astro mitigates this but build times increase with large datasets. [7] |
| **Content modeling & workflow** | Content types are defined via custom post types and ACF fields; versioning is limited to revisions. No native preview-mode for static sites, but Draft Mode in Next.js can be used with Astro via custom scripts. [8] |
| **Community & ecosystem** | Largest CMS ecosystem; thousands of plugins and themes. AI-related plugins exist (e.g., AI-generated excerpts) but often require paid upgrades. [7] |
| **Hosting / CDN** | Typically self-hosted or managed WordPress hosting. CDN must be added separately (Cloudflare, Jetpack, etc.). No built-in CDN for content API. [6] |

**Strengths** - Familiar UI, massive plugin market, free core.  
**Weaknesses** - No native AI, higher operational overhead, performance penalties for static builds, hidden costs.

---  

## 3. DatoCMS  

| Dimension | Findings |
|-----------|----------|
| **AI features** | **AI Assist** for translations, alt-text, and bulk content generation via OpenAI, Anthropic, Google Gemini or DeepL plugins. One-click bulk translation and smart image tagging are included in the free tier. [9][10] |
| **Astro/React compatibility** | Offers a **GraphQL API** and a TypeScript SDK that works with any framework, including Astro and React. The GraphQL schema is auto-generated from the visual model builder. [11] |
| **Cost / free tier** | **Free forever**: up to 3 projects, 1 collaborator, 5 K records, 1 GB storage, 100 k API calls/month. AI credits are included (e.g., 1 200 credits for alt-text). Paid plans start at €149 / mo for larger limits. [12][13] |
| **Setup & DX** | Visual schema builder, no code required for basic models; advanced users can use the **Schema-as-Code** CLI. Documentation is clear and includes starter projects for Astro. [11] |
| **Performance & SEO** | Global CDN with edge caching; GraphQL queries are highly efficient, making static-site generation fast. Built-in image API provides on-the-fly transformations. [9] |
| **Content modeling & workflow** | Visual editor for models, versioning, preview drafts, scheduled publishing, and multi-language support. Workflow extensions (webhooks, custom roles) are available on paid plans. [9] |
| **Community & ecosystem** | Growing community, official plugins marketplace, good docs. Not as large as WordPress but active. [9] |
| **Hosting / CDN** | Fully managed SaaS; assets stored in DatoCMS CDN. No self-hosting required. [12] |

**Strengths** - Strong native AI plugins, GraphQL-first, generous free tier for small projects.  
**Weaknesses** - Paid plans are euro-priced; some workflow features (e.g., advanced role-based permissions) are locked behind higher tiers.

---  

## 4. Directus  

| Dimension | Findings |
|-----------|----------|
| **AI features** | **Directus AI** (MCP server) offers transcription, image-alt generation, smart tagging, translation, and content-generation prompts. All operations run inside the Directus UI via the Model Context Protocol. [14][15] |
| **Astro/React compatibility** | Exposes a **REST API** and **GraphQL** endpoint; SDKs are community-maintained. Works with any static-site generator, including Astro, via simple fetch calls. [16] |
| **Cost / free tier** | **Self-hosted** version is **MIT-licensed and free** for any scale under $5 M revenue. Cloud-hosted plans start at $19 / mo, but the free open-source option eliminates usage caps. [17] |
| **Setup & DX** | Requires a Node/PostgreSQL server; CLI (`npx directus bootstrap`) creates a project quickly. The UI is low-code; schemas are defined via the admin UI or JSON files. Learning curve moderate for non-technical users. [16] |
| **Performance & SEO** | Data served from a PostgreSQL backend; can be cached via CDN or Netlify Edge Functions. No built-in image CDN, so you need external services for optimisation. |
| **Content modeling & workflow** | Fully custom collections, relational fields, versioning, drafts, and granular role-based permissions. No native scheduled releases (need custom hooks). |
| **Community & ecosystem** | Active open-source community, 500+ plugins, regular releases. AI extensions are still maturing. |
| **Hosting / CDN** | Self-hosted or Directus Cloud; CDN must be added separately. |

**Strengths** - Completely free, open source, strong AI via MCP, flexible schema.  
**Weaknesses** - Requires self-hosting and external CDN for assets; AI features need configuration.

---  

## 5. Cosmic  

| Dimension | Findings |
|-----------|----------|
| **AI features** | **Four built-in AI agents**: Content Agent (bulk edits, migrations), Code Agent (auto-generate PRs), Computer-Use Agent (browser automation) and Team Agent (Slack/WhatsApp). All run inside the workspace without extra services. [18][19] |
| **Astro/React compatibility** | Offers a **REST API**, TypeScript SDK, and first-class support for Next.js, Astro, Vue, Svelte, Remix, etc. The SDK is framework-agnostic and works with static-site generation. [20][19] |
| **Cost / free tier** | **Free plan**: 1 bucket, 2 team members, 1 000 objects, 3 AI agents (manual only). Paid plans start at $49 / mo with additional agents and higher limits. No credit-card required for the free tier. [20][21] |
| **Setup & DX** | CLI (`cosmic init`) creates a project, generates SDK, and can deploy to Vercel/Netlify in minutes. Documentation is concise; learning curve is low for developers familiar with REST. |
| **Performance & SEO** | Global CDN with edge caching; sub-100 ms API responses. Image pipeline (imgix) provides on-the-fly transformations, beneficial for SEO. |
| **Content modeling & workflow** | Schema defined via UI or JSON; supports locales, version history (paid), and webhooks. Real-time collaboration is limited on the free tier. |
| **Community & ecosystem** | Smaller than Sanity or WordPress but growing; active Discord and marketplace for extensions. |
| **Hosting / CDN** | Fully managed SaaS; CDN integrated. No self-hosting needed. |

**Strengths** - AI agents go beyond content (code generation, browser automation), generous free tier for prototyping, fast CDN.  
**Weaknesses** - Free tier limited to 2 users and 1 000 objects; advanced workflow (scheduled releases, multi-locale) requires paid plan.

---  

## 6. Ghost  

| Dimension | Findings |
|-----------|----------|
| **AI features** | No native AI, but third-party integrations (e.g., Asyntai chatbot, TaskAGI blog writer) add AI-generated posts, alt-text, and member-tier Q&A. Most integrations are paid or require a separate service key. [22][23] |
| **Astro/React compatibility** | Provides a **REST API** (`/ghost/api/v3/content`) that Astro can query. No GraphQL out-of-the-box. |
| **Cost / free tier** | **Free** self-hosted Ghost is open source; Ghost(Pro) starts at $29 / mo for up to 1 000 members and includes built-in membership tools. Free tier has no AI. |
| **Setup & DX** | Simple Node.js install or Docker; admin UI is clean. No CLI for schema - content types are fixed (posts, pages, tags). |
| **Performance & SEO** | Extremely fast static output; built-in SEO fields and automatic sitemap. |
| **Content modeling & workflow** | Fixed content types, versioning via revisions, scheduled publishing, and membership tiers. No custom schemas without code extensions. |
| **Community & ecosystem** | Active community, many themes, but fewer plugins than WordPress. |
| **Hosting / CDN** | Managed Ghost(Pro) includes CDN; self-hosted requires external CDN. |

**Strengths** - Excellent performance, built-in membership, clean UI.  
**Weaknesses** - No native AI, limited schema flexibility, AI integrations are external and often paid.

---  

## 7. Strapi  

| Dimension | Findings |
|-----------|----------|
| **AI features** | **Strapi AI** (beta) provides content-model generation, translation and smart tagging. It runs as a plugin but is only available on Growth/Enterprise plans. [24][25] |
| **Astro/React compatibility** | Fully **REST** and **GraphQL** APIs; official TypeScript SDKs. Works with any static-site generator. |
| **Cost / free tier** | **Open-source** core is free with unlimited self-hosted usage. AI plugin requires a paid plan (Growth $15 / seat / mo). |
| **Setup & DX** | CLI (`strapi generate`) creates a project; schemas are defined in JavaScript/TS files. Good docs, but initial setup can be verbose. |
| **Performance & SEO** | Self-hosted; performance depends on your infra. Can be paired with CDN and edge functions for fast builds. |
| **Content modeling & workflow** | Flexible collection definitions, role-based permissions, versioning via plugins, draft-preview, and webhooks. |
| **Community & ecosystem** | Large plugin marketplace, active community, many tutorials. |
| **Hosting / CDN** | Self-hosted or Strapi Cloud (paid). CDN must be added separately. |

**Strengths** - Unlimited free core, flexible schema, strong ecosystem.  
**Weaknesses** - AI features locked behind paid tier; self-hosting adds ops overhead.

---  

## 8. Builder.io (Headless CMS for Astro)  

| Dimension | Findings |
|-----------|----------|
| **AI features** | **AI-generated sections** and content blocks via “Design-to-Code” AI, plus AI-assisted copy generation in the visual editor. [26] |
| **Astro/React compatibility** | Direct integration with Astro through a Qwik-based plugin; provides a TypeScript SDK and supports partial hydration. |
| **Cost / free tier** | **Free tier** includes unlimited projects, 2 000 monthly API calls, and AI-generated sections (no credit-card). Paid plans start at $49 / mo for higher limits. |
| **Setup & DX** | Drag-and-drop visual editor plus code export; developers can map UI components to Builder blocks. Learning curve low for marketers, moderate for devs mapping components. |
| **Performance & SEO** | Content delivered via Builder’s CDN; can be statically baked at build time for Astro, preserving SEO. |
| **Content modeling & workflow** | Visual model builder, versioning, preview, and role-based access. |
| **Community & ecosystem** | Growing community, official docs, and a marketplace of pre-built blocks. |
| **Hosting / CDN** | Fully managed SaaS with CDN; no self-hosting required. |

**Strengths** - Visual editor, AI-generated UI sections, easy Astro integration.  
**Weaknesses** - Free tier limits API calls; advanced AI (e.g., bulk translation) not yet available.

---  

## 9. Comparative Summary  

| Platform | Native AI (free) | Free-tier limits | Astro/React SDK | Self-hosted option | Overall suitability for “free + AI-assisted” |
|----------|-------------------|------------------|----------------|-------------------|----------------------------------------------|
| **Sanity** | AI Assist + Content Agent (limited credits) | 10 K docs, 100 AI credits/mo | ✅ (JS/TS SDK, GROQ) | Studio can be self-hosted | Strong for dev-centric sites that need AI but can live within modest content volume. |
| **WordPress** | No native AI; third-party plugins (often paid) | Unlimited core, but hosting & plugins add cost | ✅ (REST/GraphQL) | ✅ (self-host) | Good if you already own WP and accept extra plugins; not ideal for pure free AI workflow. |
| **DatoCMS** | AI translations, alt-text, bulk generation (free tier includes credits) | 5 K records, 1 GB storage, 100 k API calls/mo | ✅ (GraphQL SDK) | ❌ (SaaS only) | Excellent for small-to-medium projects that need built-in AI without extra services. |
| **Directus** | AI via MCP (free, open-source) | Unlimited (self-hosted) | ✅ (REST/GraphQL) | ✅ (MIT-licensed) | Best for teams comfortable managing a server and wanting fully free AI + flexible schema. |
| **Cosmic** | Four AI agents (free tier limited to manual agents) | 1 000 objects, 2 users | ✅ (REST + TS SDK) | ❌ (SaaS) | Ideal for prototyping AI-driven workflows (content + code) with low-cost upgrade path. |
| **Ghost** | No native AI; external paid bots | Free self-hosted core, $29 / mo for hosted | ✅ (REST) | ✅ (self-host) | Great for performance-first blogs, but AI will require extra paid services. |
| **Strapi** | AI plugin (paid) | Unlimited free core | ✅ (REST/GraphQL) | ✅ (self-host) | Good for fully free CMS, but AI requires upgrade. |
| **Builder.io** | AI-generated sections (free) | Unlimited projects, 2 000 API calls/mo | ✅ (Astro plugin) | ❌ (SaaS) | Perfect for marketers who want visual editing plus AI-generated UI, but limited API quota. |

---  

## 10. Recommendation  

1. **Primary Recommendation - DatoCMS**  
   *Why?* DatoCMS provides **native AI tools (translation, alt-text, bulk generation) that are usable on the free tier**, a **GraphQL-first SDK** that works out-of-the-box with Astro and React, and a **global CDN** for instant performance. The free limits (5 K records, 1 GB storage) are ample for most marketing sites or small product catalogs. No extra hosting costs are required because it is a managed SaaS.

2. **Second Choice - Sanity**  
   If you need **real-time collaboration**, a **highly customizable Studio**, and are comfortable working with schema-as-code, Sanity’s AI Assist and Content Agent give you on-the-fly content suggestions. The free tier is generous for early projects, and the SDK integrates smoothly with Astro.

3. **Self-Hosted Alternative - Directus**  
   For teams that want **zero-cost AI** and full control over the backend, Directus’ open-source version plus the MCP AI module delivers transcription, alt-text, and translation without any usage caps. It requires a PostgreSQL server and a CDN of your choice, but the cost is truly zero for most small sites.

4. **If you already own WordPress** - use WPGraphQL for Astro integration and add a **paid AI plugin** only when needed; otherwise the overhead outweighs the benefits for a free AI workflow.

5. **For AI-heavy automation (code generation, browser tasks)** - Cosmic’s AI agents are unique, but the free tier is restrictive. Consider it when you need **code-level automation** and are willing to upgrade early.

---  

### Bottom Line  

- **Free-tier AI + strong Astro/React support:** **DatoCMS** > **Sanity** > **Directus**.  
- **Full control with no SaaS lock-in:** **Directus** (self-hosted).  
- **Maximum ecosystem & familiarity:** **WordPress**, but expect extra costs for AI.

Choose the platform that matches your tolerance for hosting complexity, the depth of AI you need, and the projected content volume. For most developers building a modern Astro or React site with AI-assisted content creation, **DatoCMS** offers the best balance of built-in AI, generous free limits, and seamless framework integration.

---

### Sources
- [1] https://www.somar.co.nz/blog/sanity-cms-for-modern-websites
- [2] https://www.sanity.io/pricing
- [3] https://bejamas.com/hub/headless-cms/sanity
- [4] https://docs.astro.build/en/guides/cms/wordpress
- [5] https://hostwp.io/blog/astro-wordpress-integration-headless-guide
- [6] https://checkthat.ai/brands/ghost/pricing
- [7] https://www.browsercat.com/post/wordpress-vs-astro
- [8] https://www.sanity.io/answers/using-sanity-for-content-management-with-membership-tiers-and-user-submissions
- [9] https://www.datocms.com/features/ai
- [10] https://www.datocms.com/marketplace/plugins/i/datocms-plugin-ai-translations
- [11] https://devtune.ai/verticals/headless-cms-content-platforms/datocms
- [12] https://www.datocms.com/pricing
- [13] https://www.datocms.com/blog/new-pricing
- [14] https://directus.io/mcp
- [15] https://directus.io/tv/ai/ai-overview
- [16] https://directus.io/docs/guides/ai/assistant/setup
- [17] https://directus.io/pricing
- [18] https://www.cosmicjs.com/blog/cosmic-vs-sanity-ai-agents
- [19] https://www.cosmicjs.com/blog/the-power-of-headless-cms-with-ai-agents
- [20] https://www.cosmicjs.com/blog/best-headless-cms-2026
- [21] https://www.cosmicjs.com/changelog/best-cms-for-ai-saas
- [22] https://asyntai.com/ai-chatbot-for-ghost
- [23] https://taskagi.net/agent/ghost-cms-ai-blog-writer
- [24] https://focusreactive.com/blog/compare-open-source-cms-in-2026
- [25] https://strapi.io/blog/ai-and-headless-cms
- [26] https://www.builder.io/m/astro-cms
