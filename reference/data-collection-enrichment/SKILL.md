---
name: data-collection-enrichment
description: Collect and enrich data from web sources using Python. Covers HAR file extraction, BeautifulSoup HTML parsing, Selenium browser automation, Scrapy frameworks, and API integration. Use when building data pipelines that ingest web data, alternative data, or public datasets for ML models.
---
# Data Collection & Enrichment
## When to Use This Skill
Use when you need to acquire data from web sources, APIs, or public datasets for ML pipelines. Especially for alternative data that doesn't come in clean CSVs.

## Instructions
1. **Strategy Selection**
   - Public API available? → Use requests + API key
   - Structured data visible in browser? → HAR file extraction
   - Dynamic/JS-rendered pages? → Selenium + BeautifulSoup
   - Large-scale multi-page collection? → Scrapy framework
   - Last resort → Paid services (Apify, Octoparse)
2. **HAR File Method** — Open browser DevTools → Network tab → browse target → Export HAR → parse JSON for API responses
3. **BeautifulSoup + Selenium** — Selenium drives browser, waits for JS render, passes page_source to BeautifulSoup for parsing
4. **Scrapy** — Define Spider class with start_urls, parse() method, Item pipelines. Best for large-scale structured collection.
5. **Data Cleaning Post-Collection** — Deduplicate by unique ID, handle missing fields, validate types, timestamp each record
6. **Enrichment** — Join collected data with existing datasets, compute derived features, add sentiment scores or geocoding
7. **Storage** — Save to CSV/Parquet for analysis, or insert into PostgreSQL/Snowflake for production pipelines
8. **Ethics & Rate Limiting** — Respect robots.txt, add delays between requests, use User-Agent headers, cache responses to avoid re-fetching
