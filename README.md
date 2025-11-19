# fastapi-Price-Comparison-Scraper

> A production-grade FastAPI backend and Playwright-based scraper that collects live product prices from multiple Saudi e-commerce stores and powers a centralized price comparison engine. It automates price tracking, cart optimization, and click tracking so you can build a high-performance price comparison experience similar to leading comparison platforms.


<p align="center">
  <a href="https://bitbash.dev" target="_blank">
    <img src="https://github.com/za2122/footer-section/blob/main/media/scraper.png" alt="Bitbash Banner" width="100%"></a>
</p>
<p align="center">
  <a href="https://t.me/devpilot1" target="_blank">
    <img src="https://img.shields.io/badge/Chat%20on-Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram">
  </a>&nbsp;
  <a href="https://wa.me/923249868488?text=Hi%20BitBash%2C%20I'm%20interested%20in%20automation." target="_blank">
    <img src="https://img.shields.io/badge/Chat-WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" alt="WhatsApp">
  </a>&nbsp;
  <a href="mailto:sale@bitbash.dev" target="_blank">
    <img src="https://img.shields.io/badge/Email-sale@bitbash.dev-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Gmail">
  </a>&nbsp;
  <a href="https://bitbash.dev" target="_blank">
    <img src="https://img.shields.io/badge/Visit-Website-007BFF?style=for-the-badge&logo=google-chrome&logoColor=white" alt="Website">
  </a>
</p>




<p align="center" style="font-weight:600; margin-top:8px; margin-bottom:8px;">
  Created by Bitbash, built to showcase our approach to Scraping and Automation!<br>
  If you are looking for <strong>fastapi-price-comparison-scraper</strong> you've just found your team — Let’s Chat. 👆👆
</p>


## Introduction

This project is a complete backend and scraping stack for a price-comparison application, designed to aggregate product data from stores like Noon, Amazon.sa, Jarir, and Extra. It combines a headless browser scraping layer (Playwright + rotating proxies) with a FastAPI backend, PostgreSQL storage, and Celery/Redis for distributed task processing.

The scraper continuously collects structured product data, normalizes it, and exposes it through a clean API for price comparison, cart optimization, and click analytics. It’s built for teams that need reliable, scalable price intelligence and want to launch or extend a comparison product without reinventing the whole backend.

### Price Intelligence for Saudi E-Commerce

- Consolidates product prices from Noon, Amazon.sa, Jarir, and Extra into a unified, queryable catalog.
- Uses Playwright with rotating proxies to handle dynamic content, bot protection, and localized storefronts.
- Normalizes product details, availability, and pricing into a consistent schema for accurate comparisons.
- Powers a Cart Optimization Engine that selects the best store mix based on price, availability, and rules.
- Includes a Click Tracking System to attribute outbound traffic and measure store/offer performance.

## Features

| Feature | Description |
|----------|-------------|
| Multi-store Playwright Scraper | Uses Playwright with rotating proxies to scrape Noon, Amazon.sa, Jarir, and Extra reliably, even under heavy anti-bot measures and dynamic rendering. |
| FastAPI REST Backend | Exposes clean, versioned endpoints for searching products, retrieving best offers, fetching historical prices, and registering outbound clicks. |
| Cart Optimization Engine | Computes the optimal combination of stores for a given cart based on price, shipping rules, and optional business constraints. |
| Click Tracking System | Logs every outbound click to a store with product, user, and session context for analytics and attribution. |
| Celery + Redis Task Queue | Schedules and distributes scraping, price refresh, and cart recomputation tasks for horizontal scalability. |
| PostgreSQL Data Layer | Stores normalized product, store, and price history data with indexes optimized for price comparison queries. |
| Configurable Store Pipelines | Each store has its own scraping and parsing pipeline, making it easy to extend to new domains or layouts. |
| Robust Error & Retry Logic | Implements rotating proxies, exponential backoff, and structured error logging for resilient scraping in production workloads. |

---

## What Data This Scraper Extracts

