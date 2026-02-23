# Stockx Scrapers - Playwright (Node.js)

Effortlessly extract real-time market data from Stockx using these high-performance Node.js scrapers powered by Playwright. This suite provides reliable tools to capture pricing, product details, and search results from the world's leading sneaker and apparel marketplace.

## Overview

This directory contains Node.js scrapers built with **Playwright**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why Playwright?

Playwright is a modern, powerful automation library that offers several distinct advantages for scraping highly dynamic sites like Stockx:

- **Browser-Level Interaction**: Unlike simple HTTP clients, Playwright operates a real browser instance (Chromium, Firefox, or WebKit), allowing it to execute JavaScript and handle complex single-page applications (SPAs) seamlessly.
- **Auto-Wait Functionality**: Playwright automatically waits for elements to be actionable before performing tasks, significantly reducing flakiness caused by slow-loading network requests or animations.
- **Superior Performance**: Playwright's architecture uses a single-connection WebSocket to control the browser, making it faster and more resource-efficient than older tools like Selenium.
- **Stealth Capabilities**: It provides advanced features to emulate real user behavior, such as realistic mouse movements, keyboard inputs, and screen resolutions, which are essential for navigating modern bot detection systems.
- **Context Isolation**: Easily manage multiple browser contexts with isolated cookies and storage, perfect for running parallel scraping tasks without cross-contamination.

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
npm install playwright cheerio
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
- [Cheerio (Node.js)](../cheerio-axios/README.md)

### Python Alternatives
- [Selenium (Python)](../../python/selenium/README.md)
- [BeautifulSoup (Python)](../../python/BeautifulSoup/README.md)
- [Playwright (Python)](../../python/playwright/README.md)

## Project Structure

```
playwright/
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
- **Playwright Documentation**: https://playwright.dev/
- **Example Outputs**: See `example/` folders in each scraper directory

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using these scrapers.