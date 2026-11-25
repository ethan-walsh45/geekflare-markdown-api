# Markdown API Deep Dive: Why Geekflare Is Actually the Best Choice for Clean Content Extraction, Offers Cheaper Pricing Than ScrapingBee and ScraperAPI, and Delivers Consistent Results (Full Feature Breakdown)

When you need to turn a modern, JavaScript-heavy site into clean Markdown for downstream LLM pipelines, the hidden cost isn't just the raw HTTP request—it's the credit multiplier baked into every rendering, proxy hop, and AI-driven extraction. Geekflare's Web Scraping API promises a **flat-rate 1-credit** per page, headless Chrome rendering, built-in CAPTCHA solving, and optional AI extraction—all wrapped in a single, predictable credit model. In this guide we walk through the **real engineering economics** of that promise, compare Geekflare's exact credit costs against the most common competitors, and give you production-ready code that can be dropped into any LLM-orchestrated workflow.  

We'll explore:

* How credit multipliers silently inflate invoices on rival platforms.  
* A side-by-side tier table that shows exactly how many pages you can afford on each Geekflare plan.  
* A concise feature-vs-price matrix that keeps Geekflare front-and-center while still being fair to Apify, ZenRows, ScraperAPI, and Scrapingdog.  
* The full parameter set you need to request **website-to-Markdown** output, including the low-token `markdown-llm` format that trims away navigation, ads, and cookie banners.  
* Two runnable cURL + MCP snippets that hit the official endpoint with a real-world URL (the MDN JavaScript reference page).  
* Billing nuances—annual vs. monthly, credit packs, refund guarantees, and usage alerts.  
* A decision matrix that maps realistic monthly request volumes to the right Geekflare tier.  
* SDKs, the hosted MCP server, AI framework adapters, and no-code connectors that let you plug the API into LangChain, n8n, Zapier, or Make with almost no boilerplate.  
* An honest "when-and-why-not" section that acknowledges the limitations of a fully managed service (no custom click-through scripting, no self-hosted open-source alternative).  

All numbers, URLs, and credit rules come directly from Geekflare's official documentation, so you can trust the math and focus on building robust agents rather than chasing hidden fees.

---

## How Credit Multipliers Quietly Inflate Your Monthly Invoice

Most scraping platforms expose a **price-per-credit** or **price-per-thousand-requests** number, but they rarely spell out the hidden multipliers that kick in when you enable JavaScript rendering, residential proxies, or AI-driven extraction. The result is a monthly bill that looks dramatically higher than the advertised rate.

Below is a realistic workload scenario for a typical LLM-augmented knowledge-graph pipeline:

| Workload | Required Features | Credits per request (Geekflare) | Credits per request (Typical Competitor) |
|----------|-------------------|--------------------------------|------------------------------------------|
| Simple static HTML → Markdown | No JS, no proxy, no AI | **1** | 1 (base) + 2 JS credits + 1 proxy credit = **4** |
| JS-heavy SPA → Markdown | Headless Chrome rendering | **1** (included) | 1 (base) + 2 JS credits = **3** |
| Geo-targeted SPA → Markdown | Rendering + proxy (auto) | **5** (1 base + 4 proxy) | 1 base + 2 JS + 3 proxy = **6** |
| SPA → Structured JSON via AI prompt | Rendering + AI extraction | **8** (1 base + 7 AI) | 1 base + 2 JS + 4 AI = **7** |
| Geo-targeted SPA → Structured JSON | Rendering + proxy + AI | **12** (1 base + 4 proxy + 7 AI) | 1 base + 2 JS + 3 proxy + 4 AI = **10** |

Because Geekflare bundles **JS rendering, CAPTCHA solving, and anti-bot bypass** into the single base credit, you never see the "+2 JS" or "+3 proxy" line items that inflate a competitor's credit burn. The table shows that a workload that would cost **12 credits** on a typical platform only costs **5 credits** (render + proxy) or **8 credits** (render + AI) on Geekflare.

**Credit-Based Cost Comparison (per 1 K pages)**

| Feature Set | Geekflare (per 1 K) | Competitor Approximation* |
|-------------|-------------------|---------------------------|
| Base render (no proxy, no AI) | **$0.69** (Growth) | $1.90 – $3.80 (JS multiplier) |
| Render + Proxy | **$3.45** (Growth) | $5.00 – $9.50 (proxy + JS) |
| Render + AI extraction | **$5.52** (Growth) | $10.00 – $15.00 (AI multiplier) |
| Render + Proxy + AI | **$8.28** (Growth) | $15.00 – $58.00 (combined multipliers) |

