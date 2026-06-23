[Google Search Scraper](https://apify.com/harvestlab/google-search-scraper?fpr=data)

Track Google rankings by keyword, country, and language. Paste search queries, get structured SERP results, rank-change snapshots, People Also Ask questions, featured snippets, webhook alerts, and optional AI SEO briefs.

**Best first run**

```
{
  "queries": ["best CRM software for small business"],
  "maxResults": 10,
  "country": "us",
  "language": "en",
  "includePeopleAlsoAsk": true,
  "includeRelatedSearches": true,
  "trackSerpDiff": false,
  "enableAiAnalysis": false
}
```

Use this actor when you need a lightweight Google rank tracker, SERP export, featured-snippet monitor, or SEO research feed without paying for a full Ahrefs or Semrush subscription. Start with one query and 10 results, then enable `trackSerpDiff` and `alertWebhookUrl` for scheduled rank-change alerts.

**Output:** one dataset item per query with organic results, positions, URLs, snippets, SERP features, block diagnostics, and optional AI content recommendations.

**Good fit:** SEOs, content teams, affiliate publishers, agencies, growth teams, and rank-tracking SaaS builders.

**Not magic:** Google can rate-limit repeated commercial queries. Keep first runs small; if you see `block_detected: true`, wait a minute, reduce batch size, or use residential proxy.

The actor uses dynamic Apify memory sizing: a one-query smoke test starts with lower memory, while multi-query, paginated, AI-analysis, or alerting runs scale up automatically. That keeps exploratory runs cheaper without asking users to tune memory manually.

Track SERP rankings and run AI SEO audits for **$0.003/query** - a pay-as-you-go alternative to **Ahrefs ($129/mo), Semrush ($139/mo), and SERPRobot ($49/mo)** rank trackers. A daily 50-query rank-tracking schedule costs about **$4.50/month** here vs **$129+/mo** on Ahrefs. Extract organic listings, featured snippets, knowledge panels, People Also Ask, and related searches across 30+ countries, with cross-run diff mode, push webhooks, and optional 5-provider AI SEO analysis.

## What it does

Google Search Scraper runs headless Chromium via Playwright to render Google's search results pages exactly as a real browser would — defeating the JavaScript-only detection that blocks simple HTTP scrapers. For every query you provide, it extracts the full SERP: organic results with title, URL, snippet, and position; featured snippets; knowledge panel entities; People Also Ask questions; and related searches. Results are localized by country code (`gl`) and language code (`hl`), so you get the same page a user in Germany, Japan, or Brazil would see.

Beyond raw extraction, the actor solves three real problems without requiring a SaaS subscription:

**Rank tracking at a fraction of SaaS cost.** Enable diff mode (`trackSerpDiff: true`) to snapshot SERP positions across scheduler runs. On every run the actor compares top-10 URLs against the previous snapshot keyed by `(query, country, language)`, flags domains that moved 2+ positions, identifies new entries and dropped domains, and records featured-snippet source changes — the same functionality that drives $50–200/month in Ahrefs, Semrush, and SERPRobot subscriptions.

**Push-based webhook alerts.** Point `alertWebhookUrl` at any Slack incoming webhook, Zapier catch-hook, Make scenario, or custom endpoint. Whenever a tracked query's SERP shifts significantly, the actor POSTs a structured JSON diff payload — no polling, no dashboard refreshes. Wire into Slack for an ops-channel alert or into n8n/Make to route rank-drops to email and rank-climbs to a Notion log.

**AI SEO analysis per query.** Enable `enableAiAnalysis` and choose your LLM provider (OpenRouter, Anthropic, Google AI, OpenAI, or self-hosted Ollama). The actor classifies search intent, identifies content gaps against the current top-10, and produces keyword and content recommendations — functionality that typically requires a separate SEO audit tool.

SEOs, content strategists, growth teams, and rank-tracking SaaS builders use this actor to monitor competitive positions and automate SERP intelligence at scale.

## Features

- **Playwright browser rendering**: Uses headless Chromium to render Google's JavaScript-heavy pages, bypassing anti-bot measures that block simple HTTP requests
- **Full organic result extraction**: Title, URL, displayed URL, snippet text, and position for every ranking result
- **Featured snippet extraction**: Captures the answer box when Google serves one, including source URL and snippet text
- **Knowledge panel extraction**: Extracts entity facts, descriptions, and structured data from knowledge panels
- **People Also Ask**: Captures PAA questions shown alongside results — ideal for content planning and FAQ targeting
- **Related searches**: Extracts suggestions from the bottom of the SERP for keyword expansion
- **Multi-country support**: Pass any ISO 3166-1 alpha-2 country code (`us`, `gb`, `de`, `fr`, `jp`, `br`, `in`, etc.) to localize results
- **Multi-language support**: Pass any ISO 639-1 language code (`en`, `de`, `fr`, `es`, `ja`, `pt`, `zh`) to filter by result language
- **Bulk query processing**: Submit multiple queries in one run — each produces a separate output item
- **Pagination**: Automatically fetches up to 10 pages to collect up to 100 organic results per query
- **Rank-tracker diff mode**: Cross-run SERP snapshots stored per `(query, country, language)` triple with rank-change detection — replaces dedicated SaaS rank trackers
- **Webhook alerts**: Fire-and-forget POST to Slack / Zapier / Make / n8n / custom endpoints on significant SERP movements (2+ position shifts, new/dropped top-10 domains, featured-snippet flips)
- **AI SEO analysis**: Search intent classification, keyword insights, content gap analysis, and content structure recommendations via 5 LLM providers
- **Multi-LLM support**: OpenRouter (300+ models, recommended), Anthropic Claude, Google AI Gemini, OpenAI GPT, or self-hosted Ollama
- **Pay-per-query pricing**: Only billed for queries that return at least one organic result — blocked runs cost nothing

### Head-to-head: vs `apify/google-search-scraper`

This Actor competes directly against Apify's official `apify/google-search-scraper` (~116K users, 4.8/5, 125 reviews). Where each wins:

| Feature | This Actor (harvestlab) | apify/google-search-scraper |
| --- | --- | --- |
| **Cross-run rank-change tracker** | ✅ diff mode with per-(query, country, language) snapshots | ❌ not offered |
| **SERP-change webhook alerts** | ✅ Slack / Zapier / n8n / Make / custom on rank shifts | ❌ not offered |
| **AI SEO analysis** | ✅ 5-provider router (OpenRouter / Anthropic / Google / OpenAI / Ollama) for intent / gaps / content recommendations | ❌ has "AI search mode" (Perplexity / ChatGPT search) — different feature, not SEO |
| **Featured snippet + knowledge panel extraction** | ✅ both | ⚠️ PAA documented; featured snippets and knowledge panels not explicit |
| **People Also Ask** | ✅ full | ✅ full |
| **Multi-country** | ✅ 30+ countries | ✅ supported |
| **Per-result price** | $0.003 / query | $0.0018 / page |
| **Total users / rating** | New (volume gap) | 116K / 4.8/5 |

**Why pay 1.7× more per query?** Because `apify/google-search-scraper` is a SERP fetcher; this Actor is a **rank-tracker SaaS at SERP-scraper pricing**. A 50-keyword daily rank-tracker on Apify's actor costs ~$2.70/mo for raw SERPs that you'd then need to diff and alert on yourself. On this actor: ~$4.50/mo for diffed SERPs + Slack alerts already wired. Versus Ahrefs ($129/mo) or SEMrush ($139.95/mo): **30×+ cheaper** with the same rank-tracker outputs.

**If you only want raw SERP HTML and you'll do diffing/alerts yourself**, Apify's official actor is cheaper. **If you want a turnkey rank-tracker that fires Slack alerts when your competitor gains a featured snippet**, this Actor's $9/mo is the cheapest option on the Apify Store.

## Use Cases

Each use case below shows the SaaS tool you'd otherwise pay for and what this actor costs at the same workload — most replacements come in 10–100× cheaper.

- **Daily rank tracking** — Replace **Ahrefs Lite ($129/mo)**, **SEMrush Pro ($139.95/mo)**, **SE Ranking Essential ($65/mo)**, or **SERPRobot Pro ($49/mo)**. Schedule this actor daily for a 100-keyword list with `trackSerpDiff: true` and Slack alerts: ~$0.30/day = **~$9/month** vs $49–140/month subscriptions you don't fully use. The `serp_diff` item gives you the same rank-change feed those dashboards build their alerting on.
- **SERP competitor monitoring** — Replace the position-tracker tier of **Ahrefs ($129/mo)** or **SEMrush ($139.95/mo)**. Track which domains hold top-10 spots for your money keywords and POST to Slack/n8n when a competitor gains or drops 2+ positions. Real-time, push-based, no dashboard refresh required.
- **SEO keyword research at scale** — Replace **Ahrefs Keywords Explorer** or **SEMrush Keyword Magic Tool** for SERP-side research. 1,000 head-term SERPs (organic + PAA + related searches + featured snippet) cost **~$3** as one-shot data — you keep the raw JSON, not a locked dashboard. Pair with `enableAiAnalysis` for $0.05 per query of intent classification, content gaps, and recommended content structure (typical AI SEO tools like **Surfer SEO ($89/mo)** or **MarketMuse ($149/mo)** sell the same insight).
- **Content brief generation** — Pull PAA questions, related searches, and the current top-10 for a target keyword, then feed `enableAiAnalysis: true` to get a draft H2/H3 outline, target word count, and competitive gaps. Replaces the brief-builder feature of **Frase ($45/mo)**, **MarketMuse ($149/mo)**, or **Clearscope ($189/mo)** at $0.053/brief.
- **Featured-snippet & PAA opportunity hunting** — Filter datasets where `featured_snippet` is `null` to find queries where the answer box is unclaimed, or where it cites a weak source you can outrank. Replaces the "SERP features" report in **Ahrefs/SEMrush** at fractional cost.
- **Multi-market localized SEO** — Run the same query across `country: us / gb / de / fr / jp / br` and diff the SERPs to see which content travels and which markets need localized landing pages. Replaces the multi-location tier of **AccuRanker ($129/mo)** or **Wincher ($60/mo)**.
- **Affiliate & content-arbitrage discovery** — Score SERPs for commercial-intent queries where the top-10 are weak (low DR placeholders, AI-generated thin content). The `domain` and `snippet` fields plus AI competitive-gap analysis surface keywords where a real piece of content can rank — replaces parts of **Niche Hunter** workflows in **SEMrush** or **Ahrefs**.
- **Lead generation from local & niche queries** — Extract business URLs from `"plumber [city]"`, `"[industry] consultant near me"`, or B2B intent queries to feed into a CRM. SERP-based lead lists are an alternative to **ZoomInfo ($14,995/yr seat)** or **Apollo ($59/mo seat)** for top-of-funnel domain discovery.
- **Brand & reputation monitoring** — Schedule daily SERP runs for your brand name + variants and alert via webhook when a negative third-party page enters top-10 or your own domain drops. Replaces the brand-mention tier of **Brand24 ($99/mo)** or **Mention ($49/mo)** for SERP-specific monitoring.
- **AI SEO analysis without a separate tool** — `enableAiAnalysis` returns search intent, competition level, content recommendations, competitive gaps, and featured-snippet strategy in one pass for $0.05/query. Replaces buying both a rank tracker AND an AI SEO tool — typical stack is **Ahrefs ($129/mo) + Surfer SEO ($89/mo) = $218/mo**; this actor covers both surfaces.
- **Academic & data-journalism research** — SERP data for studies on information retrieval, search bias, media coverage patterns, election-cycle ranking, or localization effects. Pay only for the queries you actually run; no annual seat cost.

## Input

### Search Parameters

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `queries` | string[] | — | One or more search queries. Each produces a separate output item. |
| `maxResults` | integer | 10 | Max organic results per query (1–100). Values above 10 require pagination (+2–3s per extra page). |
| `country` | string | `us` | ISO 3166-1 alpha-2 country code for geo-targeted results (`us`, `gb`, `de`, `jp`, etc.). |
| `language` | string | `en` | ISO 639-1 language code for result language (`en`, `de`, `fr`, `es`, `ja`, etc.). |
| `includePeopleAlsoAsk` | boolean | `true` | Extract PAA questions. |
| `includeRelatedSearches` | boolean | `true` | Extract related search suggestions. |
| `includeFeaturedSnippet` | boolean | `true` | Extract the featured snippet / answer box. |
| `includeKnowledgePanel` | boolean | `true` | Extract knowledge panel entity data. |

### SERP Diff / Rank Tracker

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `trackSerpDiff` | boolean | `false` | Enable cross-run rank tracking. Snapshots top-10 URLs per `(query, country, language)` and emits a `serp_diff` item with rank changes on every subsequent run. |
| `alertWebhookUrl` | string | — | POST a JSON diff payload here when a tracked query's `change_score` meets the threshold. Auto-enables `trackSerpDiff`. |
| `alertMinChangeScore` | integer | 2 | Minimum change score to fire a webhook (1 = any change, higher = less noise). |

### AI Analysis Settings

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `enableAiAnalysis` | boolean | `false` | Enable AI-powered SEO analysis. Requires a provider API key. |
| `llmProvider` | string | `openrouter` | Provider: `openrouter` (recommended), `anthropic`, `google`, `openai`, or `ollama`. |
| `openrouterApiKey` | string | — | OpenRouter API key. Set via `OPENROUTER_API_KEY` env var or this field. |
| `anthropicApiKey` | string | — | Anthropic API key. Set via `ANTHROPIC_API_KEY` env var or this field. |
| `googleApiKey` | string | — | Google AI API key. Set via `GOOGLE_API_KEY` env var or this field. |
| `openaiApiKey` | string | — | OpenAI API key. Set via `OPENAI_API_KEY` env var or this field. |
| `ollamaBaseUrl` | string | `http://localhost:11434` | Ollama base URL for self-hosted inference. |
| `llmModel` | string | — | Optional model override. Leave empty for provider defaults. |

### Proxy Settings

| Field | Type | Description |
| --- | --- | --- |
| `proxyConfiguration` | object | Proxy settings. Residential proxy is strongly recommended for popular head-term queries to avoid Google CAPTCHA blocks. Default: Apify RESIDENTIAL group. |

Google aggressively throttles common commercial queries (`"best CRM software"`, `"ai news 2026"`, `"buy laptop"`) from datacenter IPs and returns a reCAPTCHA / "unusual traffic" page. The default proxy is `{"useApifyProxy": true, "apifyProxyGroups": ["RESIDENTIAL"]}` — each retry rotates to a fresh household IP. You are never charged for blocked queries.

**CLI aliases** (hidden from Console form): `query`, `searchQuery`, `searchTerm` all map to `queries[]`; `maxItems` maps to `maxResults`.

## Output

### Per-query result item

Each query produces one output item pushed to the Apify dataset:

| Field | Type | Description |
| --- | --- | --- |
| `query` | string | The search query used |
| `country` | string | Country code used |
| `language` | string | Language code used |
| `total_results_text` | string | Google's "About X results" count text |
| `organic_results_count` | integer | Number of organic results extracted |
| `organic_results` | object[] | Ranked list of organic results (see below) |
| `people_also_ask` | string[] | PAA questions (empty array if none or disabled) |
| `related_searches` | string[] | Related search suggestions (empty array if none or disabled) |
| `featured_snippet` | object|null | Featured snippet data if Google serves one |
| `knowledge_panel` | object|null | Knowledge panel entity data if present |
| `scraped_at` | string | ISO 8601 timestamp of the scrape |

### Organic result object

| Field | Type | Description |
| --- | --- | --- |
| `position` | integer | Rank position (1 = top) |
| `title` | string | Page title as shown in search results |
| `url` | string | Full canonical URL |
| `domain` | string | Root domain extracted from the URL (e.g. `forbes.com`) |
| `displayed_url` | string | URL as displayed by Google (breadcrumb format) |
| `snippet` | string | Description text under the title |

### SERP diff item (rank-tracker mode)

When `trackSerpDiff: true`, an additional `serp_diff` item is emitted per query:

| Field | Type | Description |
| --- | --- | --- |
| `type` | string | Always `"serp_diff"` |
| `query` | string | Search query |
| `change_score` | integer | Total significant changes detected |
| `rank_changes` | object[] | Domains that moved 2+ positions (`rank_now`, `rank_previous`, `rank_delta`, `direction`) |
| `new_entries` | object[] | Domains new to the tracked top-10 |
| `dropped_entries` | object[] | Domains that left the tracked top-10 |
| `featured_snippet_changed` | boolean | Whether the featured snippet source flipped |
| `snapshot_at` | string | ISO 8601 timestamp |
| `hours_since_last_snapshot` | number | Hours elapsed since the prior snapshot |

### AI analysis fields (when enabled)

When `enableAiAnalysis: true`, each query result includes an `ai_analysis` object with:

- `search_intent` — informational / commercial / transactional / navigational
- `competition_level` — low / medium / high
- `keyword_insights` — primary intent, long-tail opportunities, PAA-derived questions
- `content_recommendations` — optimized title, H2/H3 outline, target word count
- `competitive_gaps` — opportunities the current top-10 is missing
- `featured_snippet_strategy` — how to win the answer box for this query

### Pricing

| Event | When charged | Price |
| --- | --- | --- |
| `serp-result-scraped` | Once per query returning ≥1 organic result | **$0.003** |
| `ai-analysis-completed` | Per query when AI SEO analysis runs | **$0.05** |
| `serp-change-detected` | Per query with a significant SERP change vs prior run | **$0.002** |
| `alert-dispatched` | Per successfully delivered webhook alert (2xx response) | **$0.002** |

**vs. commercial alternatives**: SerpAPI charges $50/mo for 5,000 searches ($0.01/search); DataForSEO charges $1.50/1,000 SERPs; Bright Data SERP API costs $3/1,000 results. This actor uses pay-per-event — $0.005/SERP result page with no monthly minimum and Playwright-rendered JavaScript SERPs.

Blocked queries (CAPTCHA / 0 results) are **never charged**. A typical 50-keyword daily rank-tracker costs ~$0.15/day ($4.50/month) vs $50–200/month for SaaS alternatives.

#### Skyfire bulk bundle (AI-agent payment rail)

A `skyfire-bundle-1500-results` event ships at **$5.00 per 1,500 SERP results** for AI agents paying via the Skyfire JWT rail. Effective rate: **$0.00333/result** — a small (+11%) premium over the raw `serp-result-scraped` baseline ($0.003). Skyfire requires a $5 minimum charge per actor invocation; the $5 floor exceeds 1500 × $0.003 = $4.50 baseline by ~11%, and the bundle covers that floor for agent-payment-rail compatibility (Tavily/Serper alternative). Pay-as-you-go users via Apify's standard PPE rail still get the cheaper $0.003/result. Skyfire is opt-in for agents that need single-prepaid-call billing semantics.

### Output example

```
{
    "query": "best project management software 2026",
    "country": "us",
    "language": "en",
    "total_results_text": "About 142,000,000 results",
    "organic_results_count": 10,
    "organic_results": [
        {
            "position": 1,
            "title": "Best Project Management Software of 2026 - Forbes Advisor",
            "url": "https://www.forbes.com/advisor/business/best-project-management-software/",
            "domain": "forbes.com",
            "displayed_url": "forbes.com › advisor › business",
            "snippet": "We tested and compared 20+ project management tools..."
        }
    ],
    "people_also_ask": [
        "What is the best project management tool in 2026?",
        "Is Monday.com better than Asana?"
    ],
    "related_searches": [
        "project management software free",
        "best project management software for small teams"
    ],
    "featured_snippet": null,
    "knowledge_panel": null,
    "scraped_at": "2026-04-24T10:30:00Z"
}
```

## Quick Start

**Basic SERP scrape — 10 results for one keyword:**

```
{
    "queries": ["best CRM software 2026"],
    "maxResults": 10,
    "country": "us",
    "proxyConfiguration": {
        "useApifyProxy": true,
        "apifyProxyGroups": ["RESIDENTIAL"]
    }
}
```

Cost: $0.003. Returns title, URL, snippet, position for 10 results + PAA + related searches.

**Multiple queries in one run:**

```
{
    "queries": [
        "best project management software",
        "asana vs monday vs clickup",
        "project management tools for agencies"
    ],
    "maxResults": 20,
    "country": "us"
}
```

**Localized search (Germany):**

```
{
    "queries": ["beste Projektmanagement Software"],
    "maxResults": 10,
    "country": "de",
    "language": "de"
}
```

**Daily rank tracker with Slack alerts:**

```
{
    "queries": ["my target keyword", "competitor keyword"],
    "trackSerpDiff": true,
    "alertWebhookUrl": "https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK",
    "alertMinChangeScore": 2,
    "country": "us"
}
```

Schedule this run daily via Apify Scheduler. The first run establishes the baseline (not charged). Subsequent runs emit a `serp_diff` item and POST to Slack whenever positions shift 2+ places, new domains enter top 10, or a featured snippet flips. Cost: ~$7–10/month for 50 tracked keywords vs $50–200/month for SaaS rank trackers.

**With AI SEO analysis:**

```
{
    "queries": ["email marketing platforms"],
    "maxResults": 30,
    "enableAiAnalysis": true,
    "llmProvider": "openrouter",
    "openrouterApiKey": "sk-or-..."
}
```

Returns full SERP data plus `ai_analysis` with intent classification, content gaps, keyword opportunities, and a recommended content structure.

### MCP Quickstart — call this actor from Claude / Cursor / ChatGPT

Open Apify's hosted MCP configurator at [mcp.apify.com](https://mcp.apify.com), or install the Apify MCP server in your AI agent of choice:

```
# Claude Code
claude mcp add apify -- npx -y @apify/actors-mcp-server --token YOUR_APIFY_TOKEN

# Claude Desktop / Cursor (add to mcp.json):
{"mcpServers":{"apify":{"command":"npx","args":["-y","@apify/actors-mcp-server","--token","YOUR_APIFY_TOKEN"]}}}
```

Then prompt the agent:

> "Use the harvestlab/google-search-scraper actor on Apify to track our Google rankings for ['ai code editor', 'ai pair programming'] across US/UK/DE/FR with featured snippet detection — fire a webhook on rank changes. Push the results back as JSON."

Through Apify MCP, the agent will discover the actor's `dataset_schema.json`, generate the right input, run it, and pipe the typed output back into your conversation.

## Use with AI agents

This actor's output (organic SERP results, featured snippets, PAA, knowledge panels, plus optional AI SEO analysis as JSON) is agent-ready — drop it into a research agent, RAG pipeline, or SEO auditor as a tool. It's a direct **Tavily / Serper / Brave Search API alternative** at $0.003/query, and the same run replaces Ahrefs ($129/mo) for rank tracking. Reference templates live at [apify/actor-templates → js-langchain](https://github.com/apify/actor-templates/tree/master/templates/js-langchain) and [js-langgraph-agent](https://github.com/apify/actor-templates/tree/master/templates/js-langgraph-agent).

**LangChain — wrap as a `Tool` for a research / RAG agent:**

```
from apify_client import ApifyClient
from langchain.tools import Tool

client = ApifyClient("YOUR_APIFY_TOKEN")

def google_search(args: dict) -> list:
    run = client.actor("harvestlab/google-search-scraper").call(run_input={
        "queries": args["queries"],
        "country": args.get("country", "us"),
        "maxResults": args.get("maxResults", 10),
    })
    return list(client.dataset(run["defaultDatasetId"]).iterate_items())

google_search_tool = Tool(
    name="google_search",
    description="Fresh Google SERP results + featured snippets. Use for research, RAG context, or SEO checks.",
    func=google_search,
)
# agent.invoke({"input": "..."}) -> agent calls google_search_tool.invoke({"queries": ["ai code editors 2026"], "country": "us"})
```

**LangGraph — node in a research-agent graph:**

```
from apify_client import ApifyClient
from langgraph.graph import StateGraph

client = ApifyClient("YOUR_APIFY_TOKEN")

def serp_node(state: dict) -> dict:
    run = client.actor("harvestlab/google-search-scraper").call(run_input={
        "queries": [state["query"]],
        "country": state.get("country", "us"),
        "maxResults": 10,
    })
    item = next(client.dataset(run["defaultDatasetId"]).iterate_items())
    return {
        "urls": [r["url"] for r in item["organic_results"]],
        "snippets": [r["snippet"] for r in item["organic_results"]],
        "featured_answer": (item.get("featured_snippet") or {}).get("snippet"),
    }

graph = StateGraph(dict)
graph.add_node("search", serp_node)
# wire into your research / synthesis / writer nodes downstream
```

## Troubleshooting

**CAPTCHA or "unusual traffic" block**
Google blocks datacenter IPs on popular commercial queries. Always use `proxyConfiguration` with `RESIDENTIAL` proxy group. You are never charged for blocked queries — only successful scrapes bill. If blocks persist on a specific query, rephrase as a long-tail variant (`"ai news april 2026"` instead of `"ai news 2026"`) or reduce run frequency.

**Fewer results than expected (e.g. 7 instead of 10)**
Google serves fewer organic results for navigational queries, rare queries, or when it fills space with Knowledge Panels, Shopping carousels, or AI Overviews. Try broadening the search term or removing quotes. AI Mode / AI Overview layouts (new generative-answer pages rolling out for commercial-intent queries) may return fewer `<h3>` headings — rephrase as a long-tail or informational query to get the traditional organic SERP.

**Featured snippet not appearing in output**
Not all queries trigger featured snippets. The `featured_snippet` field is `null` when Google doesn't serve one. Question-format queries (`"how to X"`, `"what is Y"`) are more likely to trigger answer boxes.

**Results differ from my browser**
Google personalizes results based on cookies, location, and search history. This actor uses a clean browser session with no cookies. Set `country` and `language` to match your target audience. Results for the same query may also vary by Google data center.

**Rank tracking shows no diff on second run**
Confirm `trackSerpDiff: true` in both runs and that `country`/`language` match exactly between runs — the snapshot key is `(query, country, language)`. Also verify the actor has permission to write to the `google-serp-rank-history` named key-value store.

**AI analysis returns an error**
Confirm the API key is set either as an environment variable (`OPENROUTER_API_KEY`, `ANTHROPIC_API_KEY`, `GOOGLE_API_KEY`, or `OPENAI_API_KEY`) or in the corresponding input field. The error message names both paths. For Ollama, confirm `ollamaBaseUrl` is reachable from the Apify runner (self-hosted Ollama is not reachable from cloud runs unless tunneled).

**Webhook not firing**
`alertWebhookUrl` requires `trackSerpDiff: true` (auto-enabled when a URL is set) and a prior baseline run. Webhooks fire only when `change_score >= alertMinChangeScore` (default 2). Check the actor log for `[alert]` lines — they show whether the webhook was dispatched, skipped (below threshold), or failed (timeout / non-2xx). Failed webhooks never fail the run and are not charged.

## Legal and Compliance

This actor is provided as a technical tool. Users are solely responsible for ensuring their use complies with applicable laws and platform policies, including:

- **Google's Terms of Service** — Google's ToS restricts automated access to their services. Use this tool responsibly and at your own risk.
- **Data protection regulations** — GDPR (EU), CCPA (California), and equivalent local laws govern the collection and processing of any personal data encountered in search results.
- **Local laws** — Web scraping legality varies by jurisdiction. Users must verify compliance with applicable laws in their region and the region of the target data.

This actor extracts only publicly visible search result information — titles, URLs, and snippet text that Google displays to any anonymous visitor. It does not bypass authentication, access private data, or circumvent paywalls.

The developer of this actor is not responsible for any misuse, ToS violations, or legal consequences arising from its use. No personal data is stored by the actor beyond what Apify's standard dataset storage provides; consult the [Apify Privacy Policy](https://apify.com/privacy-policy) for data handling details.

### Related Actors

- **[News Monitor](https://apify.com/harvestlab/news-monitor)** — Track news mentions on the same competitor brand keywords you scrape SERPs for. Pair Google's organic / featured-snippet picture with real-time press coverage so SEO and PR teams see search rank shifts and the news cycles driving them in one workflow.
- **[Contact Extractor](https://apify.com/harvestlab/contact-extractor)** — Turn top-ranking SERP URLs into a lead list. Pipe `result.url` from Google Search Scraper into Contact Extractor to harvest SMTP-verified emails, phones, and tech-stack signals from every domain that ranks for your target query — link-building outreach and ICP discovery in one pass.
- **[Reddit Scraper](https://apify.com/harvestlab/reddit-scraper)** — Layer community sentiment onto SERP keyword research. Run the same competitor and category terms through Reddit to see how real users discuss them, surfacing pain points and feature gaps that pure SERP data hides.