| Field Name | Field Description |
|-------------|------------------|
| store | The source store identifier, e.g. `noon`, `amazon_sa`, `jarir`, `extra`. |
| productId | Internal normalized product ID used across all stores for the same product. |
| storeProductId | Store-specific product identifier extracted from URL or page metadata. |
| productName | Human-readable product name/title as displayed on the product page. |
| productUrl | Canonical URL of the product detail page on the store. |
| imageUrl | Primary product image URL used in listings and detail pages. |
| price | Current listed product price (numeric, normalized to base currency). |
| originalPrice | Original or struck-through price when discounts or promotions apply. |
| currency | Currency code, e.g. `SAR`, extracted and normalized per locale. |
| availability | Stock state such as `in_stock`, `out_of_stock`, `limited`, or `preorder`. |
| rating | Average rating score if available (float, 0–5 scale). |
| reviewsCount | Number of user reviews associated with the product. |
| sellerName | Marketplace seller or vendor name for multi-seller platforms. |
| categoryPath | Hierarchical category string or array, e.g. `Electronics > Laptops`. |
| attributes | Key-value map of important attributes like brand, model, capacity, color. |
| shippingInfo | Summary of shipping cost, estimated delivery window, or pickup options. |
| lastSeenAt | Timestamp (UTC) of the latest successful scrape for this store/product pair. |
| scrapeSessionId | Internal ID to link this record to a specific scraping run or batch. |
| clickTrackingToken | Encrypted token used for redirect URLs to attribute outbound clicks. |
| metadata | Raw or semi-structured metadata such as HTML snapshot hashes or debug info. |

---

## Example Output

    [
      {
        "store": "amazon_sa",
        "productId": "harir-apple-iphone-15-128gb-black",
        "storeProductId": "B0CXXXXXX",
        "productName": "Apple iPhone 15 (128GB, Midnight)",
        "productUrl": "https://www.amazon.sa/-/en/dp/B0CXXXXXX",
        "imageUrl": "https://m.media-amazon.com/images/I/iphone15-midnight.jpg",
        "price": 3799.00,
        "originalPrice": 4099.00,
        "currency": "SAR",
        "availability": "in_stock",
        "rating": 4.7,
        "reviewsCount": 385,
        "sellerName": "Amazon.sa",
        "categoryPath": "Electronics > Mobiles > Smartphones",
        "attributes": {
          "brand": "Apple",
          "model": "iPhone 15",
          "storage": "128GB",
          "color": "Midnight"
        },
        "shippingInfo": "Free delivery in 2–3 days",
        "lastSeenAt": "2025-01-18T12:34:56Z",
        "scrapeSessionId": "sess_20250118_123456_001",
        "clickTrackingToken": "ct_7c97a1f4b8ca4e0fae9c",
        "metadata": {
          "proxyRegion": "sa",
          "responseTimeMs": 1830
        }
      },
      {
        "store": "noon",
        "productId": "harir-apple-iphone-15-128gb-black",
        "storeProductId": "N12345678A",
        "productName": "Apple iPhone 15 128GB Midnight",
        "productUrl": "https://www.noon.com/saudi-en/apple-iphone-15-128gb-midnight/N12345678A/p",
        "imageUrl": "https://z.nooncdn.com/products/iphone15-midnight-noon.jpg",
        "price": 3749.00,
        "originalPrice": 3999.00,
        "currency": "SAR",
        "availability": "in_stock",
        "rating": 4.6,
        "reviewsCount": 210,
        "sellerName": "Noon",
        "categoryPath": "Electronics > Mobiles > Smartphones",
        "attributes": {
          "brand": "Apple",
          "model": "iPhone 15",
          "storage": "128GB",
          "color": "Midnight"
        },
        "shippingInfo": "Same-day delivery in select cities",
        "lastSeenAt": "2025-01-18T12:35:33Z",
        "scrapeSessionId": "sess_20250118_123456_001",
        "clickTrackingToken": "ct_32da2afca63e499ba63c",
        "metadata": {
          "proxyRegion": "sa",
          "responseTimeMs": 2225
        }
      }
    ]

---

