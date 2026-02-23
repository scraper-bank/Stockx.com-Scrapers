# Stockx Product Search Scraper - Puppeteer (Node.js)

This Node.js scraper is designed to extract comprehensive product listings from Stockx search result pages using the Puppeteer framework. It leverages browser automation and stealth plugins to capture real-time market data, including prices, product metadata, and image assets, while efficiently handling dynamic content.

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

- **Product Name (string)**: Full title of the item (e.g., "Jordan 4 Retro Black Cat").
- **Price (number)**: The current lowest ask or listed price.
- **Currency (string)**: The currency code for the price (e.g., "USD").
- **Availability (string)**: Stock status (e.g., "in_stock").
- **Category (string)**: Product classification (e.g., "sneakers").
- **Description (string)**: Search page or category meta description.
- **Images (array)**: List of image objects containing URLs and alt text.
- **Products (array)**: A collection of individual product objects found on the search page, each containing its own URL, name, and price.
- **URL (string)**: Direct link to the specific product page.
- **Metadata**: Includes brand, aggregate ratings, features, reviews, and specifications (where available).

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
cd node/puppeteer/product_search
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

- `https://stockx.com`
- `https://stockx.com/search?s=kids+shoes`

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
| `aggregateRating` | null | null value |
| `availability` | string | String value |
| `brand` | string | String value |
| `category` | string | String value |
| `currency` | string | String value |
| `description` | string | String value |
| `features` | array | Array |
| `images` | array | Array of objects |
| `name` | string | String value |
| `preDiscountPrice` | null | null value |
| `price` | integer | Numeric value |
| `productId` | string | String value |
| `products` | array | Array of objects |
| `reviews` | array | Array |
| `seller` | null | null value |
| `serialNumbers` | array | Array |
| `specifications` | array | Array |
| `url` | string | String value |
| `videos` | array | Array |

### Field Descriptions

- **aggregateRating** (null): The overall rating score for the product list (if available).
- **availability** (string): Current stock status, typically "in_stock".
- **brand** (string): The brand name associated with the search results.
- **category** (string): The primary category of the results (e.g., sneakers).
- **currency** (string): The 3-letter currency code, usually USD.
- **description** (string): A short textual summary of the search page content.
- **features** (array): Contains a list of items representing product highlights.
- **images** (array): Contains a list of object items with 'url' and 'altText'.
- **name** (string): The title of the primary or first product found.
- **preDiscountPrice** (null): Original price before any markdowns.
- **price** (integer): The numerical value of the item's current price.
- **productId** (string): Unique identifier for the product.
- **products** (array): Contains a list of object items found in the search results (grid items).
- **reviews** (array): Contains a list of items representing customer feedback.
- **seller** (null): Information regarding the merchant or seller.
- **serialNumbers** (array): Contains a list of items for specific model IDs.
- **specifications** (array): Contains a list of items for technical details.
- **url** (string): The direct canonical URL to the product.
- **videos** (array): Contains a list of items for video media.

### Field Details
- **Products Array**: Each object in the `products` list contains nested fields: `name`, `price`, `url`, `availability`, and `images`.
- **Images Array**: Objects contain specific `url` strings and `altText` descriptions for accessibility.

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

This scraper uses **Puppeteer** to launch a headless Chromium instance. It utilizes the `puppeteer-extra-plugin-stealth` to mimic human behavior and bypass basic bot detection. 

1. **Navigation**: The scraper navigates to the provided Stockx search URL.
2. **Extraction**: Once the page loads, it uses **Cheerio** to parse the HTML content. It targets specific CSS selectors to find the product grid and metadata.
3. **Data Processing**: The script iterates through the product list, cleaning strings (via `stripHTML`) and detecting currencies.
4. **Pipeline**: A `DataPipeline` class manages the output, ensuring duplicate items are filtered out before appending them to a `.jsonl` file.
5. **Proxying**: All traffic is routed through the ScrapeOps Residential Proxy gateway to hide the scraper's origin IP.

## Error Handling & Troubleshooting

- **Selector Changes**: If the scraper returns empty arrays, Stockx may have updated their class names. Inspect the page and update the selectors in the script.
- **Timeouts**: Stockx is a heavy site. If the scraper fails to load, increase the `CONFIG.timeout` value.
- **Rate Limiting**: If you receive 403 or 429 errors, ensure your ScrapeOps API key is active and consider reducing `maxConcurrency`.

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
- [Playwright - Product Search](./node/playwright/product_search/README.md)

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