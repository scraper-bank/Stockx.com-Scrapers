# Stockx Product Search Scraper - Selenium (Python)

This powerful Python scraper allows you to efficiently extract search results and product listings from Stockx using the Selenium framework. It is designed to navigate through search pages, handle dynamic content, and bypass common anti-bot hurdles using ScrapeOps residential proxies to ensure reliable data collection for market analysis and price tracking.

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

- **breadcrumbs**: Navigation path (array of objects containing name and URL)
- **pagination**: Current page, total pages, and next/previous page status (object)
- **products**: Comprehensive list of search results including:
    - Product name (string)
    - Price and Currency (number/string)
    - Product ID and URL (string)
    - Availability status (string)
    - Images (array of objects with URLs and alt text)
    - Badges like "Xpress Ship" (array)
    - Brand and Category (string)
    - Price Range (min/max price object)
- **searchMetadata**: Metadata related to the search query (object)
- **recommendations**: Suggested products based on search (object)
- **relatedSearches**: List of similar search terms (array)
- **sponsoredProducts**: List of promoted items (array)

## Quick Start

### Prerequisites

- Python 3.7 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
pip install selenium beautifulsoup4
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```python
API_KEY = "YOUR-API-KEY"
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd python/selenium/product_search
```

2. Edit the URLs in `scraper/stockx_scraper_product_search_v1.py`:

```python
# In stockx_scraper_product_search_v1.py
url = "https://stockx.com/search?s=kids+shoes"
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

- `https://stockx.com`
- `https://stockx.com/search?s=[QUERY]`
- `https://stockx.com/search?s=kids+shoes`

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
| `breadcrumbs` | array | Array of objects containing navigation links |
| `pagination` | object | Object containing pagination state (currentPage, totalPages, etc.) |
| `products` | array | Array of objects representing individual product listings |
| `recommendations` | object | Object containing recommended product data |
| `relatedSearches` | array | List of alternative search terms |
| `searchMetadata` | object | Object containing metadata about the search result set |
| `sponsoredProducts` | array | List of paid/sponsored product placements |

### Field Descriptions

- **breadcrumbs** (array): Contains a list of object items representing the site hierarchy.
- **pagination** (object): Contains nested data regarding the current page and navigation links.
- **products** (array): Contains a list of object items, each representing a product found in search results.
- **recommendations** (object): Contains nested data for suggested items.
- **relatedSearches** (array): Contains a list of items related to the user's query.
- **searchMetadata** (object): Contains nested data regarding result counts and query info.
- **sponsoredProducts** (array): Contains a list of items that are promoted.

### Field Details
The `products` array contains objects with detailed nested structures, including an `images` array (URL and alt text) and a `priceRange` object indicating the volatility of the product's market value.

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

This scraper uses **Selenium** with the `undetected_chromedriver` and `selenium-wire` extensions to mimic human browsing behavior. It navigates to the Stockx search URL and waits for the dynamic content to load using explicit waits (`WebDriverWait`). 

The extraction process involves:
1. **Initialization**: Setting up a Chrome instance with ScrapeOps residential proxies to mask the scraper's identity.
2. **Navigation**: Accessing the search page and handling any initial overlays.
3. **Data Extraction**: Using a combination of Selenium selectors and BeautifulSoup to parse the HTML and extract product details defined in the `ScrapedData` dataclass.
4. **Pagination**: Detecting the "Next" page and iterating through the results until the specified limit or `totalPages` is reached.
5. **Persistence**: Saving the results incrementally to a JSONL file via a `DataPipeline` to prevent data loss and manage memory.

## Error Handling & Troubleshooting

- **TimeoutErrors**: If pages load too slowly, increase the `WebDriverWait` duration.
- **Proxy Issues**: Ensure your ScrapeOps API key is active and has remaining credits.
- **Selector Changes**: Stockx frequently updates its CSS classes. If data returns as `null`, inspect the page and update the `By.CSS_SELECTOR` paths.
- **Rate Limiting**: If you encounter 403 errors, increase the sleep time between requests or reduce concurrency.

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
- [Playwright - Product Search](./python/playwright/product_search/README.md)

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

The scraper supports concurrent requests using `ThreadPoolExecutor`. See the scraper code for configuration options.

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