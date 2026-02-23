# Stockx Product Category Scraper - Playwright (Python)

This powerful Python scraper is designed to extract comprehensive product listing data from Stockx category pages using the Playwright framework. It efficiently captures product details, category hierarchies, and filtering metadata while utilizing ScrapeOps residential proxies to navigate anti-bot protections.

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

- **appliedFilters (array)**: List of active filters including filter names and values.
- **bannerImage (string)**: URL of the category's header or social media image.
- **breadcrumbs (array)**: Navigation path objects containing names and URLs.
- **categoryId (string)**: Unique identifier for the category (e.g., "electronics").
- **categoryName (string)**: Human-readable name of the category.
- **categoryUrl (string)**: The full canonical URL of the category page.
- **description (string)**: Detailed SEO text and category descriptions.
- **pagination (object)**: Metadata regarding total pages and current position.
- **products (array)**: List of product objects found in the category listing.
- **subcategories (array)**: Nested sub-category links and names for deeper navigation.

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
cd python/playwright/product_category
```

2. Edit the URLs in `scraper/stockx_scraper_product_category_v1.py`:

```python
# In stockx_scraper_product_category_v1.py
url = "https://stockx.com/category/electronics"
```

3. Run the scraper:

```bash
python stockx_scraper_product_category_v1.py
```

The scraper will generate a timestamped JSONL file (e.g., `Stockx_com_product_category_page_scraper_data_20260114_120000.jsonl`) containing all extracted data.

### Example Output

See `example-data/product_category.json` for a sample of the extracted data structure.

## Supported URLs

The scraper supports the following URL patterns:

- `https://stockx.com`
- `https://stockx.com/category/electronics`
- `https://stockx.com/category/[CATEGORY_NAME]`

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
| `appliedFilters` | array | Array of objects representing active search filters |
| `bannerImage` | string | URL string for the category banner |
| `breadcrumbs` | array | Array of objects containing navigation links |
| `categoryId` | string | Internal slug or ID for the category |
| `categoryName` | string | Display name of the category |
| `categoryUrl` | string | Full URL of the scraped category |
| `description` | string | Full text description of the category |
| `pagination` | object | Object containing pagination metadata |
| `products` | array | Array of individual product item objects |
| `subcategories` | array | Array of related sub-category objects |

### Field Descriptions

- **appliedFilters** (array): Contains a list of object items, usually pairing `filterName` with `filterValue`.
- **bannerImage** (string): The primary image associated with the category, often used for social sharing or headers.
- **breadcrumbs** (array): A list of objects representing the site hierarchy leading to this page.
- **categoryId** (string): A unique identifier, such as "electronics" or "sneakers".
- **categoryName** (string): The title of the category as it appears on the page.
- **categoryUrl** (string): The absolute URL of the category being scraped.
- **description** (string): Detailed marketing or SEO text found on the category page.
- **pagination** (object): Metadata including current page, total pages, and items per page.
- **products** (array): The core data list containing individual product cards/details.
- **subcategories** (array): Links and names for related or child categories.

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

The scraper uses **Playwright** to launch a headless browser and navigate to the specified Stockx category URL. It utilizes `playwright_stealth` to reduce the likelihood of detection by hiding automated browser signatures. 

Once the page loads, the script uses a combination of CSS selectors and regular expressions to locate data points. The `ScrapedData` dataclass ensures a consistent structure for the extracted information. The scraper handles common data cleaning tasks like converting relative URLs to absolute URLs and stripping HTML tags from descriptions. Finally, the `DataPipeline` class manages deduplication and saves each category's data into a JSONL file.

## Error Handling & Troubleshooting

- **Proxy Failures**: If the residential proxy connection fails, verify your API key and ensure you have remaining credits in your ScrapeOps account.
- **Selector Changes**: Stockx frequently updates its frontend. If fields return empty, inspect the page in a browser to see if class names or data attributes have changed.
- **Timeout Issues**: Large category pages with many images may take longer to load. Increase the `timeout` settings in the Playwright `goto` function if necessary.

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

This repository provides multiple implementations for scraping Stockx Product Category pages:

### Python Implementations

- [BeautifulSoup - Product Category](./python/BeautifulSoup/product_category/README.md)
- [Selenium - Product Category](./python/selenium/product_category/README.md)

### Node.js Implementations

- [Cheerio - Product Category](./node/cheerio-axios/product_category/README.md)
- [Playwright - Product Category](./node/playwright/product_category/README.md)
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
- **Scraper Code**: See `scraper/stockx_scraper_product_category_v1.py` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using this scraper.