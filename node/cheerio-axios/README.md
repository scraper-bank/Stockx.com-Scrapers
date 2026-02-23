# Stockx Scrapers - Cheerio (Node.js)

Efficiently extract product pricing, market trends, and search results from Stockx using these high-performance Node.js scrapers built with Cheerio and Axios. This suite provides a lightweight, reliable solution for gathering real-time sneaker and streetwear data while navigating the complexities of modern e-commerce web structures.

## Overview

This directory contains Node.js scrapers built with **Cheerio**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why Cheerio?

Cheerio, combined with Axios, represents the gold standard for high-performance data extraction in the Node.js ecosystem. Unlike browser-based tools like Puppeteer or Playwright, Cheerio parses markup and provides an API for traversing/manipulating the resulting data structure without the overhead of a full browser engine.

**Key Features and Capabilities:**
- **Lightning Speed:** By avoiding the rendering of CSS, images, and JavaScript execution, Cheerio scrapers are significantly faster and consume 90% less RAM/CPU than headless browsers.
- **jQuery Syntax:** It implements a subset of core jQuery, making it incredibly intuitive for developers to select elements and extract text or attributes.
- **Efficiency:** Ideal for scraping static HTML or pages where the data is embedded in the initial response (like JSON-LD or pre-rendered HTML).
- **Scalability:** Due to its low resource footprint, you can run hundreds of concurrent requests on modest hardware.

**When to use this stack:** Use Cheerio when performance and cost-efficiency are your primary goals. It is the best choice for large-scale scraping of Stockx pages where the data is available in the source code, as it allows for much higher throughput than browser automation.

## Prerequisites

- **Node.js**: Node.js 14 or higher
- **npm**: npm
- **ScrapeOps API Key**: For anti-bot protection (free tier available)

## Installation

1. Navigate to the specific scraper directory:
```bash
cd product_category  # or product_data, product_search
```

2. Install dependencies:
```bash
npm install cheerio axios
```

3. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

4. Update the API key in the scraper file:
```node
API_KEY = 'YOUR-API-KEY'
```

## Anti-Bot Protection

All scrapers can integrate with **ScrapeOps** to help handle Stockx's anti-bot measures:
- Proxy rotation (may help reduce IP blocking)
- Request header optimization (can help reduce detection)
- Rate limiting management

**Note**: Anti-bot measures vary by site and may change over time. CAPTCHA challenges may occur and cannot be guaranteed to be resolved automatically. Using proxies and browser automation can help reduce blocking, but effectiveness depends on the target site's specific anti-bot measures.

**Free Tier Available**: ScrapeOps offers a generous free tier perfect for testing and small-scale scraping.

## Output Format

All scrapers output data in **JSONL format** (one JSON object per line):
- Each line represents one product/result
- Efficient for large datasets
- Easy to process line-by-line
- Can be imported into databases or data processing tools

Example output files:
- `stockx_com_product_category_page_scraper_data_20260114_120000.jsonl`
- `stockx_com_product_page_scraper_data_20260114_120000.jsonl`
- `stockx_com_product_search_page_scraper_data_20260114_120000.jsonl`

## Alternative Implementations

This repository provides multiple implementations for different use cases:

### Node.js Alternatives
- [Puppeteer (Node.js)](../puppeteer/README.md)
- [Playwright (Node.js)](../playwright/README.md)

### Python Alternatives
- [Selenium (Python)](../../python/selenium/README.md)
- [BeautifulSoup (Python)](../../python/BeautifulSoup/README.md)
- [Playwright (Python)](../../python/playwright/README.md)

## Project Structure

```
cheerio-axios/
- product_category/
  - example-data/
    - product_category.json
  - README.md
  - scraper/
    - stockx_scraper_product_category_v1.js
- product_data/
  - example-data/
    - product_data.json
  - README.md
  - scraper/
    - stockx_scraper_product_data_v1.js
- product_search/
  - example-data/
    - product_search.json
  - README.md
  - scraper/
    - stockx_scraper_product_search_v1.js
```

## Best Practices

1. **Respect Rate Limits**: Use appropriate delays and concurrency settings
2. **Monitor ScrapeOps Usage**: Track your API usage in the ScrapeOps dashboard
3. **Handle Errors Gracefully**: Implement proper error handling and logging
4. **Validate URLs**: Ensure URLs are valid Stockx pages before scraping
5. **Update Selectors**: Stockx may change HTML structure; update selectors as needed
6. **Test Regularly**: Test scrapers regularly to catch breaking changes early
7. **Handle Missing Data**: Some products may not have all fields; handle null values appropriately

## Support & Resources

- **ScrapeOps Documentation**: [https://scrapeops.io/docs/intro/](https://scrapeops.io/docs/intro/)
- **Cheerio Documentation**: https://cheerio.js.org/
- **Example Outputs**: See `example/` folders in each scraper directory

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using these scrapers.