# Stockx Scrapers - Puppeteer (Node.js)

Efficiently extract product pricing, search results, and category data from Stockx using these Node.js scrapers built with Puppeteer. This suite of tools leverages headless browser automation to navigate Stockx's dynamic interface and retrieve real-time market data.

## Overview

This directory contains Node.js scrapers built with **Puppeteer**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why Puppeteer?

Puppeteer is a powerful Node.js library that provides a high-level API to control headless Chrome or Chromium. It is particularly well-suited for scraping Stockx due to the following advantages:

- **Full JavaScript Rendering**: Stockx relies heavily on dynamic content loading. Puppeteer ensures that all React components and data-driven elements are fully rendered before data extraction begins.
- **Browser Automation**: Puppeteer can mimic human behavior, such as scrolling to trigger lazy-loading or clicking elements to reveal hidden data, which is often required for modern e-commerce sites.
- **Performance & Control**: Compared to other automation tools, Puppeteer offers fine-grained control over the browser instance, allowing you to disable images or CSS to speed up scraping while maintaining the ability to execute complex scripts.
- **Chrome DevTools Protocol**: Built by the Google Chrome team, it offers the most stable and feature-rich integration with the Chromium engine, ensuring high reliability for long-running scraping tasks.
- **When to use this stack**: Choose Puppeteer over Cheerio when the data you need is injected via JavaScript after the initial page load, and over Playwright when you prefer a more specialized, Chrome-centric ecosystem with extensive community support in the Node.js environment.

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
npm install puppeteer cheerio
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
- [Playwright (Node.js)](../playwright/README.md)
- [Cheerio (Node.js)](../cheerio-axios/README.md)

### Python Alternatives
- [Selenium (Python)](../../python/selenium/README.md)
- [BeautifulSoup (Python)](../../python/BeautifulSoup/README.md)
- [Playwright (Python)](../../python/playwright/README.md)

## Project Structure

```
puppeteer/
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
- **Puppeteer Documentation**: https://pptr.dev/
- **Example Outputs**: See `example/` folders in each scraper directory

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using these scrapers.