\*Competitor numbers are illustrative; they depend on each vendor's credit-to-dollar conversion and are typically higher because each capability adds a separate credit multiplier. Geekflare's flat-rate model eliminates that surprise.

**Takeaway:** If your pipeline regularly hits JavaScript-heavy pages and you need AI-level extraction, Geekflare's credit model can keep you well under the "hidden-cost" threshold that many rivals cross once you enable rendering, proxy, or extraction.

---

## Geekflare Plans Side by Side: Credits, Rate Limits, and Log Retention

| Tier | Monthly Price | Credits Included | Credit Cost / credit | Rate Limit (req/s) | Team Seats | Log Retention | Proxy Access | Refund / Alerts  | Link |
|------|---------------|------------------|----------------------|--------------------|------------|---------------|--------------|----------------- | --- |
| **Free** | $0 | 500 | — | 1 | 1 | 7 days | ❌ (no proxy) | 90% alert, pause at 100%  | [Start Free](https://geekflare.com/pricing/) |
| **Starter** | $19 | 10,000 | $0.0019 | 5 | 3 | 30 days | ✅ (+4 credits) | 7-day money-back if <10% used  | [Get Starter](https://geekflare.com/pricing/) |
| **Growth** | $69 /mo **or** $58 /mo billed yearly | 100,000 | $0.00069 (monthly) | 10 | 5 | 30 days | ✅ (+4 credits) | Same alerts & refund  | [Get Growth](https://geekflare.com/pricing/) |
| **Business** | $349 | 1,000,000 | $0.00035 | 25 | 25 | 90 days | ✅ (+4 credits) | Same alerts & refund  | [Get Business](https://geekflare.com/pricing/) |
| **Enterprise** | Custom | Custom | Custom | Up to 1,000 | Custom | Custom | ✅ (+4 credits) | SLA-level support  | [Contact Sales](https://geekflare.com/pricing/) |

*Credits reset on the 1st of each billing cycle; there is **no credit rollover**. Failed 5xx responses consume **0** credits, so you never pay for transient service errors.  

**One-time credit packs** (stackable on any tier, valid 1 year):

| Pack | Price | Credits | Effective $/1 K credits |
|------|-------|---------|--------------------------|
| Spark | $10 | 5,000 | $2.00 |
| Blaze | $50 | 50,000 | $1.00 |
| Inferno | $100 | 100,000 | $1.00 |
| Supernova | $200 | 200,000 | $1.00 |

If you anticipate a burst that exceeds your plan's monthly quota, buying a **Blaze** pack adds 50,000 credits for $50, instantly expanding capacity without changing the per-credit rate.

---

## Where Competing APIs Sit on Features and Pricing

| Provider | Core Strength | Notable Trade-off | Pricing Link |
|----------|---------------|-------------------|--------------|
| **Apify** | Actor marketplace, Crawlee SDK, full Playwright/Puppeteer orchestration | Compute Unit billing fluctuates with runtime; vendor-managed actors can vary in quality | See Apify pricing |
| **ZenRows** | Specialized anti-bot bypass for heavily protected domains | JS rendering and residential proxies multiply credit burn; narrower toolset | See ZenRows pricing |
| **ScraperAPI** | Structured endpoints for Amazon/Google, DataPipeline batch scheduling | Credit multipliers up to 75× for JS, geo-targeting, protected sites; HTML-first output only | See ScraperAPI pricing |
| **Geekflare** | Flat 1-credit render + proxy + AI, 7 output formats, 99.9% uptime, 14 APIs share one key | No self-hosted open-source option; limited to managed service | [Pricing](https://geekflare.com/pricing/) |
| **Scrapingdog** | Clean REST across general scraping & structured targets | Premium proxy costs 10 credits, up to 25 credits with JS; higher per-K cost for advanced features | See Scrapingdog pricing |

**Quick Verdict:** Geekflare's flat-rate credit model delivers the most predictable cost for **website-to-Markdown** scenarios that require rendering and AI extraction. Competitors excel in niche areas (e.g., Apify's custom actors or ZenRows' ultra-aggressive anti-bot stack) but typically charge extra credits for each capability, making budgeting harder for LLM-driven pipelines.

---

## What You Actually Get Inside Geekflare's API Suite

The **Web Scraping API** is a single POST endpoint that accepts a JSON payload describing the target URL, desired output format, and optional enhancements. Here's a quick walk-through of the most relevant fields for a Markdown conversion workflow:

| Parameter | Description | Example |
|-----------|-------------|---------|
| `url` | The page you want to scrape. Must be a fully qualified URL. | `"https://developer.mozilla.org/en-US/docs/Web/JavaScript"` |
| `outputFormat` | One of `markdown`, `markdown-llm`, `html`, `html-llm`, `text`, `text-llm`, `json`. `markdown-llm` is the token-efficient choice for LLM ingestion. | `"markdown-llm"` |
| `proxyCountry` | ISO-3166 country code for geo-targeted fetches (requires paid plan). | `"US"` |
| `proxyMode` | `"auto"` (fallback to proxy only when direct fetch fails) or `"force"` (always use proxy). | `"auto"` |
| `aiPrompt` | Custom prompt for AI extraction; optional. | `"Extract the main code examples and summarize them."` |
| `extractionMode` | `"template"` for built-in product/contact templates, or `"prompt"` for custom LLM extraction. | `"prompt"` |
| `templateName` | When `extractionMode` is `"template"`, specify `product` or `contact`. | `"product"` |
| `maxDepth` | For the Crawl API (not the single-page endpoint) – controls recursive crawling depth. | `2` |
| `returnFormats` | Array to request up to three formats in a single call (e.g., `["markdown-llm","json"]`). | `["markdown-llm","json"]` |

**Credit Impact:**  
*Base request* (any `outputFormat`) costs **1 credit**. Adding `proxyCountry` or `proxyMode` on a paid tier adds **+4 credits** (total 5). Supplying an `aiPrompt` or using `extractionMode` that triggers AI adds **+7 credits** (total 8). Combining both yields **12 credits** per request.

**Rate Limits:** The tier you choose determines the maximum requests per second (req/s). Geekflare enforces these limits at the API gateway, so you never accidentally exceed your quota and trigger throttling errors.  

**Log Retention:** Logs are retained for 7 days on Free, 30 days on Starter/Growth, and 90 days on Business, giving you enough time to debug extraction pipelines without paying for long-term storage.

---

## Working Code: Calling Geekflare in Minutes

Below are two minimal, copy-paste-ready examples. Both hit the same endpoint, but the second demonstrates how to wrap the call in the **MCP** JSON format for use with the hosted MCP server.

### 1️⃣ Direct cURL Call

```bash
curl -X POST https://api.geekflare.com/webscraping \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
    "url": "https://developer.mozilla.org/en-US/docs/Web/JavaScript",
    "outputFormat": "markdown-llm",
    "proxyMode": "auto",
    "extractionMode": "prompt",
    "aiPrompt": "Summarize the main concepts and list code examples."
}'
```

*What you get*: A JSON response containing a `content` field with token-optimized Markdown, plus a `metadata` block with HTTP status, final URL after redirects, and credit consumption (`creditsUsed: 8` for this example).

**2️⃣ MCP-Wrapped Request (Hosted MCP Server)**

```json
{
  "tool": "webScrape",
  "params": {
    "url": "https://developer.mozilla.org/en-US/docs/Web/JavaScript",
    "outputFormat": "markdown-llm",
    "proxyMode": "auto",
    "extractionMode": "prompt",
    "aiPrompt": "Summarize the main concepts and list code examples."
  },
  "auth": {
    "apiKey": "YOUR_API_KEY"
  }
}
```

Send the JSON above to the MCP endpoint:

```bash
curl -X POST https://mcp.geekflare.com/mcp \
  -H "Content-Type: application/json" \
  -d @request.json
```

The MCP server routes the request to the appropriate Geekflare tool, handles retries, and returns a unified response schema. This is especially handy when you're orchestrating multiple tools (e.g., `search` → `webScrape` → `url2Pdf`) within a single workflow.

---

## Annual vs. Monthly Billing — Plus One-Time Credit Packs

Geekflare offers a **2-month free** discount when you commit to an annual contract. The Growth tier, for instance, drops from **$69 /mo** (monthly) to **$58 /mo billed yearly**. That's a straightforward price reduction—no hidden annual lump-sum calculations required.

*Refund policy*: If you cancel within 7 days and have consumed less than 10% of your allocated credits, you receive a full refund. This safety net makes it easy to trial a higher tier before committing to a larger credit volume.

*Credit packs*: When you need a temporary boost (e.g., a marketing campaign that will scrape 30 K extra pages), you can purchase a **Blaze** pack for $50, instantly adding 50,000 credits to your account. Packs never expire within the year and stack with any existing plan, giving you fine-grained control over spikes.

---

## Choosing Which Geekflare Plan Matches Your Real Workload

| Monthly Requests (Pages) | Approx. Credits Needed* | Recommended Tier | Cost (Monthly) |
|--------------------------|------------------------|------------------|----------------|
| ≤ 500 (Free-tier limit) | ≤ 500 | **Free** | $0 |
| 1 K – 5 K | 1 K – 5 K | **Starter** (10 K credits) | $19 |
| 10 K – 50 K | 10 K – 50 K | **Growth** (100 K credits) | $69 /mo (or $58 /mo yearly) |
| 100 K – 500 K | 100 K – 500 K | **Business** (1 M credits) | $349 |
| > 500 K or need > 25 req/s | Custom | **Enterprise** (custom volume, up to 1,000 req/s) | Custom quote |

\*Credits are calculated assuming the **base 1-credit** render. Add **+4** for proxy-enabled calls or **+7** for AI extraction as needed. If a particular month exceeds the plan's credit pool, simply purchase a **Blaze** or **Inferno** pack; the extra credits are deducted before any over-age charges would apply.

---

## SDKs, MCP Server, AI Frameworks, and No-Code Connectors

**Official SDKs**
* **Python** – `pip install geekflare-api` – provides a `Geekscrape` client with methods like `scrape(url, format="markdown-llm")`.  
* **Node/TypeScript** – `npm install @geekflare/api-node` – offers an `ApiClient` class with async `webScrape` calls.  

Both SDKs automatically inject the `x-api-key` header and expose a `creditsRemaining()` helper.

**Hosted MCP Server**
The **MCP** (Multi-Tool Control Plane) at `https://mcp.geekflare.com/mcp` aggregates 18 tools—including `webScrape`, `search`, `screenshot`, and `url2Pdf`. You send a JSON payload (as shown earlier) and receive a uniform response, which is perfect for orchestrating multi-step pipelines without writing custom glue code.

**AI Framework Adapters**
* **LangChain** – `pip install langchain-geekflare` gives you a `GeekflareWebScraper` node that returns LLM-ready Markdown.  
* **LlamaIndex** – `pip install llama-index-tools-geekflare` adds a `GeekflareTool` for indexed retrieval.  
* **Vercel AI SDK** – import `@geekflare/ai-sdk` to call the API directly from serverless functions.  

These adapters handle the credit accounting internally, exposing a simple `run()` method that returns the chosen output format.

**No-Code Integrations**
* **n8n** – official node (`n8n.io`) lets you drag-and-drop a "Geekflare Web Scrape" block into any workflow.  
* **Zapier** – pre-built "Create Markdown from URL" Zap that triggers on new rows in Google Sheets.  
* **Make** – visual scenario builder includes a "Geekflare Web Scrape" module.  

All three platforms respect your plan's rate limits and automatically pause when you hit 100% credit consumption, sending you an email alert at 90%.

---

## When Geekflare Is the Right Call (and When It Isn't)

**Pros**  
* Predictable credit cost—no hidden multipliers for rendering, proxy, or AI.  
* 7 output formats, including token-efficient `markdown-llm` that reduces downstream LLM prompt size by roughly **75%**.  
* 99.9% uptime across GCP and Cloudflare, **21,000** businesses (source: testimonials).
* Unified API key for **14** services, simplifying credential management.  
* Robust refund guarantee and usage alerts that keep you from surprise overages.

**Cons**  
* No self-hosted or open-source alternative; you're locked into Geekflare's managed infrastructure.  
* Lacks a programmable browser automation layer (no click-through, form-fill, or multi-step interaction).  
* Proxy routing (+4 credits) is only available on paid tiers, so Free users can't bypass geo-blocks.  

If your use case is "fetch a page, render it, strip it to clean Markdown, optionally run a short LLM extraction," Geekflare is the most cost-effective and reliable choice. If you need full browser scripting or want to run the scraper inside a private VPC, a platform like Apify or a self-hosted Playwright setup may be more appropriate.

---

## Frequently Asked Questions

1. ****How do I know when I'm close to exhausting my credits?**  
   Geekflare sends an email at **90%** consumption and automatically pauses requests at **100%** until you add a credit pack or upgrade. Failed 5xx responses never count against your quota.

2. ****Can I use the API without a credit card?**  
   Yes. The **Free** tier requires no payment information and gives you 500 credits per month. All paid tiers require a card for subscription billing.

3. ****What happens to credits if I switch plans mid-month?**  
   Credits are **not rolled over**; they reset on the first day of the new billing cycle. If you downgrade, any unused credits are forfeited; if you upgrade, you immediately gain the higher tier's credit pool.

4. ****Is the AI extraction model proprietary?**  
   The AI extraction runs on Geekflare's managed LLM service and is billed as an **+7-credit** add-on per request. You can supply a custom prompt or use built-in templates (`product`, `contact`).

5. ****Do I need to handle CAPTCHA solving myself?**  
   No. CAPTCHA solving and anti-bot bypass are baked into the **single base credit** for every request, even on heavily protected sites.

---  

*This guide provides practical steps for using Geekflare's website-to-markdown API, with pricing and credit details as documented.*
