# Stockx Product Data Scraper - Cheerio (Node.js)

This specialized Node.js scraper is designed to extract comprehensive product information from Stockx using the Cheerio and Axios libraries. It efficiently captures product details, pricing, branding, and media assets while utilizing ScrapeOps to navigate anti-bot protections and ensure reliable data collection.

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

- **Product Identity**: Name (string), Product ID (string), and Brand (string)
- **Pricing & Availability**: Current Price (number), Currency (string), and Availability status (string)
- **Categorization**: Product Category (string)
- **Media Assets**: Images (array of objects containing URLs and alt text)
- **Descriptions**: Full product description (string with HTML stripping support)
- **Metadata**: Specifications (array), Serial Numbers (array), and Features (array)
- **Social Proof**: Aggregate Ratings and Reviews (array)
- **Seller Info**: Detailed Seller information (object)
- **Source Info**: Product URL (string)

## Quick Start

### Prerequisites

- Node.js 14 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
npm install cheerio axios
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```javascript
const API_KEY = "YOUR-API-KEY";
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd node/cheerio-axios/product_data
```

2. Edit the URLs in `scraper/stockx_scraper_product_data_v1.js`:

```javascript
// In stockx_scraper_product_data_v1.js
const url = "https://stockx.com/adidas-ctt-chinese-track-top-31-gender-neutral-jacket-asia-sizing-dark-grey";
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
| `videos` | null | null value |

### Field Descriptions

- **aggregateRating** (null): The average rating score, if available.
- **availability** (string): Current stock status (e.g., "in_stock").
- **brand** (string): The manufacturer or brand name (e.g., "adidas").
- **category** (string): The high-level product category (e.g., "streetwear").
- **currency** (string): The currency code for the price (e.g., "USD").
- **description** (string): Detailed product description, cleaned of HTML tags.
- **features** (array): A list of specific product highlights or features.
- **images** (array): A list of objects containing image URLs and descriptive alt text.
- **name** (string): The full product title.
- **preDiscountPrice** (null): Original price before any discounts were applied.
- **price** (integer): The current listing price on the platform.
- **productId** (string): The unique identifier used by the site.
- **reviews** (array): A list of user-submitted reviews.
- **seller** (object): Information regarding the merchant or marketplace seller.
- **serialNumbers** (array): List of specific identifiers or SKUs.
- **specifications** (array): Technical details and attributes.
- **url** (string): The direct link to the scraped product page.
- **videos** (null): Links to video content, if present.

### Field Details
- **images**: Items contain `url` (string) and `altText` (string).
- **seller**: Nested object containing merchant details such as name or rating if available in the source HTML.

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

This scraper uses a static extraction approach combining **Axios** for HTTP requests and **Cheerio** for HTML parsing. 
1. **Request Initiation**: It sends requests through the ScrapeOps Proxy API to bypass basic IP filtering and regional restrictions.
2. **HTML Parsing**: Once the HTML content is retrieved, Cheerio loads the DOM, allowing the script to use jQuery-like selectors to find specific data points (like brand names, prices, and JSON-LD blocks).
3. **Data Cleaning**: A `stripHTML` helper function is utilized to ensure descriptions are returned as clean text.
4. **Data Pipeline**: The `DataPipeline` class manages the state, checking for duplicate `productId`s before appending the results to a JSONL file.
5. **Concurrency**: The script uses a configurable concurrency limit (defaulting to 1) to respect the target's server and avoid triggering aggressive rate limits.

## Error Handling & Troubleshooting

### Troubleshooting
- **Empty Results**: Stockx frequently updates its CSS classes. If data is missing, check if the selectors in `extractData` still match the live site.
- **403 Forbidden**: This usually indicates your IP is blocked. Ensure the ScrapeOps API key is active and the proxy URL is correctly formatted.
- **Timeout Errors**: Large pages or slow proxy responses can cause timeouts. Increase the `CONFIG.timeout` value in the script.

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

- [Playwright - Product Data](./node/playwright/product_data/README.md)
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