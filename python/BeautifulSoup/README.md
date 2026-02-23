# Stockx Scrapers - BeautifulSoup (Python)

Effortlessly extract sneaker prices, market data, and product listings from Stockx using these high-performance Python scrapers built with BeautifulSoup. Designed for reliability and speed, these tools help you monitor the secondary market and track product availability with ease.

## Overview

This directory contains Python scrapers built with **BeautifulSoup**.

## Available Scrapers

- [Product Category](./product_category/README.md)
- [Product Data](./product_data/README.md)
- [Product Search](./product_search/README.md)

## Why BeautifulSoup?

BeautifulSoup is the industry standard for parsing HTML and XML documents in Python, offering a perfect balance between ease of use and processing speed. When combined with a robust HTTP client, it provides a lightweight alternative to resource-heavy browser automation tools.

**Key Features and Capabilities:**
- **Exceptional Performance**: Unlike Selenium or Playwright, BeautifulSoup does not require a headless browser, resulting in significantly lower CPU and RAM usage.
- **Robust Parsing**: It handles "messy" or poorly formatted HTML gracefully, ensuring data extraction continues even if the page source is slightly malformed.
- **Intuitive API**: Uses Pythonic idioms for navigating, searching, and modifying the parse tree, which speeds up development and maintenance.
- **When to Use**: This stack is ideal for high-volume scraping tasks where speed is a priority and the target pages do not rely heavily on JavaScript for content rendering.
- **Framework Advantages**: It is highly portable and has minimal dependencies, making it easy to deploy in serverless environments or containerized microservices.

## Prerequisites

- **Python**: Python 3.7 or higher
- **pip**: pip
- **ScrapeOps API Key**: For anti-bot protection (free tier available)

## Installation

1. Navigate to the specific scraper directory:
```bash
cd product_category  # or product_data, product_search
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

4. Update the API key in the scraper file:
```python
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

### Python Alternatives
- [Selenium (Python)](../selenium/README.md)
- [Playwright (Python)](../playwright/README.md)

### Node.js Alternatives
- [Puppeteer (Node.js)](../../node/puppeteer/README.md)
- [Playwright (Node.js)](../../node/playwright/README.md)
- [Cheerio (Node.js)](../../node/cheerio-axios/README.md)

## Project Structure

```
BeautifulSoup/
- product_category/
  - example-data/
    - product_category.json
  - README.md
  - scraper/
    - stockx_scraper_product_category_v1.py
- product_data/
  - example-data/
    - product_data.json
  - README.md
  - scraper/
    - stockx_scraper_product_data_v1.py
- product_search/
  - example-data/
    - product_search.json
  - README.md
  - scraper/
    - stockx_scraper_product_search_v1.py
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
- **BeautifulSoup Documentation**: https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- **Example Outputs**: See `example/` folders in each scraper directory

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using these scrapers.