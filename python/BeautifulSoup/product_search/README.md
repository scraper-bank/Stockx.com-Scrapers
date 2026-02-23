# Stockx Product Search Scraper - BeautifulSoup (Python)

This professional Python scraper specializes in extracting comprehensive search results from Stockx using the BeautifulSoup library. It efficiently captures product listings, pricing, and availability while utilizing ScrapeOps for enhanced request management and anti-bot bypass capabilities.

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

- **breadcrumbs (array)**: Navigation hierarchy including page names and URLs.
- **pagination (object)**: Current page and total available pages for the search query.
- **products (array)**: A list of search results including:
    - Product name (string)
    - Current price (number)
    - Currency code (string)
    - Availability status (string)
    - Product URL (string)
    - Images (array of objects containing alt text and source URLs)
- **searchMetadata (object)**: Contextual data related to the search execution.

## Quick Start

### Prerequisites

- Python 3.7 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
pip install -r requirements.txt
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```python
API_KEY = "YOUR-API-KEY"
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd python/beautifulsoup/product_search
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
- `https://images.stockx.com/images/{url_key}-Product.jpg`
- `https://proxy.scrapeops.io/v1/?`
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
| `pagination` | object | Object containing current and total page counts |
| `products` | array | Array of objects representing individual product listings |
| `searchMetadata` | object | Object containing metadata about the search result |

### Field Descriptions

- **breadcrumbs** (array): Contains a list of object items, each with `name` and `url`.
- **pagination** (object): Contains nested data such as `currentPage` (integer) and `totalPages` (integer).
- **products** (array): Contains a list of items including `name`, `price`, `url`, and `availability`.
- **searchMetadata** (object): Contains internal metadata used to track the search context.

### Field Details
- **Images Array**: Each product contains an `images` array with objects providing the image `url` and `altText`.
- **Price**: Represented as a number (e.g., `147`) extracted from the currency-formatted string on the page.

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

The scraper utilizes **BeautifulSoup** and **Requests** to fetch and parse the HTML content of Stockx search pages. It follows a structured workflow:
1. **URL Construction**: It takes a search query or category URL and prepares the request.
2. **Proxy Integration**: Requests are routed through the ScrapeOps Proxy API to mask the scraper's identity and handle rotation.
3. **HTML Parsing**: BeautifulSoup identifies specific CSS selectors to extract product details, breadcrumbs, and pagination info.
4. **Data Cleaning**: The script includes utility functions like `strip_html` to clean text and `make_absolute_url` to ensure all links are fully qualified.
5. **Deduplication**: A `DataPipeline` class ensures that duplicate products are not saved within the same session.
6. **Output**: Extracted items are serialized into the JSONL format for efficient storage.

## Error Handling & Troubleshooting

- **Connection Errors**: Usually caused by aggressive blocking. Ensure your ScrapeOps API key is valid and you are using the proxy URL.
- **Empty Results**: Stockx frequently updates its HTML structure. If selectors break, check if the classes in the BeautifulSoup `find` methods still match the live site.
- **Rate Limiting**: If you receive 429 errors, increase the delay between requests or reduce the number of concurrent threads in the `ThreadPoolExecutor`.

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

- [Playwright - Product Search](./python/playwright/product_search/README.md)
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