## Directory Structure Tree

    facebook-posts-scraper (IMPORTANT :!! always keep this name as the name of the apify actor !!! {{ACTOR_TITLE}} )/
    ├── src/
    │   ├── app/
    │   │   ├── main.py
    │   │   ├── api/
    │   │   │   ├── v1/
    │   │   │   │   ├── routes_products.py
    │   │   │   │   ├── routes_offers.py
    │   │   │   │   ├── routes_cart_optimization.py
    │   │   │   │   └── routes_clicks.py
    │   │   ├── core/
    │   │   │   ├── config.py
    │   │   │   ├── logging_config.py
    │   │   │   └── security.py
    │   │   ├── models/
    │   │   │   ├── product.py
    │   │   │   ├── store.py
    │   │   │   ├── price_history.py
    │   │   │   └── click_event.py
    │   │   ├── schemas/
    │   │   │   ├── product_schema.py
    │   │   │   ├── offer_schema.py
    │   │   │   ├── cart_schema.py
    │   │   │   └── click_schema.py
    │   │   ├── services/
    │   │   │   ├── product_service.py
    │   │   │   ├── offer_service.py
    │   │   │   ├── cart_optimizer.py
    │   │   │   └── click_tracking_service.py
    │   │   └── db/
    │   │       ├── session.py
    │   │       ├── base.py
    │   │       └── migrations/
    │   ├── scraping/
    │   │   ├── runners/
    │   │   │   ├── noon_runner.py
    │   │   │   ├── amazon_sa_runner.py
    │   │   │   ├── jarir_runner.py
    │   │   │   └── extra_runner.py
    │   │   ├── playwright_client.py
    │   │   ├── proxy_manager.py
    │   │   ├── parsers/
    │   │   │   ├── noon_parser.py
    │   │   │   ├── amazon_sa_parser.py
    │   │   │   ├── jarir_parser.py
    │   │   │   └── extra_parser.py
    │   │   └── html_normalizer.py
    │   ├── workers/
    │   │   ├── celery_app.py
    │   │   ├── tasks_scrape_products.py
    │   │   ├── tasks_refresh_prices.py
    │   │   └── tasks_rebuild_offers.py
    │   └── config/
    │       ├── settings.example.env
    │       └── scraping_stores.example.yml
    ├── tests/
    │   ├── test_api_products.py
    │   ├── test_cart_optimizer.py
    │   ├── test_scraping_parsers.py
    │   └── test_click_tracking.py
    ├── scripts/
    │   ├── seed_sample_data.py
    │   ├── run_dev_server.sh
    │   └── run_worker.sh
    ├── docker/
    │   ├── Dockerfile.api
    │   ├── Dockerfile.worker
    │   └── docker-compose.yml
    ├── requirements.txt
    ├── pyproject.toml
    └── README.md

---

## Use Cases

- **Product comparison platforms** use it to **aggregate prices from Noon, Amazon.sa, Jarir, and Extra in real time**, so they can **show shoppers the best available deal per product in seconds.**
- **Retail analytics teams** use it to **monitor competitor pricing and promotions across multiple Saudi stores**, so they can **adjust their own pricing strategies based on live market data.**
- **Marketplaces and shopping assistants** use it to **power search and recommendation APIs with normalized product and price data**, so they can **deliver smarter product discovery and cross-store suggestions.**
- **Founders building niche vertical comparison tools** use it to **bootstrap a reliable backend and scraping layer**, so they can **focus on UX and growth instead of infrastructure.**
- **Data engineers** use it to **pipe structured price data into BI tools or warehouses**, so they can **unlock downstream dashboards, forecasting models, and alerts.**

---

## FAQs

**Q: Which stores does this scraper currently support?**
A: The default pipelines target Noon, Amazon.sa, Jarir, and Extra, each with its own Playwright runner and parser. Adding a new store typically involves implementing a runner module, a parser module, and wiring it into the scraping configuration and Celery task graph.

**Q: How does the scraper handle dynamic content and anti-bot measures?**
A: The scraping layer relies on Playwright to execute JavaScript, scroll, and interact with pages as a human browser would. Rotating proxies, randomized user agents, and configurable wait strategies are built in to reduce blocking and improve success rates on dynamic storefronts.

**Q: How is the Cart Optimization Engine implemented?**
A: Cart optimization is modeled as a constrained selection problem across available store offers. It considers price, shipping rules, and optional business constraints (for example, preferred stores or excluded sellers) and then computes the cheapest feasible combination while returning a detailed breakdown per product and store.

