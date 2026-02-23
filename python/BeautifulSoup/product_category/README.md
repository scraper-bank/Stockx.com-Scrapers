# Stockx Product Category Scraper - BeautifulSoup (Python)

This Stockx Product Category scraper is a powerful Python-based tool designed to extract comprehensive product listings and metadata from Stockx category and brand pages. Utilizing BeautifulSoup for efficient HTML parsing and ScrapeOps for robust request management, it allows users to collect structured data including product names, prices, images, and pagination details for large-scale market analysis.

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

- **appliedFilters (array)**: List of active filters applied to the category view.
- **bannerImage**: URL of the category header or banner image.
- **breadcrumbs (array)**: Navigation path including names and URLs.
- **categoryId**: Unique identifier for the category.
- **categoryName**: The title of the category (e.g., "Nike Sneakers - Preschool").
- **categoryUrl**: The full URL of the scraped category page.
- **description**: SEO or category description text.
- **pagination (object)**: Details including current page, next/prev page URLs, and total results.
- **products (array)**: List of products containing names, prices, currency, images, and availability status.
- **subcategories (array)**: List of related sub-categories found on the page.

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
cd python/beautifulsoup/product_category
```

2. Edit the URLs in `scraper/stockx_scraper_product_category_v1.py`:

```python
# In stockx_scraper_product_category_v1.py
url = "https://stockx.com/brands/nike?category=sneakers&gender=kids&kids-age-group=preschool"
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
- `https://proxy.scrapeops.io/v1/?`
- `https://stockx.com/brands/nike?category=sneakers&gender=kids&kids-age-group=preschool`

Example URL: `https://proxy.scrapeops.io/v1/?`

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
| `appliedFilters` | array | Array of filter objects currently active. |
| `bannerImage` | null | URL string or null if no banner exists. |
| `breadcrumbs` | array | Array of objects containing name and url for site navigation. |
| `categoryId` | null | Unique ID for the category if available. |
| `categoryName` | string | The name of the category or brand page. |
| `categoryUrl` | string | The source URL of the category. |
| `description` | string | Text description of the category. |
| `pagination` | object | Object containing currentPage, nextPageUrl, totalPages, etc. |
| `products` | array | Array of product objects (name, price, img, etc.). |
| `subcategories` | array | List of nested or related category links. |

### Field Descriptions

- **appliedFilters** (array): Contains a list of items
- **bannerImage** (null): The main visual asset for the category header.
- **breadcrumbs** (array): Contains a list of object items showing the page hierarchy.
- **categoryId** (null): Internal Stockx identifier for the specific category.
- **categoryName** (string): Nike Sneakers - Preschool
- **categoryUrl** (string): https://stockx.com/brands/nike?category=sneakers&gender=kids&kids-age-group=preschool
- **description** (string): Text content describing the products in the category.
- **pagination** (object): Contains nested data regarding results count and page navigation.
- **products** (array): Contains a list of object items representing individual sneakers or items.
- **subcategories** (array): Contains a list of items for further filtering.

### Field Details
The `products` array contains objects with the following structure: `productId`, `name`, `brand`, `price`, `currency`, `image`, and `url`. The `pagination` object tracks `totalResults` and `totalPages` to assist in automated crawling of entire categories.

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

The scraper uses Python's `requests` library to fetch HTML content through the ScrapeOps Proxy API. Once the HTML is retrieved, it uses **BeautifulSoup** to parse the document structure. It specifically targets listing grids to identify product items, then extracts details like price and product URLs using CSS selectors and regular expressions. The script includes a `DataPipeline` class that manages duplicate detection and handles the streaming of data into a JSONL file, ensuring that even if a scrape is interrupted, the previously collected data is saved. It also features a `ThreadPoolExecutor` for concurrent processing of multiple category pages.

## Error Handling & Troubleshooting

- **403 Forbidden**: This usually indicates anti-bot detection. Ensure your ScrapeOps API key is valid and consider enabling advanced proxy features.
- **Empty Product List**: Stockx frequently changes its HTML class names. Check the `products` selectors in the code if extraction fails.
- **Connection Timeouts**: Increase the timeout settings in the `requests` call or lower the concurrency level.

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

- [Playwright - Product Category](./python/playwright/product_category/README.md)
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