# Stockx Product Category Scraper - Playwright (Node.js)

This efficient Node.js scraper is designed to extract comprehensive product listing data from Stockx category pages using the Playwright framework and Cheerio for high-performance parsing. It features integrated stealth plugins and ScrapeOps proxy support to navigate Stockx's anti-bot protections while capturing detailed product metadata, pricing, and pagination info.

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

- **appliedFilters (array)**: List of active filters including filter name and value.
- **bannerImage (string)**: URL to the category header or social media image.
- **breadcrumbs (array)**: Navigation path leading to the current category.
- **categoryId (string)**: Unique identifier for the category (e.g., "electronics").
- **categoryName (string)**: The display name of the category.
- **categoryUrl (string)**: The full URL of the category being scraped.
- **description (string)**: The SEO description text providing context about the category products.
- **pagination (object)**: Metadata including current page, total pages, results per page, and navigation URLs.
- **products (array)**: List of items containing brand, name, price, currency, image URL, and availability status.
- **subcategories (array)**: Related sub-sections within the main category.

## Quick Start

### Prerequisites

- Node.js 14 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
npm install playwright cheerio playwright-extra puppeteer-extra-plugin-stealth
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```javascript
const API_KEY = "YOUR-API-KEY";
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd node/playwright/product_category
```

2. Edit the URLs in `scraper/stockx_scraper_product_category_v1.js`:

```javascript
// In stockx_scraper_product_category_v1.js
const url = "https://stockx.com/category/electronics";
```

3. Run the scraper:

```bash
node stockx_scraper_product_category_v1.js
```

The scraper will generate a timestamped JSONL file (e.g., `Stockx_com_product_category_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_category.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://stockx.com/category/[category-name]`
- `https://stockx.com/[category-name]`
- `https://stockx.com/category/electronics`

## Configuration

### Scraper Parameters

The scraper supports several configuration options. See the scraper code for available parameters.

### ScrapeOps Configuration

The scraper can use ScrapeOps for anti-bot protection and request optimization:

```javascript
// ScrapeOps proxy configuration
const PROXY_CONFIG = {
    server: 'http://residential-proxy.scrapeops.io:8181',
    username: 'scrapeops',
    password: API_KEY
};
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
| `appliedFilters` | array | List of active filter objects (name/value) |
| `bannerImage` | string | URL of the category banner image |
| `breadcrumbs` | array | Navigation links and names |
| `categoryId` | string | Internal slug or ID for the category |
| `categoryName` | string | Human-readable name of the category |
| `categoryUrl` | string | Source URL of the category |
| `description` | string | Text description of the category |
| `pagination` | object | Contains `currentPage`, `totalPages`, and result counts |
| `products` | array | List of product objects found on the page |
| `subcategories` | array | List of nested category links |

### Field Descriptions

- **appliedFilters** (array): Contains a list of object items currently filtering the view.
- **bannerImage** (string): The primary visual asset URL for the category page.
- **breadcrumbs** (array): Hierarchical list of parent pages.
- **categoryId** (string): The machine-readable identifier (e.g., "electronics").
- **categoryName** (string): The title shown at the top of the page.
- **categoryUrl** (string): The canonical URL of the page.
- **description** (string): Long-form text describing the products in this category.
- **pagination** (object): Details about the result set size and current page position.
- **products** (array): Individual product cards containing price, brand, and image data.
- **subcategories** (array): Links to more specific niches within the current category.

### Field Details
The `products` array contains objects with:
- `name`: Full title of the product.
- `price`: Numeric value of the current lowest ask/price.
- `brand`: The manufacturer or designer.
- `image`: URL to the product thumbnail.

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

The scraper uses **Playwright** with a **Stealth Plugin** to launch a headless browser instance that mimics human behavior. It navigates to the specified Stockx category URL using **ScrapeOps Residential Proxies** to avoid IP-based blocking. Once the page loads, the script retrieves the full HTML content and passes it to **Cheerio** for rapid server-side parsing. The extraction logic identifies product grids, pagination elements, and metadata. Finally, a `DataPipeline` class handles deduplication and saves the results into a `.jsonl` file incrementally to ensure data safety and low memory usage.

## Error Handling & Troubleshooting

### Troubleshooting
- **Empty Results**: Check if the selectors have changed. Stockx frequently updates its class names.
- **403 Forbidden**: Ensure your ScrapeOps API key is valid and you are using the residential proxy endpoint.
- **Timeout Errors**: Increase the `timeout` value in the `CONFIG` object if the page loads slowly.

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

This repository provides multiple implementations for scraping Stockx Product Category pages:

### Python Implementations

- [BeautifulSoup - Product Category](./python/BeautifulSoup/product_category/README.md)
- [Playwright - Product Category](./python/playwright/product_category/README.md)
- [Selenium - Product Category](./python/selenium/product_category/README.md)

### Node.js Implementations

- [Cheerio - Product Category](./node/cheerio-axios/product_category/README.md)
- [Puppeteer - Product Category](./node/puppeteer/product_category/README.md)

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

The scraper supports concurrent requests. See the scraper code for configuration options. Currently set to `maxConcurrency: 1` to ensure stability on high-security sites like Stockx.

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
- **Framework Documentation**: [Playwright Documentation](https://playwright.dev/)
- **Example Output**: See `example-data/product_category.json` for sample data structure
- **Scraper Code**: See `scraper/stockx_scraper_product_category_v1.js` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using this scraper.