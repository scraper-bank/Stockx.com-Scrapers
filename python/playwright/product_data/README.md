# Stockx Product Data Scraper - Playwright (Python)

This professional-grade Python scraper leverages the Playwright framework to extract comprehensive product information from Stockx. Designed for high-performance data collection, it features asynchronous execution and integrated anti-bot bypass capabilities via ScrapeOps to reliably capture pricing, market trends, and product specifications.

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

- **Product Identity**: Full name, secondary name/colorway, brand, and category.
- **Pricing Data**: Current price (number), currency (string), and pre-discount pricing.
- **Market Information**: Detailed market_data object including historical trends.
- **Media**: High-resolution image URLs (array of objects) and video links.
- **Product Details**: Full description (HTML/Text), product ID, and specifications.
- **Inventory Status**: Availability status (e.g., "in_stock").
- **Variants**: Available sizes and style variations (array).
- **Ratings**: Aggregate rating values and review counts.
- **Metadata**: Seller information and serial numbers.

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
cd python/playwright/product_data
```

2. Edit the URLs in `scraper/stockx_scraper_product_data_v1.py`:

```python
# In stockx_scraper_product_data_v1.py
url = "https://stockx.com/adidas-ctt-chinese-track-top-31-gender-neutral-jacket-asia-sizing-dark-grey"
```

3. Run the scraper:

```bash
python stockx_scraper_product_data_v1.py
```

The scraper will generate a timestamped JSONL file (e.g., `Stockx_com_product_data_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_data.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://stockx.com/[product-slug]`
- `https://stockx.com/adidas-ctt-chinese-track-top-31-gender-neutral-jacket-asia-sizing-dark-grey`
- Standard Stockx product detail pages across all categories (Sneakers, Apparel, Electronics, etc.)

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
| `aggregateRating` | object | Object containing nested fields |
| `availability` | string | String value |
| `brand` | string | String value |
| `category` | string | String value |
| `currency` | string | String value |
| `description` | string | String value |
| `features` | array | Array |
| `images` | array | Array of objects |
| `market_data` | object | Object containing nested fields |
| `name` | string | String value |
| `name_secondary` | string | String value |
| `preDiscountPrice` | null | null value |
| `price` | integer | Numeric value |
| `productId` | string | String value |
| `reviews` | array | Array |
| `seller` | object | Object containing nested fields |
| `serialNumbers` | array | Array of objects |
| `specifications` | array | Array of objects |
| `url` | string | String value |
| `variants` | array | Array of objects |
| `videos` | array | Array |

### Field Descriptions

- **aggregateRating** (object): Contains nested data including `ratingValue` and `reviewCount`.
- **availability** (string): Stock status, typically `in_stock`.
- **brand** (string): The manufacturer (e.g., adidas).
- **category** (string): Product classification (e.g., streetwear).
- **currency** (string): The currency code (e.g., USD).
- **description** (string): Detailed product description text.
- **features** (array): List of specific product features or highlights.
- **images** (array): List of image objects containing `url` and `altText`.
- **market_data** (object): Complex object containing historical sales data and market volatility.
- **name** (string): Full display title of the product.
- **name_secondary** (string): Often refers to the specific colorway or subtitle.
- **preDiscountPrice** (null): Original price before any markdowns.
- **price** (integer): The current listed price on the platform.
- **productId** (string): Unique UUID for the product in the Stockx database.
- **reviews** (array): List of individual user reviews.
- **seller** (object): Information regarding the fulfillment or seller details.
- **serialNumbers** (array): List of identification numbers associated with the item.
- **specifications** (array): Key-value pairs of technical details (e.g., Release Date, Style ID).
- **url** (string): The canonical URL of the scraped product page.
- **variants** (array): List of available sizes and their respective IDs/prices.
- **videos** (array): List of associated video content URLs.

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

This scraper uses **Playwright** for asynchronous browser automation, allowing it to render the JavaScript-heavy components of Stockx product pages. 

1. **Navigation**: It initializes a headless browser instance and navigates to the target product URL using `stealth_async` to minimize detection.
2. **Extraction Strategy**: It utilizes a combination of CSS selectors and JSON-LD parsing to extract structured data from the page's metadata and DOM elements.
3. **Data Processing**: Extracted data is mapped to a `ScrapedData` dataclass, ensuring type safety and consistent structure.
4. **Pipeline**: The `DataPipeline` handles de-duplication (based on URL/Name) and streams the results into a `.jsonl` file to maintain a low memory footprint.
5. **Proxying**: All traffic is routed through the ScrapeOps Residential Proxy network to ensure high success rates against sophisticated bot detection.

## Error Handling & Troubleshooting

- **Timeout Errors**: If the page fails to load, increase the `timeout` parameter in the Playwright `goto` function.
- **Empty Fields**: Stockx frequently updates its CSS classes. If fields return empty, inspect the page and update the selectors in the extraction logic.
- **IP Blocking**: If you receive 403 or 429 errors, ensure your ScrapeOps API key is active and that you are using residential proxies.
- **Data Duplication**: The built-in `DataPipeline` tracks seen URLs; if you restart a crawl, clear the `items_seen` set or the output file if a fresh crawl is required.

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

This repository provides multiple implementations for scraping Stockx Product Data pages:

### Python Implementations

- [BeautifulSoup - Product Data](./python/BeautifulSoup/product_data/README.md)
- [Selenium - Product Data](./python/selenium/product_data/README.md)

### Node.js Implementations

- [Cheerio - Product Data](./node/cheerio-axios/product_data/README.md)
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
- **Scraper Code**: See `scraper/stockx_scraper_product_data_v1.py` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using this scraper.