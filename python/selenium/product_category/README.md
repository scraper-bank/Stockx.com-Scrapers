# Stockx Product Category Scraper - Selenium (Python)

This powerful Stockx scraper allows you to efficiently extract comprehensive product listing data from category pages. Built with Python and Selenium, it leverages browser automation and ScrapeOps residential proxies to navigate Stockx's dynamic content, capturing product details, pricing, and pagination metadata with high reliability.

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

- **Applied Filters (array)**: List of active filters applied to the category view.
- **Banner Image (string)**: URL of the category's header or social media image.
- **Breadcrumbs (array)**: Navigation path including names and URLs (e.g., Home > Electronics).
- **Category Metadata**: Includes `categoryId`, `categoryName`, and the full `categoryUrl`.
- **Description (string)**: Detailed SEO text describing the category and featured brands.
- **Pagination (object)**: Critical navigation data including `currentPage`, `nextPageUrl`, `totalPages`, and `totalResults`.
- **Products (array)**: Detailed list of items containing:
    - Product Name and ID
    - Price (number) and Currency
    - Availability status
    - Brand name
    - Product Image URL
    - Meta flags (isPrime, isSponsored)
- **Subcategories (array)**: List of related sub-category links and names.

## Quick Start

### Prerequisites

- Python 3.7 or higher
- pip package manager (for Python) or npm (for Node.js)

### Installation

1. Install required dependencies:

```bash
pip install selenium beautifulsoup4
```

2. Get your ScrapeOps API key from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

3. Update the API key in the scraper:

```python
API_KEY = "YOUR-API-KEY"
```

### Running the Scraper

1. Navigate to the scraper directory:

```bash
cd python/selenium/product_category
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
- `https://stockx.com/[CATEGORY_NAME]?page=[PAGE_NUMBER]`

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
| `appliedFilters` | array | List of strings representing active filters |
| `bannerImage` | string | URL of the category banner image |
| `breadcrumbs` | array | Array of objects containing `name` and `url` |
| `categoryId` | string | Unique identifier for the category |
| `categoryName` | string | Human-readable name of the category |
| `categoryUrl` | string | The full URL of the scraped category |
| `description` | string | Text description of the category content |
| `pagination` | object | Object with `currentPage`, `totalPages`, and `nextPageUrl` |
| `products` | array | List of product objects with price, name, and image |
| `subcategories` | array | List of related sub-category objects |

### Field Descriptions

- **appliedFilters** (array): Contains a list of items currently filtering the results.
- **bannerImage** (string): Link to the primary visual asset for the category.
- **breadcrumbs** (array): Hierarchical navigation links leading to the current page.
- **categoryId** (string): The internal slug or ID used by the site (e.g., "electronics").
- **categoryName** (string): The display name of the category.
- **categoryUrl** (string): The canonical URL of the category page.
- **description** (string): The marketing or SEO text found at the bottom or top of the page.
- **pagination** (object): Metadata regarding the result set size and navigation links.
- **products** (array): The main collection of product data extracted from the grid.
- **subcategories** (array): Links to more specific niches within the parent category.

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

This scraper uses **Selenium** with `undetected_chromedriver` to simulate a real user browsing the Stockx website. It initializes a WebDriver instance configured with **ScrapeOps Residential Proxies** to mask the scraper's identity and avoid IP-based blocking.

The process follows these steps:
1. **Navigation**: The scraper loads the specified Stockx category URL.
2. **Waiting**: It uses `WebDriverWait` to ensure the product grid and dynamic elements are fully loaded.
3. **Extraction**: It parses the page source using both Selenium selectors and BeautifulSoup for robust data extraction.
4. **Data Cleaning**: Product IDs, prices (converted to numbers), and image URLs are cleaned and structured into a `ScrapedData` dataclass.
5. **Pipeline**: A `DataPipeline` checks for duplicate entries before appending the results to a JSONL file.

## Error Handling & Troubleshooting

### Common Issues
- **TimeoutException**: Occurs if the page takes too long to load or anti-bot measures block the request. Try increasing the wait time or checking your proxy credentials.
- **StaleElementReferenceException**: Happens when the DOM changes during extraction. The scraper includes logic to re-locate elements if this occurs.
- **Rate Limiting**: If you receive 403 or 429 errors, reduce your concurrency or ensure your ScrapeOps API key is active.

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
- [Playwright - Product Category](./python/playwright/product_category/README.md)

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

The scraper supports concurrent requests using `ThreadPoolExecutor`. See the scraper code for configuration options.

**Recommendations:**
- Start with minimal concurrency (e.g., 2-3 threads) for testing.
- Gradually increase based on your ScrapeOps plan limits.
- Monitor for rate limiting or blocking.

### Output Format

Data is saved in JSONL format (one JSON object per line):
- Efficient for large datasets.
- Easy to process line-by-line.
- Can be imported into databases or data processing tools.
- Each line is a complete, valid JSON object.

### Memory Usage

The scraper processes data incrementally:
- Products are written to file immediately after extraction.
- No need to load entire dataset into memory.
- Suitable for scraping large pages.

## Best Practices

1. **Respect Rate Limits**: Use appropriate delays and concurrency settings.
2. **Monitor ScrapeOps Usage**: Track your API usage in the ScrapeOps dashboard.
3. **Handle Errors Gracefully**: Implement proper error handling and logging.
4. **Validate URLs**: Ensure URLs are valid Stockx pages before scraping.
5. **Update Selectors**: Stockx may change HTML structure; update selectors as needed.
6. **Test Regularly**: Test scrapers regularly to catch breaking changes early.

## Support & Resources

- **ScrapeOps Documentation**: [https://scrapeops.io/docs/intro/](https://scrapeops.io/docs/intro/)
- **Framework Documentation**: [Selenium Python Docs](https://selenium-python.readthedocs.io/)
- **Example Output**: See `example-data/product_category.json` for sample data structure
- **Scraper Code**: See `scraper/stockx_scraper_product_category_v1.py` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using this scraper.