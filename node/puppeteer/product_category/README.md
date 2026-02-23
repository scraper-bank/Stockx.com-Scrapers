# Stockx Product Category Scraper - Puppeteer (Node.js)

This professional-grade Node.js scraper is designed to extract comprehensive product listing data from Stockx category pages using the Puppeteer framework. It features integrated stealth plugins to mimic real user behavior and utilizes ScrapeOps residential proxies to navigate anti-bot protections, making it ideal for large-scale market research and price monitoring.

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

- **appliedFilters (array)**: List of currently active filters on the category page
- **bannerImage (string)**: URL to the category's header or social media image
- **breadcrumbs (array)**: Navigation path including names and URLs (e.g., Home > Electronics)
- **categoryId (string)**: Unique identifier for the category (e.g., "electronics")
- **categoryName (string)**: Human-readable name of the category
- **categoryUrl (string)**: The full canonical URL of the category
- **description (string)**: Rich text description of the category and its products
- **pagination (object)**: Details on current page, total pages, and next/previous links
- **products (array)**: List of product objects containing names, prices, and thumbnails
- **subcategories (array)**: List of related sub-category links and names

## Quick Start

### Prerequisites

- Node.js 14 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
npm install puppeteer cheerio
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```javascript
const API_KEY = "YOUR-API-KEY";
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd node/puppeteer/product_category
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

- `https://stockx.com`
- `https://stockx.com/category/electronics`

Example URL: `https://stockx.com`

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
| `appliedFilters` | array | Array of filters applied to the current view |
| `bannerImage` | string | URL of the main category banner image |
| `breadcrumbs` | array | Array of objects containing 'name' and 'url' |
| `categoryId` | string | The slug/ID of the category |
| `categoryName` | string | The display name of the category |
| `categoryUrl` | string | Full URL of the category page |
| `description` | string | The SEO/Marketing description text for the category |
| `pagination` | object | Object containing current page and total results info |
| `products` | array | Array of objects representing individual products |
| `subcategories` | array | Array of objects for related child categories |

### Field Descriptions

- **appliedFilters** (array): Contains a list of items currently filtering the results
- **bannerImage** (string): https://stockx-assets.imgix.net/social-media.jpg
- **breadcrumbs** (array): Contains a list of object items representing the site hierarchy
- **categoryId** (string): electronics
- **categoryName** (string): Electronics
- **categoryUrl** (string): https://stockx.com/category/electronics
- **description** (string): Text content describing the category (e.g., details about Graphics Cards, iPhones, etc.)
- **pagination** (object): Contains nested data regarding page numbers and navigation links
- **products** (array): Contains a list of object items for each item found in the grid
- **subcategories** (array): Contains a list of object items for refining the search

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

This scraper uses **Puppeteer** with the **Stealth Plugin** to launch a headless browser instance that bypasses common bot detection signatures. It navigates to the specified Stockx category URL via a **ScrapeOps Residential Proxy** to ensure high deliverability. Once the page loads, the script uses **Cheerio** to parse the HTML and extract structured data from the DOM. A custom `DataPipeline` class manages the results, ensuring duplicates are filtered out before saving the data line-by-line into a `.jsonl` file.

## Error Handling & Troubleshooting

- **Timeout Errors**: If the page fails to load within 180 seconds, check your proxy connection or increase the `timeout` in the `CONFIG` object.
- **Empty Results**: Stockx frequently updates its class names; if extraction fails, verify that the selectors in the `extractData` function still match the live site.
- **Rate Limiting**: If you receive 403 or 429 errors, ensure your ScrapeOps API key is correct and consider decreasing `maxConcurrency`.

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
- [Playwright - Product Category](./node/playwright/product_category/README.md)

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
- **Example Output**: See `example-data/product_category.json` for sample data structure
- **Scraper Code**: See `scraper/stockx_scraper_product_category_v1.js` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using this scraper.