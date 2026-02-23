# Stockx Product Search Scraper - Playwright (Python)

This powerful Python scraper is designed to extract comprehensive search results from StockX using the Playwright framework. It efficiently captures product listings, pricing, and metadata while utilizing ScrapeOps residential proxies to navigate anti-bot protections and ensure reliable data collection at scale.

## Table of Contents

- [What This Scraper Extracts](#what-this-scraper-extracts)
- [Quick Start](#quick-start)
- [Supported URLs](#supported-urls)
- [Configuration](#configuration)
- [Output Schema](#output-schema)
- [Anti-Bot Protection](#anti-bot-protection)
- [How It Works](#how-it-works)
- [Error Handling & Troubleshooting](#error-handling--troubleshooting)
- [Alternative Implementations](#alternative-implementations)

## What This Scraper Extracts

- **breadcrumbs (array)**: Navigation path leading to the current search result page.
- **pagination (object)**: Details about the current page, total pages, and links to next/previous pages.
- **products (array)**: List of items found, including names, prices, currencies, and image URLs.
- **recommendations (object)**: Suggested products or categories based on the search query.
- **relatedSearches (array)**: A list of string keywords related to the current search.
- **searchMetadata (object)**: Internal metadata regarding the search execution and result count.
- **sponsoredProducts (array)**: List of promoted items appearing within the search results.

## Quick Start

### Prerequisites

- Python 3.7 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
pip install playwright beautifulsoup4
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```python
API_KEY = "YOUR-API-KEY"
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd python/playwright/product_search
```

2. Edit the URLs in `scraper/stockx_scraper_product_search_v1.py`:

```python
# In stockx_scraper_product_search_v1.py
url = "https://example.com/product"
```

3. Run the scraper:

```bash
python stockx_scraper_product_search_v1.py
```

The scraper will generate a timestamped JSONL file (e.g., `Stockx_com_product_search_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_search.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://stockx.com/search?s=[QUERY]`
- `https://stockx.com/[CATEGORY]`
- `https://stockx.com` (General landing pages)

**Example URL:** `https://stockx.com/search?s=kids+shoes`

## Configuration

### Scraper Parameters

The scraper supports several configuration options. See the scraper code for available parameters.

### ScrapeOps Configuration

The scraper can use ScrapeOps for anti-bot protection and request optimization:

```python
# ScrapeOps proxy configuration
proxy_url = f"https://proxy.scrapeops.io/v1/?api_key={API_KEY}"
```

**ScrapeOps Features:**
- Proxy rotation (may help reduce IP blocking)
- Request header optimization (can help reduce detection)
- Rate limiting management
- Note: CAPTCHA challenges may occur depending on site behavior and cannot be guaranteed to be resolved automatically

## Output Schema

The scraper outputs data in JSONL format (one JSON object per line). Each object contains:

| Field | Type | Description |
| :--- | :--- | :--- |
| `breadcrumbs` | array | Array of objects containing name and url |
| `pagination` | object | Object containing currentPage, totalPages, and navigation URLs |
| `products` | array | Array of objects containing product details (price, name, url, etc.) |
| `recommendations` | object | Object containing suggested items |
| `relatedSearches` | array | List of related search terms (strings) |
| `searchMetadata` | object | Object containing search-specific metadata |
| `sponsoredProducts` | array | Array of objects for paid/promoted listings |

### Field Descriptions

- **breadcrumbs** (array): Contains a list of object items representing the site hierarchy.
- **pagination** (object): Contains nested data such as `currentPage`, `hasNextPage`, and `totalPages`.
- **products** (array): Contains a list of object items including brand, price, currency, and availability.
- **recommendations** (object): Contains nested data for suggested products.
- **relatedSearches** (array): Contains a list of items suggested by the site to refine the search.
- **searchMetadata** (object): Contains nested data regarding the query and result stats.
- **sponsoredProducts** (array): Contains a list of object items that are promoted in results.

### Field Details
- **Products Array**: Each item includes an `images` array with `url` and `altText`, and a `seller` object containing the name and URL.
- **Pagination**: Includes `nextPageUrl` and `previousPageUrl` which are absolute URLs for easy crawling.

## Anti-Bot Protection

This scraper can integrate with ScrapeOps to help handle Stockx's anti-bot measures:

### Why ScrapeOps?

Stockx may employ various anti-scraping measures including:
- Rate limiting and IP blocking
- Browser fingerprinting
- CAPTCHA challenges (may occur depending on site behavior)
- JavaScript rendering requirements
- Request pattern analysis

### ScrapeOps Integration

The scraper can use ScrapeOps proxy service which may provide:

1. **Proxy Rotation**: May help distribute requests across multiple IP addresses
2. **Request Optimization**: May optimize headers and request patterns to reduce detection
3. **Retry Logic**: Built-in retry mechanism with exponential backoff

**Note**: Anti-bot measures vary by site and may change over time. CAPTCHA challenges may occur and cannot be guaranteed to be resolved automatically. Using proxies and browser automation can help reduce blocking, but effectiveness depends on the target site's specific anti-bot measures.

### Getting Started with ScrapeOps

1. Sign up for a free account at [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)
2. Get your API key from the dashboard
3. Replace `YOUR-API-KEY` in the scraper code
4. The scraper can use ScrapeOps for requests (if configured)

**Free Tier**: ScrapeOps offers a generous free tier perfect for testing and small-scale scraping.

## How It Works

The scraper uses **Playwright** for browser automation, allowing it to render JavaScript-heavy content that standard HTTP clients might miss. 
1. **Navigation**: It launches a headless browser and navigates to the StockX search URL.
2. **Stealth**: It utilizes `playwright_stealth` to mask automated browser signals.
3. **Extraction**: Once the page is loaded, it passes the HTML content to **BeautifulSoup** for structured parsing of product grids, prices, and metadata.
4. **Data Processing**: Relative URLs are converted to absolute URLs, and data is validated via a `DataPipeline` class to prevent duplicates.
5. **Storage**: Extracted data is appended line-by-line into a JSONL file for memory efficiency.

## Error Handling & Troubleshooting

- **Empty Results**: StockX often updates its CSS selectors. If the `products` array is empty, check if the selectors in the BeautifulSoup parsing logic need updating.
- **Timeout Errors**: Playwright might timeout if the page takes too long to load. Increase the `timeout` parameter in `page.goto()`.
- **Proxy Issues**: Ensure your ScrapeOps API key is valid and you have remaining credits if results are being blocked.

### Debugging

Enable detailed logging:

```python
# Enable debug logging
import logging
logging.basicConfig(level=logging.DEBUG)
```

This will show:
- Request URLs and responses
- Extraction steps
- Parsing errors
- Retry attempts

### Retry Logic

The scraper includes retry logic with configurable retry attempts and exponential backoff.

## Alternative Implementations

This repository provides multiple implementations for scraping Stockx Product Search pages:

### Python Implementations

- [BeautifulSoup - Product Search](./python/BeautifulSoup/product_search/README.md)
- [Selenium - Product Search](./python/selenium/product_search/README.md)

### Node.js Implementations

- [Cheerio - Product Search](./node/cheerio-axios/product_search/README.md)
- [Playwright - Product Search](./node/playwright/product_search/README.md)
- [Puppeteer - Product Search](./node/puppeteer/product_search/README.md)

### Choosing the Right Implementation

**Use BeautifulSoup/Cheerio** when:
- You need fast, lightweight scraping
- JavaScript rendering is not required
- You want minimal dependencies
- You're scraping simple HTML pages

**Use Playwright** when:
- You need modern browser automation with excellent API
- You want built-in waiting and auto-waiting features
- You need cross-browser support (Chromium, Firefox, WebKit)
- You want reliable network interception

**Use Puppeteer** when:
- You only need Chromium/Chrome support
- You want a mature, stable API
- You need Chrome DevTools Protocol features
- You prefer a smaller dependency footprint

**Use Selenium** when:
- You need maximum browser compatibility
- You're working with legacy systems
- You need WebDriver standard compliance
- You want the most widely-used framework


## Performance Considerations

### Concurrency

The scraper supports concurrent requests. See the scraper code for configuration options.

**Recommendations:**
- Start with minimal concurrency for testing
- Gradually increase based on your ScrapeOps plan limits
- Monitor for rate limiting or blocking

### Output Format

Data is saved in JSONL format (one JSON object per line):
- Efficient for large datasets
- Easy to process line-by-line
- Can be imported into databases or data processing tools
- Each line is a complete, valid JSON object

### Memory Usage

The scraper processes data incrementally:
- Products are written to file immediately after extraction
- No need to load entire dataset into memory
- Suitable for scraping large pages

## Best Practices

1. **Respect Rate Limits**: Use appropriate delays and concurrency settings
2. **Monitor ScrapeOps Usage**: Track your API usage in the ScrapeOps dashboard
3. **Handle Errors Gracefully**: Implement proper error handling and logging
4. **Validate URLs**: Ensure URLs are valid Stockx pages before scraping
5. **Update Selectors**: Stockx may change HTML structure; update selectors as needed
6. **Test Regularly**: Test scrapers regularly to catch breaking changes early

## Support & Resources

- **ScrapeOps Documentation**: [https://scrapeops.io/docs/intro/](https://scrapeops.io/docs/intro/)
- **Framework Documentation**: See framework-specific documentation
- **Example Output**: See `example-data/product_search.json` for sample data structure
- **Scraper Code**: See `scraper/stockx_scraper_product_search_v1.py` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using this scraper.