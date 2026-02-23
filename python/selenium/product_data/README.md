# Stockx Product Data Scraper - Selenium (Python)

This comprehensive Stockx Product Data scraper allows you to extract detailed product information including historical pricing, brand details, and high-resolution images. Built with Python and Selenium, it utilizes `undetected-chromedriver` and ScrapeOps residential proxies to navigate Stockx's sophisticated anti-bot protections effectively.

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
- **Categorization**: Category/Department (string)
- **Pricing**: Current Price (number), Currency (string), and Pre-discount Price (if available)
- **Media**: Product Images (array of objects with URLs and alt text) and Videos (array)
- **Product Details**: Full Description (string), Features (array), and Specifications (array)
- **Availability**: Stock Status (string)
- **Metadata**: Serial Numbers (array), Aggregate Ratings (object), and Seller Information (object)

## Quick Start

### Prerequisites

- Python 3.7 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
pip install selenium beautifulsoup4 selenium-wire undetected-chromedriver
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```python
API_KEY = "YOUR-API-KEY"
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd python/selenium/product_data
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
- General Stockx product detail pages across various categories (Sneakers, Streetwear, Electronics, etc.)

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

- **aggregateRating** (null): The average user rating score (if available).
- **availability** (string): Current stock status (e.g., "in_stock").
- **brand** (string): The manufacturer or brand name (e.g., "adidas").
- **category** (string): The primary category the product belongs to.
- **currency** (string): The ISO currency code for the price.
- **description** (string): Detailed product description text.
- **features** (array): List of specific product highlights.
- **images** (array): List of objects containing image `url` and `alt_text`.
- **name** (string): Full title of the product.
- **preDiscountPrice** (null): Original price before any active discounts.
- **price** (integer): The current listed price on Stockx.
- **productId** (string): Unique identifier (UUID) for the product.
- **reviews** (array): List of user reviews.
- **seller** (object): Information about the seller (defaults to Stockx).
- **serialNumbers** (array): Relevant serial or model numbers.
- **specifications** (array): Technical details and attributes.
- **url** (string): The direct link to the product page.
- **videos** (array): List of related video content URLs.

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

This scraper uses **Selenium** with the `undetected-chromedriver` library to mimic real user behavior and bypass common automated browser detection.

- **Navigation**: It initializes a browser instance and navigates to the provided Stockx product URL.
- **Extraction**: Once the page loads, it uses a combination of Selenium's `By.CSS_SELECTOR` and `BeautifulSoup` to parse the DOM for structured data.
- **Data Processing**: Extracted data is mapped to a Python `dataclass` called `ScrapedData`, ensuring type safety and consistent structure.
- **Concurrency**: The script utilizes `ThreadPoolExecutor` and `threading.local()` to manage multiple WebDriver instances simultaneously for faster data collection.
- **Persistence**: Data is validated for duplicates using a `DataPipeline` and saved in JSONL format for efficient storage.

## Error Handling & Troubleshooting

### Common Issues
- **TimeoutErrors**: Stockx pages can be heavy. The scraper uses `WebDriverWait` to handle slow-loading elements.
- **Selector Changes**: If Stockx updates their frontend, selectors may break. Check the `ScrapedData` mapping in the code.
- **IP Bans**: If you encounter 403 errors, ensure your ScrapeOps API key is valid and proxies are enabled.

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
- [Playwright - Product Data](./python/playwright/product_data/README.md)

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