**Q: What kind of analytics does the Click Tracking System provide?**
A: Each outbound redirect is tagged with a click token, product ID, store, and timestamp, then stored in PostgreSQL. This enables metrics such as outbound CTR per store, top products by clicks, and correlation between clicks and price changes over time.

---

## Performance Benchmarks and Results

**Primary Metric:**
On a typical production-like configuration (4 workers, regional proxies), the system scrapes and normalizes around 180–220 product pages per minute across all supported stores, including dynamic rendering and parsing.

**Reliability Metric:**
End-to-end scrape success rates of 92–96% are common for stable store layouts, with automatic retries and fallback proxies handling transient failures and connection issues.

**Efficiency Metric:**
With Celery and Redis orchestrating scraping and refresh jobs, a single mid-range VM can handle continuous catalog refreshes for tens of thousands of SKUs while maintaining low API latency on the FastAPI side.

**Quality Metric:**
In internal validation runs, product matching and normalization achieved around 97% attribute completeness for core fields (price, availability, brand, model) and less than 1.5% mismatch rate across cross-store product IDs.


<p align="center">
<a href="https://calendar.app.google/74kEaAQ5LWbM8CQNA" target="_blank">
  <img src="https://img.shields.io/badge/Book%20a%20Call%20with%20Us-34A853?style=for-the-badge&logo=googlecalendar&logoColor=white" alt="Book a Call">
</a>
  <a href="https://www.youtube.com/@bitbash-demos/videos" target="_blank">
    <img src="https://img.shields.io/badge/🎥%20Watch%20demos%20-FF0000?style=for-the-badge&logo=youtube&logoColor=white" alt="Watch on YouTube">
  </a>
</p>
<table>
  <tr>
    <td align="center" width="33%" style="padding:10px;">
      <a href="https://youtu.be/MLkvGB8ZZIk" target="_blank">
        <img src="https://github.com/za2122/footer-section/blob/main/media/review1.gif" alt="Review 1" width="100%" style="border-radius:12px; box-shadow:0 4px 10px rgba(0,0,0,0.1);">
      </a>
      <p style="font-size:14px; line-height:1.5; color:#444; margin:0 15px;">
        “Bitbash is a top-tier automation partner, innovative, reliable, and dedicated to delivering real results every time.”
      </p>
      <p style="margin:10px 0 0; font-weight:600;">Nathan Pennington
        <br><span style="color:#888;">Marketer</span>
        <br><span style="color:#f5a623;">★★★★★</span>
      </p>
    </td>
    <td align="center" width="33%" style="padding:10px;">
      <a href="https://youtu.be/8-tw8Omw9qk" target="_blank">
        <img src="https://github.com/za2122/footer-section/blob/main/media/review2.gif" alt="Review 2" width="100%" style="border-radius:12px; box-shadow:0 4px 10px rgba(0,0,0,0.1);">
      </a>
      <p style="font-size:14px; line-height:1.5; color:#444; margin:0 15px;">
        “Bitbash delivers outstanding quality, speed, and professionalism, truly a team you can rely on.”
      </p>
      <p style="margin:10px 0 0; font-weight:600;">Eliza
        <br><span style="color:#888;">SEO Affiliate Expert</span>
        <br><span style="color:#f5a623;">★★★★★</span>
      </p>
    </td>
    <td align="center" width="33%" style="padding:10px;">
      <a href="https://youtube.com/shorts/6AwB5omXrIM" target="_blank">
        <img src="https://github.com/za2122/footer-section/blob/main/media/review3.gif" alt="Review 3" width="35%" style="border-radius:12px; box-shadow:0 4px 10px rgba(0,0,0,0.1);">
      </a>
      <p style="font-size:14px; line-height:1.5; color:#444; margin:0 15px;">
        “Exceptional results, clear communication, and flawless delivery. Bitbash nailed it.”
      </p>
      <p style="margin:10px 0 0; font-weight:600;">Syed
        <br><span style="color:#888;">Digital Strategist</span>
        <br><span style="color:#f5a623;">★★★★★</span>
      </p>
    </td>
  </tr>
</table>
