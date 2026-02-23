# Stockx Product Search Scraper - Playwright (Node.js)

This powerful Node.js scraper leverages Playwright and Cheerio to efficiently extract search results from Stockx. It is designed to navigate search listings, handle dynamic content through browser automation, and capture detailed product metadata, pricing, and availability while utilizing ScrapeOps for robust anti-bot bypass.

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

- **Breadcrumbs (array)**: Navigation path leading to the search result.
- **Pagination (object)**: Details including current page, total pages, and next page URLs.
- **Products (array)**: Comprehensive list of items including:
    - Product Name and Brand
    - Description (string with HTML support)
    - Price and Currency (USD)
    - Availability status (e.g., "in_stock")
    - Images (array of objects with URLs and alt text)
    - Badges (e.g., "Xpress Ship")
- **Search Metadata (object)**: Contextual information about the search query and result count.

## Quick Start

### Prerequisites

- Node.js 14 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
npm install playwright cheerio
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```javascript
const API_KEY = "YOUR-API-KEY";
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd node/playwright/product_search
```

2. Edit the URLs in `scraper/stockx_scraper_product_search_v1.js`:

```javascript
// In stockx_scraper_product_search_v1.js
const url = "https://example.com/product";
```

3. Run the scraper:

```bash
node stockx_scraper_product_search_v1.js
```

The scraper will generate a timestamped JSONL file (e.g., `Stockx_com_product_search_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_search.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `http://residential-proxy.scrapeops.io:8181`
- `https://stockx.com`
- `https://stockx.com/search?s=kids+shoes`

## Configuration

### Scraper Parameters

The scraper supports several configuration options. See the scraper code for available parameters.

### ScrapeOps Configuration

The scraper can use ScrapeOps for anti-bot protection and request optimization:

```javascript
// ScrapeOps proxy configuration
const proxyUrl = `https://proxy.scrapeops.io/v1/?api_key=${API_KEY}`;
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
| `breadcrumbs` | array | Array of objects representing the site hierarchy |
| `pagination` | object | Object containing nested fields like `currentPage` and `totalPages` |
| `products` | array | Array of objects containing individual product details |
| `searchMetadata` | object | Object containing nested fields related to the search query |

### Field Descriptions

- **breadcrumbs** (array): Contains a list of object items
- **pagination** (object): Contains nested data
- **products** (array): Contains a list of object items
- **searchMetadata** (object): Contains nested data

### Field Details
- **Products Array**: Each item includes `brand`, `description`, `images` (array of objects), `currency`, and `availability`.
- **Pagination Object**: Includes `hasNextPage` (boolean) and `nextPageUrl` (string) to facilitate crawling multiple pages.

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

This scraper uses **Playwright** with the **Stealth Plugin** to launch a headless browser instance, which helps in mimicking real user behavior and bypassing basic bot detection. Once the page is loaded, the script retrieves the full HTML content and passes it to **Cheerio** for efficient parsing. 

The extraction logic focuses on identifying structured JSON-LD data or specific CSS selectors to pull product details, pricing, and pagination links. Data is processed through a `DataPipeline` class which ensures uniqueness by checking for duplicate items before saving them incrementally into a timestamped `.jsonl` file.

## Error Handling & Troubleshooting

- **Timeout Errors**: If the page fails to load, increase the `timeout` value in the `CONFIG` object.
- **Selector Failures**: Stockx frequently updates its frontend. If fields return `null`, inspect the page and update the CSS selectors in the `extractData` function.
- **Proxy Issues**: Ensure your ScrapeOps API key is active and has remaining credits if you encounter 403 or 401 status codes.

### Debugging

Enable detailed logging:

```javascript
// Enable debug logging
console.log('Debug mode enabled');
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
- [Selenium - Product Search](./python/selenium/product_search/README.md)

### Node.js Implementations

- [Cheerio - Product Search](./node/cheerio-axios/product_search/README.md)
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
- **Scraper Code**: See `scraper/stockx_scraper_product_search_v1.js` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using this scraper.