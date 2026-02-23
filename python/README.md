# Stockx Scrapers - Python

[![Python](https://img.shields.io/badge/Python-3.7+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](../LICENSE)
[![ScrapeOps](https://img.shields.io/badge/ScrapeOps-Integrated-orange.svg)](https://scrapeops.io)

Efficiently extract product pricing, market data, and search listings from Stockx using Python-based scrapers built with BeautifulSoup, Playwright, and Selenium.

This repository provides a comprehensive suite of Python scrapers specifically designed for Stockx.com, featuring multiple framework implementations (BeautifulSoup for speed, Playwright/Selenium for dynamic content) to extract high-quality sneaker and streetwear data while navigating modern web protections.

## 📊 What Data You Can Scrape

These Python scrapers extract data from Stockx.com:

*   **Product Data**: Detailed extraction of individual item specs, including retail price, release date, style codes, and current market asks/bids.
*   **Product Search**: Scrape search result pages to gather lists of products, thumbnails, and quick-view pricing based on specific queries.
*   **Product Category**: Systematically crawl category pages (Sneakers, Apparel, Electronics) to build large-scale datasets of available inventory.

## 📁 Scraper Structure

Each scraper type in the Stockx repository follows this structure:

```
{framework}/
├── product_data/
│   ├── scraper/
│   │   └── stockx_scraper_product_v1.py
│   ├── example/
│   │   └── product.json
│   └── README.md
├── product_search/
│   ├── scraper/
│   │   └── stockx_scraper_product_search_v1.py
│   ├── example/
│   │   └── product_search.json
│   └── README.md
├── product_category/
│   ├── scraper/
│   │   └── stockx_scraper_product_category_v1.py
│   ├── example/
│   │   └── product_category.json
│   └── README.md
├── reviews/          # Coming soon
└── sellers/          # Coming soon
```

Each scraper directory contains:
- `scraper/` - Implementation files
- `example/` - Sample JSON output files
- `README.md` - Detailed documentation for that scraper

## 🚀 Features

- **Multiple Framework Support**: BeautifulSoup, Playwright, Selenium
- **Production-Ready**: Battle-tested scrapers with error handling and retry logic
- **Anti-Bot Protection**: Optional ScrapeOps support that may help with proxy rotation and request optimization
- **Comprehensive Data Extraction**: Product data, search results, and category listings
- **JSONL Output Format**: Efficient, line-by-line JSON output for easy processing
- **Well-Documented**: Detailed READMEs for each scraper with examples and troubleshooting
- **Active Maintenance**: Regular updates to handle Stockx's changing HTML structure

## 📋 Requirements

- **Python**: Python 3.7 or higher
- **pip**: pip
- **ScrapeOps API Key**: Free tier available at [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)
- **Virtual Environment** (recommended): For dependency isolation

## 🎯 Quick Start

1. **Choose a framework** based on your needs (see comparison below)
2. **Navigate to the framework directory** and follow its README for setup
3. **Get your ScrapeOps API key** from [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

For framework-specific setup and usage, see:
- [BeautifulSoup](./BeautifulSoup/README.md)
- [Playwright](./playwright/README.md)
- [Selenium](./selenium/README.md)

## 📚 Supported Frameworks

| Framework | Type | Best For | Performance | Complexity | JavaScript Support |
|-----------|------|----------|-------------|------------|-------------------|
| BeautifulSoup | HTTP Library | Fast, static data extraction | Very High | Easy | No |
| Playwright | Browser Automation | Modern SPAs, complex interactions | Medium | Medium | Yes |
| Selenium | Browser Automation | Legacy support, visual testing | Low | Medium | Yes |

### Framework Documentation

- **BeautifulSoup**: [README](./BeautifulSoup/README.md) | [Official Docs](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- **Playwright**: [README](./playwright/README.md) | [Official Docs](https://playwright.dev/)
- **Selenium**: [README](./selenium/README.md) | [Official Docs](https://www.selenium.dev/documentation/)

## 🛡️ Anti-Bot Protection

All scrapers can integrate with **ScrapeOps** to help handle Stockx's anti-bot measures:

- **Proxy Rotation**: May help distribute requests across multiple IP addresses
- **Request Header Optimization**: May optimize headers to reduce detection
- **Rate Limiting Management**: Built-in rate limiting and retry logic

**Note**: Anti-bot measures vary by site and may change over time. CAPTCHA challenges may occur and cannot be guaranteed to be resolved automatically. Using proxies and browser automation can help reduce blocking, but effectiveness depends on the target site's specific anti-bot measures.

**Free Tier Available**: ScrapeOps offers a generous free tier perfect for testing and small-scale scraping.

Get your API key at [https://scrapeops.io/app/register/ai-scraper](https://scrapeops.io/app/register/ai-scraper)

## 📦 Output Format

All scrapers output data in **JSONL format** (one JSON object per line):

- **Efficient**: Each line is a complete JSON object
- **Streamable**: Process line-by-line without loading entire file
- **Database-Friendly**: Easy to import into databases
- **Large Dataset Support**: Handles millions of records efficiently

Example output file: `stockx_com_product_page_scraper_data_20260114_120000.jsonl`

## 🤔 Choosing the Right Framework

*   **Use BeautifulSoup** when you need maximum speed and the data is available in the initial HTML source. It is the most cost-effective solution for high-volume scraping of static content.
*   **Use Playwright** for Stockx pages that require JavaScript execution to render pricing or product variations. It is faster and more modern than Selenium, making it the preferred choice for browser automation.
*   **Use Selenium** if you have existing infrastructure built around it or require specific legacy browser drivers.
*   **Performance vs. Capability**: HTTP-based scrapers (BeautifulSoup) are significantly faster but cannot interact with buttons or wait for JS-driven content. Browser-based scrapers (Playwright/Selenium) handle complex UI but consume more CPU and RAM.

## ⚠️ Common Issues & Solutions

*   **403 Forbidden Errors**: Often caused by Stockx's anti-bot system. **Solution**: Integrate ScrapeOps proxy rotation and use realistic User-Agent headers.
*   **Missing Selectors**: Stockx frequently updates class names. **Solution**: Check the latest HTML structure and update the selectors in the `scraper/` directory.
*   **Slow Browser Launch**: Playwright/Selenium can be slow to initialize. **Solution**: Use headless mode and reuse browser contexts where possible.
*   **Dynamic Pricing Not Loading**: Pricing sometimes loads after the initial page hit. **Solution**: Use Playwright's `wait_for_selector` or `wait_until="networkidle"`.
*   **Dependency Conflicts**: Different frameworks requiring different versions of libraries. **Solution**: Always use a Python Virtual Environment (`venv`).

## 🔗 Alternative Implementations

This repository also provides Node.js implementations:

- [Node.js Scrapers](../node/README.md)

## 📖 Best Practices

1. **Use Virtual Environments**: Isolate dependencies per project
2. **Respect Rate Limits**: Use appropriate delays and concurrency settings
3. **Monitor ScrapeOps Usage**: Track your API usage in the ScrapeOps dashboard
4. **Handle Errors Gracefully**: Implement proper error handling and logging
5. **Validate URLs**: Ensure URLs are valid Stockx pages before scraping
6. **Update Selectors Regularly**: Stockx may change HTML structure
7. **Test Regularly**: Test scrapers regularly to catch breaking changes early
8. **Handle Missing Data**: Some products may not have all fields; handle null values appropriately
9. **Browser Management**: For browser automation, ensure proper cleanup and resource management
10. **Use JSONL Format**: Efficient for large datasets and streaming processing

## 📚 Resources & Documentation

### Framework Documentation
- **BeautifulSoup**: [Official Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- **Playwright**: [Python Documentation](https://playwright.dev/python/docs/intro)
- **Selenium**: [Python Documentation](https://www.selenium.dev/documentation/webdriver/languages/python/)

### External Resources
- **ScrapeOps Documentation**: [https://scrapeops.io/docs/intro/](https://scrapeops.io/docs/intro/)
- **Python Documentation**: https://docs.python.org/3/

### Project Resources
- **Root README**: [../README.md](../README.md) - Overview of all implementations
- **Framework READMEs**: See individual framework directories for specific guides
- **Scraper READMEs**: See individual scraper directories for detailed documentation

## ⚖️ License

This scraper is provided as-is for educational and commercial use. Please ensure compliance with Stockx's Terms of Service and robots.txt when using these scrapers.

See [LICENSE](../LICENSE) for full license details.

## ⚠️ Disclaimer

This software is provided for educational and commercial purposes. Users are responsible for ensuring their use complies with:

- Stockx's Terms of Service
- Stockx's robots.txt
- Applicable laws and regulations
- Rate limiting and respectful scraping practices

The authors and contributors are not responsible for any misuse of this software.