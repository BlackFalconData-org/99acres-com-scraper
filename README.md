# 99acres Scraper - India Property Listings

Extract structured data from [99acres.com](https://99acres.com) — 99acres.com property listings to buy, rent or resale in any Indian city, or paste a search URL. Every record carries the price, area and locality the site publishes. Filter by distance from a point. Structured rows, ready for a spreadsheet or a CRM.

**[99acres Scraper - India Property Listings on Apify →](https://apify.com/blackfalcondata/99acres-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 🏷️ sale type, description format, and more.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N listings (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from 99acres.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from 99acres.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "buy",
  "location": "Mumbai",
  "maxResults": 100,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | The kind of listings to collect - a listing type, not a free-text keyword. One of: `buy` (default), `rent`, `resale`, `new-booking`, `pg`, `commercial`, `commercial-rent`. You can also paste a full 99acres search URL here with all its filters applied. Required if no Start URLs are provided. Use a JSON array for several searches in one run, e.g. ["buy","rent"]. |
| `location` | string | — | City to search, e.g. Mumbai, Bangalore, Pune, Delhi. Defaults to Mumbai. Use a JSON array for several cities in one run, e.g. ["Mumbai","Pune"]. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste 99acres.com pages: a search page (a city search such as https://www.99acres.com/property-in-mumbai-ffid for buy, rent, resale, PG or commercial; filters in the URL are kept), the "properties for sale" or "properties for rent" listings page of a project (the link ends in -npffid or -rnpffid), a project page (its own "for sale" and "for rent" listings are scraped), or a single property page. Each URL becomes its own task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the query/location fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `bedrooms` | array | — | Keep only listings with these bedroom counts, e.g. ["2","3"]. Use "4+" (any N from 1 to 9 followed by +) for N or more. Empty = any. Listings that do not state a bedroom count are left out when this is set. |
| `propertyTypes` | array | — | Keep only these property types. Empty = any. |
| `minPrice` | integer | — | Minimum price in Indian rupees (rent per month for rent searches). Listings without a price are left out when a price limit is set. |
| `maxPrice` | integer | — | Maximum price in Indian rupees (rent per month for rent searches). |
| `minArea` | integer | — | Minimum area in square feet (the largest of carpet, built-up and super built-up area the listing states). Listings with no area in square feet are left out when an area limit is set. |
| `maxArea` | integer | — | Maximum area in square feet. |
| `saleType` | enum | `"all"` | For buy searches: all listings, only resale, or only new booking. New-project cards (includeProjects) carry no sale type and are not excluded by this filter. |
| `includeProjects` | boolean | `false` | Also return the new-project cards shown on the search page (builder, price range, possession status, location). They are marked resultType "project" and count toward Max Results. Off by default: only individual listings are returned. |
| `enrichEmails` | boolean | `false` | Look for a public contact email on the website of each advertiser or builder. Billed per contact email found; rows where no email is found are free. Adds time to the run. Private owners are skipped. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. 99acres serves the listing description as plain text only, so `all` and `text` both keep it. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `lat` | number | — | Latitude of the centre point for the radius filter. Needs lon and radiusKm; without all three no radius filter applies. The search itself still runs per city; the radius then keeps only listings near this point. |
| `lon` | number | — | Longitude of the centre point for the radius filter. Needs lat and radiusKm. |
| `radiusKm` | integer | — | Keep only listings within this many kilometres of the lat/lon point. Listings without coordinates cannot be placed and are left out. Needs lat and lon; empty means no radius filter. |

---

## Output fields

Every listing returns the same 66-field schema. Missing values are `null` — never omitted.

- `listingId`
- `id`
- `propId`
- `url`
- `portalUrl`
- `title`
- `listingType`
- `propertyType`
- `price`
- `maxPrice`
- `priceText`
- `priceInWords`
- `currency`
- `pricePerSqft`
- `bhk`
- `bedrooms`
- `bathrooms`
- `balconies`
- `areaSqft`
- `areaText`
- `areaType`
- `carpetAreaSqft`
- `furnishing`
- `furnishingAttributes`
- `facing`
- `overlooking`
- `availability`
- `floor`
- `totalFloors`
- `ageYears`
- `ownership`
- `gated`
- `powerBackup`
- `description`
- `highlights`
- `tags`
- `society`
- `projectName`
- `locality`
- `city`
- `address`
- `latitude`
- `longitude`
- `postedBy`
- `contactName`
- `companyName`
- `sellerMemberSince`
- `verified`
- `reraRegistered`
- `reraId`
- `imageUrl`
- `images`
- `imagesCount`
- `videos`
- `postedAt`
- `updatedAt`
- `expiresAt`
- `searchUrl`
- `resultPosition`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "listingId": "3cbb17c05dfd468a9e176374d65a3a3828fafa565ffac2670e64ef11247f5bbc",
  "id": "93883836",
  "propId": "L93883836",
  "url": "https://www.99acres.com/3-bhk-bedroom-apartment-flat-for-sale-in-oberoi-exquisite-goregaon-east-mumbai-andheri-dahisar-1820-sqft-spid-L93883836",
  "portalUrl": "https://www.99acres.com/3-bhk-bedroom-apartment-flat-for-sale-in-oberoi-exquisite-goregaon-east-mumbai-andheri-dahisar-1820-sqft-spid-L93883836",
  "title": "3 BHK Flat in Goregaon East, Mumbai",
  "listingType": "sale",
  "propertyType": "Residential Apartment",
  "price": 70500000,
  "maxPrice": 70500000,
  "priceText": "7.05 Cr",
  "priceInWords": "Rs. 7.05 Crore"
}
```

*Truncated — full records contain 66 fields. See Output fields for the complete schema.*

**[Try 99acres Scraper - India Property Listings now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/99acres-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.00162 |

See the [actor on Apify](https://apify.com/blackfalcondata/99acres-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape 99acres.com?**
Use this actor on Apify to extract structured data from 99acres.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get 99acres.com data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape 99acres.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check 99acres.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [99acres Scraper - India Property Listings](https://apify.com/blackfalcondata/99acres-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
