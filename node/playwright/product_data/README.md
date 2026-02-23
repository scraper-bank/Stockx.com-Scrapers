# Stockx Product Data Scraper - Playwright (Node.js)

This high-performance Node.js scraper leverages Playwright and Cheerio to extract comprehensive product information from Stockx. Designed for reliability and speed, it features stealth integration to mimic real user behavior and ScrapeOps proxy support to navigate complex anti-bot systems effectively.

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

- **Product Identity**: Name (string), Brand (string), and Product ID (string)
- **Pricing & Availability**: Current Price (number), Pre-discount Price (null/number), Currency (string), and Availability status (string)
- **Categorization**: Product Category (string)
- **Media**: Product Images (array of objects containing URLs and alt text) and Videos (array)
- **Descriptive Content**: Full Product Description (string) and Features (array)
- **Seller Information**: Seller details including Name and Ratings (object)
- **Technical Metadata**: Specifications (array), Serial Numbers (array), and Source URL (string)
- **Social Proof**: Aggregate Ratings and Reviews (arrays/objects)

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
cd node/playwright/product_data
```

2. Edit the URLs in `scraper/stockx_scraper_product_data_v1.js`:

```javascript
// In stockx_scraper_product_data_v1.js
const url = "https://example.com/product";
```

3. Run the scraper:

```bash
node stockx_scraper_product_data_v1.js
```

The scraper will generate a timestamped JSONL file (e.g., `Stockx_com_product_data_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_data.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://stockx.com/[product-slug]`
- `https://stockx.com/adidas-ctt-chinese-track-top-31-gender-neutral-jacket-asia-sizing-dark-grey`
- General Stockx product detail pages (PDPs)

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
| `reviews` | array | Array |
| `seller` | object | Object containing nested fields |
| `serialNumbers` | array | Array of objects |
| `specifications` | array | Array of objects |
| `url` | string | String value |
| `videos` | array | Array |

### Field Descriptions

- **aggregateRating** (null): Currently returns null if no aggregate rating is found.
- **availability** (string): Stock status (e.g., "in_stock").
- **brand** (string): The manufacturer or brand name (e.g., "adidas").
- **category** (string): The broad classification of the item (e.g., "streetwear").
- **currency** (string): The currency code (e.g., "USD").
- **description** (string): Detailed product description, often containing HTML which the scraper cleans.
- **features** (array): A list of specific product highlights or attributes.
- **images** (array): A collection of image objects containing `url` and `alt_text`.
- **name** (string): The full title of the product.
- **preDiscountPrice** (null): Original price before any markdowns.
- **price** (integer): The current listing price.
- **productId** (string): Unique internal identifier for the product.
- **reviews** (array): List of user reviews (if available).
- **seller** (object): Details about the merchant, including name and rating.
- **serialNumbers** (array): List of specific identifiers for the item.
- **specifications** (array): Technical details and size information.
- **url** (string): The direct link to the product page.
- **videos** (array): Any video content associated with the product.

### Field Details
The `seller` object typically contains `name` and `rating` fields. The `images` array contains objects structured as `{ "url": "...", "alt_text": "..." }`.

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

The scraper utilizes **Playwright** with the **Stealth Plugin** to launch a headless browser instance that bypasses common bot detection signatures. It navigates to the specified Stockx product URL and waits for the page content to load. Once the page is ready, the HTML is passed to **Cheerio** for efficient parsing. 

The extraction logic targets specific CSS selectors and structured data elements to pull product details. A custom `DataPipeline` class manages the results, ensuring that duplicate items are filtered out before being saved. The data is then appended to a JSONL file in real-time, which ensures data safety even if the process is interrupted.

## Error Handling & Troubleshooting

If you encounter issues, check the following:
- **Timeouts**: Stockx pages can be heavy. Increase the `timeout` in the `CONFIG` object if pages fail to load.
- **Selector Changes**: If fields return `null`, Stockx may have updated their HTML structure. Inspect the page and update the `extractData` function.
- **Proxy Errors**: Ensure your ScrapeOps API key is valid and you have remaining credits.

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

This repository provides multiple implementations for scraping Stockx Product Data pages:

### Python Implementations

- [BeautifulSoup - Product Data](./python/BeautifulSoup/product_data/README.md)
- [Playwright - Product Data](./python/playwright/product_data/README.md)
- [Selenium - Product Data](./python/selenium/product_data/README.md)

### Node.js Implementations

- [Cheerio - Product Data](./node/cheerio-axios/product_data/README.md)
- [Puppeteer - Product Data](./node/puppeteer/product_data/README.md)

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
- **Example Output**: See `example-data/product_data.json` for sample data structure
- **Scraper Code**: See `scraper/stockx_scraper_product_data_v1.js` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using this scraper.