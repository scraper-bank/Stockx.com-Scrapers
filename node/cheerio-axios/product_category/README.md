# Stockx Product Category Scraper - Cheerio (Node.js)

Automate the extraction of comprehensive category and product listing data from Stockx using Node.js and Cheerio. This high-performance scraper efficiently captures product details, hierarchical breadcrumbs, and filtering metadata while leveraging Axios and ScrapeOps for robust request management.

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

- **Category Metadata**: Category Name, ID, Description, and URL
- **Product Listings**: Array of products found within the category
- **Navigation Data**: Full breadcrumb path (array) and subcategories
- **Media**: Banner images (string) and product image arrays
- **Ratings**: Aggregate rating values, review counts, and scale (object)
- **Filtering**: Applied filters including names and values
- **Pagination**: Details on current page and total results
- **Technical Specs**: Specifications, serial numbers, and features (arrays)
- **Seller Info**: Detailed seller metadata (object)

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
cd node/cheerio-axios/product_category
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
- `https://stockx.com/[brand-name]`
- `https://stockx.com/category/electronics`

Example URL: `https://stockx.com/category/electronics`

## Configuration

### Scraper Parameters

The scraper supports several configuration options. See the scraper code for available parameters.

### ScrapeOps Configuration

The scraper can use ScrapeOps for anti-bot protection and request optimization:

```javascript
// ScrapeOps proxy configuration
const proxyUrl = `https://proxy.scrapeops.io/v1/?api_key=${API_KEY}&url=${encodeURIComponent(targetUrl)}`;
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
| `aggregateRating` | object | Object containing ratingValue, reviewCount, bestRating, and worstRating |
| `appliedFilters` | array | Array of objects containing filterName and filterValue |
| `bannerImage` | string | URL string for the category header image |
| `breadcrumbs` | array | Array of objects with name and url for site navigation |
| `categoryId` | string | Unique slug or ID for the category |
| `categoryName` | string | Human-readable name of the category |
| `categoryUrl` | string | Absolute URL of the category page |
| `description` | string | Long-form text describing the category and its products |
| `features` | array | List of highlighted category features |
| `images` | array | Collection of gallery or promotional images |
| `pagination` | object | Object containing current page and total results info |
| `products` | array | Array of objects representing individual product listings |
| `reviews` | array | List of user reviews associated with the category |
| `seller` | object | Object containing nested seller/merchant information |
| `serialNumbers` | array | List of related serial numbers or identifiers |
| `specifications` | array | Technical specifications relevant to the category |
| `subcategories` | array | Array of objects for child categories |
| `videos` | array | List of related video URLs or metadata |

### Field Descriptions

- **aggregateRating** (object): Contains nested data regarding user satisfaction and review volume.
- **appliedFilters** (array): Contains a list of object items representing active search refinements.
- **bannerImage** (string): The primary hero image URL for the category page.
- **breadcrumbs** (array): Navigation path leading from the home page to the current category.
- **categoryId** (string): The internal identifier used by Stockx (e.g., "electronics").
- **categoryName** (string): The display title of the page.
- **categoryUrl** (string): The canonical URL of the scraped page.
- **description** (string): SEO text and marketing copy found on the category page.
- **features** (array): Key highlights or selling points extracted from the page.
- **images** (array): A list of supplemental image assets.
- **pagination** (object): Metadata regarding the number of products and pages available.
- **products** (array): The core list of product items appearing on the grid.
- **reviews** (array): User-submitted feedback entries.
- **seller** (object): Information about featured or top-rated sellers in this category.
- **serialNumbers** (array): Specific product identifiers if listed at the category level.
- **specifications** (array): Technical requirements or attributes (common in Electronics).
- **subcategories** (array): Links and names of more specific niche categories.
- **videos** (array): Links to promotional or review videos.

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

This scraper utilizes **Node.js** with **Axios** for HTTP requests and **Cheerio** for HTML parsing. 

1. **Request Initiation**: The script sends a GET request to the target Stockx category URL. To bypass basic bot detection, it routes requests through the ScrapeOps Proxy API.
2. **HTML Parsing**: Once the HTML content is retrieved, Cheerio loads the DOM, allowing for jQuery-like selection of elements.
3. **Data Extraction**: It targets specific CSS selectors to extract category metadata, product grids, and pagination details. It includes helper functions like `stripHTML` to clean text and `detectCurrency` to normalize pricing data.
4. **Data Pipeline**: Extracted items are passed through a `DataPipeline` class which handles de-duplication (using `productId`) and streams the data into a `.jsonl` file to ensure memory efficiency.
5. **Concurrency**: The script is configured to handle requests sequentially or concurrently based on the `CONFIG.maxConcurrency` setting.

## Error Handling & Troubleshooting

### Common Issues
- **403 Forbidden**: Usually indicates the proxy is being blocked. Ensure your ScrapeOps API key is active.
- **Selector Mismatch**: Stockx updates their frontend frequently. If fields return `null`, inspect the page to see if class names have changed.
- **Timeouts**: Large category pages may take longer to load; increase the `CONFIG.timeout` value if necessary.

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
- **Scraper Code**: See `scraper/stockx_scraper_product_category_v1.js` for implementation details

## License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using this scraper.