[Google Maps Email Extractor](https://apify.com/dltik/google-maps-email-extractor?fpr=data)

# Google Maps Email Extractor — Business Leads with Emails & WhatsApp

Extract business emails, phones, **WhatsApp availability**, social profiles, and ratings from Google Maps. Enter a keyword + location, get a complete lead list with **verified emails** and **lead scoring** — all in one run.

No separate email finder needed. No chaining multiple actors. One input, one output, done.

> **No proxy needed.** This actor works without residential proxies — even on the free Apify plan. Built-in anti-detection bypasses Google's bot protection natively.

 

> ⭐ **Found this useful?** Click the **Bookmark** button at the top of [this page](https://apify.com/dltik/google-maps-email-extractor) — it helps the scraper stay visible to others who need it. Takes 1 click. No signup beyond your existing Apify account.

---

## Why this actor?

| Feature | This actor | lukaskrivka ($5/1K) | compass ($4/1K) |
| --- | --- | --- | --- |
| Google Maps data | Yes | Yes | Yes |
| Email extraction | Yes (6 techniques) | Yes | No |
| **MX email validation** | **Yes** | No | No |
| **WhatsApp check** | **Yes** | No | No |
| Social profiles | 6 platforms | Limited | No |
| Lead scoring | A/B/C/D/F | No | No |
| Price per 1,000 | **$3** | $5 | $4 (no emails) |

---

## What data can you extract?

| Field | Source | Description |
| --- | --- | --- |
| `name` | Google Maps | Business name |
| `address` | Google Maps | Full street address |
| `phone` | Google Maps | Phone number |
| `website` | Google Maps | Business website URL |
| `rating` | Google Maps | Google rating (0-5 stars) |
| `reviews_count` | Google Maps | Number of Google reviews |
| `category` | Google Maps | Business category (e.g. "Dentist", "Restaurant") |
| `latitude`, `longitude` | Google Maps | GPS coordinates |
| `google_maps_url` | Google Maps | Direct link to listing |
| `primary_email` | Website crawl | Best contact email (info@, contact@, hello@) |
| `emails` | Website crawl | All valid emails found (max 3, ranked) |
| `email_source` | Website crawl | How found: mailto, schema.org, regex, contact_page |
| `email_quality` | Computed | `high`, `medium`, `low`, or `none` |
| `email_verified` | DNS check | `true` if domain has valid MX records (can receive mail) |
| `has_whatsapp` | wa.me check | `true` if phone number has WhatsApp |
| `socials` | Website crawl | Facebook, Instagram, LinkedIn, X, YouTube, TikTok |
| `lead_score` | Computed | A/B/C/D/F quality grade |
| `guessed_emails` | Generated | info@, contact@ candidates if nothing found |

---

## Quick start

### Basic search

```
{
  "queries": ["plumber Paris", "dentist London"],
  "maxResultsPerQuery": 20
}
```

### Full enrichment (emails + MX validation + WhatsApp)

```
{
  "queries": ["restaurant Berlin Mitte"],
  "maxResultsPerQuery": 50,
  "extractEmails": true,
  "emailDepth": "deep",
  "validateEmails": true,
  "checkWhatsApp": true
}
```

### Fast mode (no emails, just Google Maps data)

```
{
  "queries": ["gym New York"],
  "maxResultsPerQuery": 100,
  "extractEmails": false
}
```

---

## Input

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `queries` | array | *required* | Search queries (e.g. `["plumber Paris", "dentist London"]`) |
| `maxResultsPerQuery` | integer | `20` | Max businesses per query (1-200) |
| `extractEmails` | boolean | `true` | Crawl websites for emails |
| `extractSocials` | boolean | `true` | Extract social media profiles |
| `emailDepth` | string | `"deep"` | `shallow` = homepage only, `deep` = + contact pages |
| `validateEmails` | boolean | `false` | MX record check — verify email domains can receive mail |
| `checkWhatsApp` | boolean | `false` | Check if phone numbers have WhatsApp |
| `language` | string | `"en"` | Google Maps language (ISO 639-1) |
| `proxyConfig` | object | — | Proxy settings (residential recommended for production) |

---

## Output example

```
{
  "name": "Kottident - Dentist in Berlin Kreuzberg",
  "address": "Adalbertstraße 94, 10999 Berlin, Germany",
  "phone": "+49 30 3911155",
  "website": "https://www.kottident.de/",
  "rating": 4.8,
  "reviews_count": 412,
  "category": "Dentist",
  "latitude": 52.5012,
  "longitude": 13.4189,
  "google_maps_url": "https://www.google.com/maps/place/Kottident/...",
  "place_id": "0x47a84e37...",
  "primary_email": "info@kottident.de",
  "emails": ["info@kottident.de"],
  "email_source": "mailto",
  "email_quality": "high",
  "email_verified": true,
  "has_whatsapp": true,
  "guessed_emails": [],
  "socials": {
    "instagram": "https://instagram.com/kottident",
    "facebook": "https://facebook.com/kottident"
  },
  "lead_score": "A",
  "query": "dentist Berlin"
}
```

---

## Lead scoring

| Score | Meaning | Criteria |
| --- | --- | --- |
| **A** | Gold lead | Email + phone + website |
| **B** | Good lead | Email + website OR phone + website |
| **C** | Basic lead | Website only |
| **D** | Minimal | Phone only, no website |
| **F** | No contact | Nothing found |

---

## MX email validation

When `validateEmails: true`, the actor checks each email domain's DNS MX records to verify it can receive mail. This filters out:

- Defunct domains
- Parked/expired domains
- Typo domains

Adds `email_verified: true/false` to each result. Runs in parallel — adds less than 1 second total.

---

## WhatsApp check

When `checkWhatsApp: true`, the actor checks each phone number against WhatsApp's wa.me service. Useful for:

- B2B outreach via WhatsApp Business
- Identifying businesses that accept WhatsApp messages
- Prioritizing leads by communication channel

Adds `has_whatsapp: true/false` to each result. Runs in parallel — adds less than 1 second total.

---

## Email extraction techniques

6 techniques applied in order of reliability:

1. **mailto: links** — explicitly published contact emails
2. **Schema.org / JSON-LD** — structured data from the page
3. **Regex scan** — pattern matching, filtered against 30+ blacklisted domains
4. **Obfuscation detection** — finds `contact [at] domain [dot] com` patterns
5. **Contact page crawling** — visits /contact, /about, /impressum (deep mode)
6. **Pattern guessing** — generates info@, contact@ as last resort

---

## Pricing

**$0.003 per business** = $3 per 1,000 leads (emails + WhatsApp included).

| Scenario | Leads | Cost | Time |
| --- | --- | --- | --- |
| Quick test | 5 | ~$0.02 | ~30s |
| Small batch | 20 | ~$0.06 | ~2min |
| Standard | 50 | ~$0.15 | ~4min |
| Large batch | 200 | ~$0.60 | ~15min |

MX validation and WhatsApp check add negligible cost (< 1s of compute).

---

## API integration

**Python:**

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")

run = client.actor("dltik/google-maps-email-extractor").call(run_input={
    "queries": ["plumber Paris", "electrician Lyon"],
    "maxResultsPerQuery": 50,
    "extractEmails": True,
    "validateEmails": True,
    "checkWhatsApp": True,
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    if item.get("lead_score") == "A" and item.get("has_whatsapp"):
        print(f"{item['name']} — {item['primary_email']} — WhatsApp: {item['phone']}")
```

**curl:**

```
curl -X POST "https://api.apify.com/v2/acts/dltik~google-maps-email-extractor/runs" \
  -H "Authorization: Bearer YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "queries": ["dentist Berlin"],
    "maxResultsPerQuery": 20,
    "validateEmails": true,
    "checkWhatsApp": true
  }'
```

---

## FAQ

**Is scraping Google Maps legal?**

This actor collects only publicly available business information. Always comply with local data protection regulations (GDPR, CCPA) and anti-spam laws.

**Why do some businesses have no emails?**

Not all businesses publish email addresses. Some use contact forms instead. The actor tries 6 techniques and provides `guessed_emails` as fallback.

**What's the difference between shallow and deep?**
`shallow` scans the homepage only. `deep` also visits /contact, /about, and /impressum pages — finds 30-50% more emails but takes longer.

**Do I need proxies?**

The actor works without proxies for small runs. For production (10+ queries), residential proxies are recommended to avoid Google rate limiting.

**How accurate is the WhatsApp check?**

It checks if the number is registered with WhatsApp via the wa.me redirect service. Accuracy is ~95% for international numbers.

**How accurate is MX validation?**

MX validation confirms the email domain can receive mail. It does not verify the specific mailbox exists — for that you'd need SMTP verification (not included).

---

## Connect with Make, Zapier & n8n

This actor integrates with any automation platform via the Apify API.

### Make (Integromat)

1. Add an **Apify module** in your Make scenario
2. Select **Run Actor** and choose this actor
3. Configure the input (paste your JSON)
4. Add a **Get Dataset Items** module to retrieve results
5. Connect to Google Sheets, HubSpot, Slack, or any other app

### Zapier

1. Use the **Apify integration** on Zapier
2. Set trigger: **Actor Run Finished**
3. Action: **Get Dataset Items**
4. Send results to your CRM, email tool, or spreadsheet

### n8n

1. Add an **HTTP Request** node to call the Apify API
2. POST to `https://api.apify.com/v2/acts/dltik~google-maps-email-extractor/runs`
3. Wait for completion, then fetch dataset items
4. Route results to any n8n node

### Webhooks

Set up a webhook to get notified when a run finishes:

```
run = client.actor("dltik/google-maps-email-extractor").call(
    run_input={...},
    webhooks=[{
        "eventTypes": ["ACTOR.RUN.SUCCEEDED"],
        "requestUrl": "https://your-webhook-url.com"
    }]
)
```

---

## Other scrapers by dltik

| Actor | What it does | Price |
| --- | --- | --- |
| [Facebook Ads Scraper](https://apify.com/dltik/facebook-ads-scraper) | Scrape Meta Ad Library — ad copy, creatives, CTA links | $1/1K |
| [TikTok Scraper](https://apify.com/dltik/tiktok-scraper) | Scrape profiles, videos, hashtags, search, trending | $1/1K |
| [TikTok Video Downloader](https://apify.com/dltik/tiktok-video-downloader) | Download TikTok videos without watermark | $5/1K |
| [Reddit Scraper](https://apify.com/dltik/reddit-scraper) | Scrape posts, comments, profiles with sentiment analysis | $2/1K |
| [Trustpilot Scraper](https://apify.com/dltik/trustpilot-scraper) | Scrape reviews, ratings, company profiles with sentiment | $0.50/1K |