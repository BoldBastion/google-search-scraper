[Google Search Scraper](https://apify.com/nexgendata/google-search-scraper?fpr=data)

# Google Search Scraper - SERP Results & Rich Snippets

Extract structured data from Google search results with precision. This actor leverages Apify's Google SERP proxy to scrape real Google search engine results pages (SERPs) and return organic results, featured snippets, ads, and "People Also Ask" questions in clean JSON format.

## Features

- **Organic Search Results**: Title, URL, description, position, domain
- **Featured Snippets**: Automatically detects and extracts knowledge panels and featured snippets
- **People Also Ask**: Captures related questions and answers
- **Multi-Country Support**: US, UK, Canada, Australia, Germany, France, Japan, Brazil, India, Mexico
- **Language Support**: Search results in multiple languages
- **Bulk Operations**: Process multiple queries in a single run
- **Structured Output**: Clean JSON data ready for analysis or integration

## Use Cases

- **SEO Research**: Analyze competitor rankings and SERP features
- **Market Intelligence**: Track keyword rankings and featured snippet opportunities
- **Competitive Analysis**: Monitor search results for market trends
- **Content Strategy**: Identify question-based content opportunities from "People Also Ask"
- **Lead Generation**: Extract business websites from search results
- **Academic Research**: Gather search trend data at scale

## Input

The actor accepts an input object with the following structure:

```
{
  "queries": ["python programming", "web scraping"],
  "maxResults": 10,
  "country": "US",
  "language": "en"
}
```

### Input Parameters

- **queries** (required): Array of search queries (e.g., ["machine learning", "AI trends"])
- **maxResults**: Number of results per query (1-100, default: 10)
- **country**: Search country (US, UK, CA, AU, DE, FR, JP, BR, IN, MX) - default: US
- **language**: Result language code (en, de, fr, es, ja, pt) - default: en

## Output

Results are pushed to the default dataset as individual JSON records:

```
{
  "query": "python programming",
  "position": 1,
  "type": "organic_result",
  "title": "Welcome to Python.org",
  "url": "https://www.python.org",
  "domain": "python.org",
  "description": "The official home of the Python Programming Language."
}
```

### Result Types

- **organic_result**: Standard search result (title, URL, description, position)
- **featured_snippet**: Featured snippet from knowledge panel
- **people_also_ask**: Related questions from "People Also Ask" section
- **ad**: Sponsored search result (if detected)
- **error**: Error record if query fails

## Pricing

This actor uses **Pay-Per-Event (PPE) pricing**:

- **$0.005** per actor start
- **$0.003** per result extracted

For 10 queries with 10 results each: $0.005 + (100 × $0.003) = $0.305

## Technology Stack

- **Python 3.12** with asyncio
- **BeautifulSoup4**: HTML parsing
- **httpx**: Async HTTP client
- **Apify SDK**: Job scheduling and data export
- **Apify Google SERP Proxy**: Real Google HTML via proxy network

## Rate Limiting

Google enforces rate limits on search requests. The actor handles rate limiting gracefully:

- Automatic retry on 429 (Too Many Requests)
- Exponential backoff
- Proxy rotation via Apify's residential proxy network

## Limitations

- Google actively blocks automated scraping; this actor uses Apify's official proxy to mitigate blocks
- Search results vary by location, IP, and query history
- Some specialized results (maps, images, videos) may not be captured in the organic results list
- Requires active Apify account with proxy access

## Examples

### 1. Basic SEO Research

```
{
  "queries": ["best coffee shops near me", "coffee trends 2026"],
  "maxResults": 20,
  "country": "US"
}
```

### 2. Multi-Language Analysis

```
{
  "queries": ["machine learning"],
  "maxResults": 15,
  "country": "DE",
  "language": "de"
}
```

### 3. Competitive Intelligence

```
{
  "queries": ["our company name", "competitor name", "industry keyword"],
  "maxResults": 30
}
```

## Support

For issues or feature requests, please contact support or check the Apify marketplace documentation.

## License

Proprietary - NexGenData

## Related tools

- [AI Sentiment Analyzer — Brand & Review Mining](https://apify.com/nexgendata/ai-sentiment-analyzer?fpr=2ayu9b)
- [SSL Certificate Checker — Bulk Expiry Monitor](https://apify.com/nexgendata/ssl-certificate-checker?fpr=2ayu9b)
- [SaaS Pricing Tracker — Monitor Competitor Pricing Changes](https://apify.com/nexgendata/saas-pricing-tracker?fpr=2ayu9b)
- [Enterprise MCP Gateway — 26 AI Servers in One](https://apify.com/nexgendata/enterprise-mcp-gateway?fpr=2ayu9b)

 

## 💻 Code Example — Python

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")
run = client.actor("nexgendata/google-search-scraper").call(run_input={
    # Fill in the input shape from the actor's input_schema
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

## 🌐 Code Example — cURL

```
curl -X POST "https://api.apify.com/v2/acts/nexgendata~google-search-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{ /* input schema */ }'
```

## ❓ FAQ

**Q: How do I get started?**
Sign up at [apify.com](https://www.apify.com/?fpr=2ayu9b), grab your API token from Settings → Integrations, and run the actor via the Apify console, API, Python SDK, or any integration (Zapier, Make.com, n8n).

**Q: What's the typical cost per run?**
See the pricing section below. Most runs finish under $0.10 for typical batches.

**Q: Is this actor maintained?**
Yes. NexGenData maintains 165+ Apify actors and ships updates regularly. Bug reports via the Apify console issues tab get responses within 24 hours.

**Q: Can I use the output commercially?**
Yes — you own the output data. Check the target site's Terms of Service for any usage restrictions on the scraped content itself.

**Q: How do I handle rate limits?**
Apify manages concurrency and retries automatically. For very large batches (10K+ items), run multiple smaller jobs in parallel instead of one mega-job for better reliability.

## 💰 Pricing

Pay-per-event pricing — you only pay for what you actually extract.

- **Actor Start:** $0.0001
- **result:** $0.0050

## 🔗 Related NexGenData Actors

- [Hacker News Scraper](https://apify.com/nexgendata/hacker-news-scraper?fpr=2ayu9b)
- [Google Maps Lead Scraper](https://apify.com/nexgendata/google-maps-scraper?fpr=2ayu9b)
- [SEC EDGAR Scraper](https://apify.com/nexgendata/sec-edgar-scraper?fpr=2ayu9b)

## 🚀 Apify Affiliate Program

New to Apify? Sign up with our [referral link](https://www.apify.com/?fpr=2ayu9b) — you get free platform credits on signup, and you help fund the maintenance of this actor fleet.

## 📚 More From NexGenData

Explore the full catalog, tutorials, Gumroad data packs, and newsletter at **[thenextgennexus.com](https://thenextgennexus.com)** — the brand home for everything we ship.

- 📖 Tutorials & how-to guides
- 🗂️ Full actor catalog with usage examples
- 📦 Gumroad data packs (one-time purchases)
- 📬 Newsletter — monthly drops of new actors and revenue experiments

---

*Built and maintained by [NexGenData](https://apify.com/nexgendata?fpr=2ayu9b) — 165+ actors covering scraping, enrichment, MCP servers, and automation.*
🏠 Home: [thenextgennexus.com](https://thenextgennexus.com)