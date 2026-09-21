# Technical & On-Site Implementation for AI Search Visibility

**Scope:** what you actually build and configure on a website so that ChatGPT, Google AI Overviews / AI Mode, Perplexity, Microsoft Copilot and Claude can *crawl it, parse it, retrieve it and cite it*. Written for service businesses, local businesses, shops and product brands.

**Research date:** 2026-09-21. Evidence is tagged:
- **[C] Confirmed** — documented by the platform vendor itself or a reproducible primary measurement.
- **[V] Vendor/industry claim** — asserted by an SEO tool vendor, agency or analyst; directionally useful, not independently verified.
- **[S] Speculative** — widely repeated in the GEO community with no primary source. Treat as a hypothesis.
- **[MYTH]** — repeated as fact but contradicted by evidence.

---

## Priority checklist

**Tier 0 — do these or nothing else matters (hours of work, highest impact)**

1. Confirm AI fetchers get **HTTP 200**, not 403/429/JS-challenge. Test with `curl -A "OAI-SearchBot/1.0"`, `-A "ChatGPT-User/1.0"`, `-A "PerplexityBot/1.0"`, `-A "ClaudeBot/1.0"`. The most common cause of invisibility is a WAF/CDN rule, not robots.txt. **[C]**
2. If you are on Cloudflare: open **AI Crawl Control** and check the Search / Agent / Training toggles. New zones, new sites on existing accounts and free plans have had AI crawlers blocked **by default** since 1 July 2025, with a further tightening for "mixed-use" crawlers on 15 September 2026. **[C]**
3. In `robots.txt`, explicitly **Allow** the *retrieval* bots (`OAI-SearchBot`, `ChatGPT-User`, `PerplexityBot`, `Perplexity-User`, `Claude-User`, `Claude-SearchBot`, `Googlebot`, `Bingbot`, `Applebot`). Decide separately about *training* bots (`GPTBot`, `ClaudeBot`, `Google-Extended`, `Applebot-Extended`, `CCBot`, `Bytespider`, `Meta-ExternalAgent`).
4. **Server-side render the primary content.** Vercel/MERJ measurement across >500M crawler fetches found no AI crawler executing JavaScript; GPTBot fetched JS files ~11.5% of the time and ClaudeBot ~23.8%, but never ran them. Only Google (AI Overviews/AI Mode, via Googlebot) and Microsoft (Copilot, via Bingbot) inherit a rendering pipeline. **[C]**
5. Do **not** ship `nosnippet` or `max-snippet:0`. Those directives — not `Google-Extended` — are what removes you from AI Overviews and AI Mode. **[C]**
6. Keep the page indexable and canonical: one self-referencing `<link rel="canonical">`, no accidental `noindex`, real 200 status.

**Tier 1 — the substantive work (days)**

7. Answer-first writing under question-shaped `<h2>`/`<h3>`; each section self-contained and quotable.
8. `Organization` + (`LocalBusiness` | `Service` | `Product`/`Offer`) JSON-LD with a full `sameAs` array and a stable `@id`.
9. Explicit prices, service areas, hours, delivery and returns **as visible text**, not only in schema or an image.
10. Accurate `lastmod` in the XML sitemap; IndexNow ping on publish/update (Bing, Yandex, Seznam, Naver, Yep — **not** Google).
11. Bing Webmaster Tools verification + the **AI Performance** report (public preview since 10 Feb 2026) — currently the only first-party AI-citation reporting any engine offers. **[C]**
12. E-commerce: Google Merchant Center feed (GTIN accuracy is decisive), Microsoft Merchant Center, and — if US — the OpenAI product feed for ChatGPT Shopping.
13. Non-English markets: publish in the **query language**, with correct `hreflang`. Do not rely on an English page ranking for a Slovenian, Czech or Danish question.

**Tier 2 — worth doing, lower or unproven marginal value**

14. Entity work: Wikidata item, consistent NAP across GBP/Apple Maps/Bing Places/major directories, author `Person` schema with credentials.
15. Comparison, "X vs Y", "best X for Y", pricing and glossary pages.
16. Markdown-at-the-same-URL via `Accept: text/markdown` content negotiation (three of seven coding agents request it as of Feb 2026). **[C]**
17. `llms.txt` — cheap, harmless, and on current evidence almost certainly does nothing. **[MYTH-adjacent]**

---

## 1. Crawler access

### 1.1 The three classes of AI bot

The single most important conceptual move is to stop talking about "AI bots" and split them into three jobs, because your commercial interest is opposite in each:

| Class | Examples | What blocking costs you |
|---|---|---|
| **Training** | `GPTBot`, `ClaudeBot`, `Google-Extended`, `Applebot-Extended`, `CCBot`, `Bytespider`, `Meta-ExternalAgent` | Your content isn't in the next model's weights. No effect on today's citations. |
| **Search / index** | `OAI-SearchBot`, `Claude-SearchBot`, `PerplexityBot`, `Googlebot`, `Bingbot`, `Applebot` | You disappear from the retrieval index. **This is the expensive one.** |
| **User-triggered fetch** | `ChatGPT-User`, `Claude-User`, `Perplexity-User`, `Meta-ExternalFetcher` | A user who pastes your URL gets "I can't access that page." |

Blocking `GPTBot` does **not** remove you from ChatGPT answers, and allowing `GPTBot` does **not** get you cited. That is the mistake in most 2023-era robots.txt files still in production. **[C]**

Cloudflare's network-wide robots.txt analysis shows the market has converged on exactly this posture: crawlers with a blocking ratio above ~2:1 are all training agents; every crawler below ~1.2:1 is a search, user-action or preview agent. **[V]**

### 1.2 A robots.txt you can ship

```
# ---- Search / retrieval: allow. These decide whether you get cited. ----
User-agent: Googlebot
User-agent: Bingbot
User-agent: Applebot
User-agent: OAI-SearchBot
User-agent: Claude-SearchBot
User-agent: PerplexityBot
Allow: /
Disallow: /cart
Disallow: /checkout
Disallow: /*?sessionid=

# ---- User-triggered fetchers: allow, or users get "can't open that link" ----
User-agent: ChatGPT-User
User-agent: Claude-User
User-agent: Perplexity-User
User-agent: Meta-ExternalFetcher
Allow: /

# ---- Training crawlers: a business decision. Shown here as "allow". ----
User-agent: GPTBot
User-agent: ClaudeBot
User-agent: Google-Extended
User-agent: Applebot-Extended
Allow: /

# ---- Bulk scrapers with no consumer surface: usually block ----
User-agent: CCBot
User-agent: Bytespider
User-agent: Diffbot
Disallow: /

User-agent: *
Allow: /

Sitemap: https://example.com/sitemap.xml
```

**Trade-off on training bots.** Allowing them is a bet that being in model weights produces unattributed but real brand recall; blocking them is a bet on leverage and licensing. For a small service business or shop the bet is asymmetric: you have no licensing leverage, so allowing training bots costs little and may help the "which plumbers in Ljubljana are good" style of parametric answer. For a publisher with licensable archives, the calculus reverses. Cloudflare's own Radar data gives the commercial context: crawl-to-referral ratios reported in 2026 run in the thousands-to-one for training crawlers versus roughly 5:1 for classic Google search. **[V]**

**Content Signals.** Cloudflare published the **Content Signals Policy** on 24 September 2025, adding a machine-readable preference line inside robots.txt groups with three signals — `search`, `ai-input` (RAG/grounding) and `ai-train`. Defaults in Cloudflare's managed file: `search=yes`, `ai-train=no`, `ai-input` left unset. **[C]** It is a *stated preference*, not an enforcement mechanism, and no AI vendor has publicly committed to honouring it. **[S]**

```
User-agent: *
Content-Signal: search=yes, ai-input=yes, ai-train=no
Allow: /
```

### 1.3 CDN / WAF pitfalls — the real killer

Order of evaluation matters: **edge bot rules fire before your robots.txt is ever read.** Symptoms and diagnosis:

- `403` — outright deny (WAF rule, Cloudflare bot fight mode, "block AI scrapers" toggle in a hosting panel or a WordPress security plugin).
- `429` — rate limiting; the crawler may back off for days.
- `200` with a few hundred bytes of "Checking your browser…" — a JS interstitial. AI fetchers cannot solve it, so it reads as an empty page. This is the *worst* failure because every monitoring tool reports 200 OK. **[C]**

Cloudflare specifics you must check:
- **Default AI blocking** since 1 July 2025 for new customers, newly added zones and all free-plan zones. Existing paid zones keep their config. **[C]**
- From **15 September 2026**, "mixed-use" crawlers (those blending search, agent and training) are blocked by default on pages that carry ads. **[C]**
- **AI Crawl Control** (dashboard) exposes per-crawler Allow/Block, a robots.txt compliance tab (shipped 21 Oct 2025) and crawler metadata. **[C]**
- **Bot Preference Sync** (announced 21 Aug 2026) mirrors your dashboard Search/Agent/Training choices into robots.txt so edge rules and the published file cannot drift. **[C]**
- **Pay Per Crawl** (HTTP `402 Payment Required` at the edge) was announced 1 July 2025 in private beta and has since been repositioned as **"Pay Per Use"**, paying on *use in AI results* rather than on crawl, with You.com and Ceramic.ai as launch partners. **[C]** Do not enable it unless you are a publisher with real licensing ambitions — a 402 to a retrieval bot is functionally a block.

### 1.4 Verifying that a bot is who it claims

User agents are trivially spoofed, and in the one well-documented enforcement case Cloudflare accused Perplexity (4–5 August 2025) of rotating ASNs and impersonating a Chrome-on-macOS user agent after being blocked, generating 3–6M daily requests across tens of thousands of domains; Cloudflare de-listed Perplexity as a Verified Bot. **[C]** Perplexity disputed the characterisation.

Verify with published IP ranges rather than UA strings:

- OpenAI: `https://openai.com/gptbot.json`, `/searchbot.json`, `/chatgpt-user.json` (plus `/adsbot.json`, `/chatgpt-agents.json`)
- Anthropic: `https://claude.com/crawling/bots.json`
- Perplexity: `https://www.perplexity.ai/perplexitybot.json`, `/perplexity-user.json`
- Google and Bing: forward-confirmed reverse DNS (`*.googlebot.com`, `*.google.com`, `*.search.msn.com`) plus Google's published `googlebot.json` / `special-crawlers.json`.

*(I could not fetch these endpoints directly from this research environment — egress was blocked — so treat the exact paths as [V] and confirm once before wiring them into automation.)*

Emerging alternative: **Web Bot Auth**, HTTP Message Signatures that cryptographically prove bot identity, backed by Cloudflare and used for its "signed agents" programme. **[C]** Adoption is early.

### 1.5 Log analysis: proving AI bots actually fetch you

GA4 will never show this — bots don't execute analytics JS, and GA4 filters known bots by design. You need raw access logs or CDN logs. **[C]**

```bash
# Which AI agents hit the site, and how often, this month
grep -Ei 'GPTBot|OAI-SearchBot|ChatGPT-User|ClaudeBot|Claude-User|Claude-SearchBot|PerplexityBot|Perplexity-User|Google-Extended|Applebot|Bingbot|Bytespider|CCBot|Meta-External' \
  /var/log/nginx/access.log \
| awk '{print $NF}' | sed 's/.*(\([^;)]*\).*/\1/' | sort | uniq -c | sort -rn

# Status-code health per agent — anything that isn't 200 is a problem
awk '/OAI-SearchBot|ChatGPT-User|PerplexityBot|ClaudeBot/ {print $9}' \
  /var/log/nginx/access.log | sort | uniq -c
```

Two distinct signals to track separately:
- **Crawl hits** (bot UA, no referer) → the engine has seen the page.
- **Referral hits** (human UA, referer `chatgpt.com`, `perplexity.ai`, `copilot.microsoft.com`, `gemini.google.com`) → a citation converted to a click.

In GA4 a regex channel group on those referrers gives you the click side; only logs give you the crawl side. A large gap between "crawled a lot" and "never referred" usually means the page is retrievable but not answer-worthy — a content problem, not an access problem.

---

## 2. Rendering & structure

### 2.1 SSR is not optional

The decisive measurement is the Vercel + MERJ analysis of AI crawler behaviour (published December 2024, >500M fetches): **none of GPTBot, ClaudeBot, PerplexityBot, Meta's agent or ByteSpider executed JavaScript.** They fetch JS assets sometimes (ChatGPT ~11.5%, Claude ~23.8% of requests) and never run them. Gemini/AI Overviews inherit Googlebot's renderer; Applebot runs a browser-based renderer; Copilot inherits Bing's. **[C]**

Practical consequences:

- A React/Vue/Angular SPA with client-only data fetching is **text-invisible** to ChatGPT, Claude and Perplexity, while looking fine in Google Search Console.
- Price, stock, reviews and specs injected client-side (very common on Shopify apps, PIM widgets, review widgets) are the first things to vanish. Independent 2026 spot-tests of major retail product pages found prices missing from the raw HTML on some large retailers.
- **Test:** disable JavaScript in the browser, or `curl -s https://example.com/page | sed 's/<[^>]*>//g' | head -100`. If the answer to your page's core question isn't in that output, AI cannot cite it.

Fixes, in order of preference: true SSR/SSG (Next.js `app` router server components, Nuxt, Astro, Remix, plain server templates) → hydration on top of complete HTML → pre-rendering / dynamic rendering as a last resort (Google tolerates it but calls it a workaround).

### 2.2 Semantic HTML and chunkability

Retrieval in every one of these systems is **passage-level**, not page-level. The page is split into chunks, embedded, and individual chunks are retrieved. Your unit of optimisation is therefore the section, not the URL. **[C for the architecture; V for the specific sizes.]**

The working heuristic the industry has converged on — roughly **100–300 words per self-contained section**, one complete idea each — is a reasonable prior but has no primary-source validation. Treat "aim for semantic completeness" as the rule and the word count as a guide. **[S]**

The pattern that repeatedly appears in citation analyses:

```html
<article>
  <h1>Emergency plumber in Ljubljana — call-out times and prices</h1>

  <p class="summary">
    <strong>Short answer:</strong> a 24/7 emergency plumber in Ljubljana
    typically reaches you in 45–90 minutes and charges €70–€110 for the
    call-out plus €45/hour of labour. Weekend and night jobs carry a
    50% surcharge.
  </p>

  <h2>How much does an emergency call-out cost in Ljubljana?</h2>
  <p>A weekday call-out is €70. Between 20:00 and 06:00, and on Sundays
     and public holidays, the call-out is €110. Labour is billed at
     €45 per hour in 30-minute increments. Parts are quoted before work
     begins.</p>
  <table>
    <caption>Emergency plumbing rates, valid from 1 January 2026</caption>
    <thead><tr><th>Service</th><th>Weekday</th><th>Night / weekend</th></tr></thead>
    <tbody>
      <tr><td>Call-out</td><td>€70</td><td>€110</td></tr>
      <tr><td>Labour (per hour)</td><td>€45</td><td>€68</td></tr>
      <tr><td>Burst pipe repair (typical)</td><td>€150–€300</td><td>€220–€420</td></tr>
    </tbody>
  </table>

  <h2>Which areas do you cover?</h2>
  <p>We cover the Ljubljana urban municipality and Domžale, Kamnik,
     Vrhnika, Grosuplje and Medvode…</p>
</article>
```

What is doing the work here:

- **Headings phrased as the question a user asks.** Embedding similarity between a query and a heading-plus-first-sentence is a real retrieval mechanism.
- **The answer in the first 40–80 words of the section**, before any preamble. This is the "answer capsule" / "answer nugget" pattern; it is the single most consistently recommended structural tactic across vendors in 2026. **[V]**
- **Self-containment:** "€70" alone is useless; "A weekday call-out is €70" survives being lifted out of context.
- **Tables and lists.** Structured comparisons are easy to serialise and are quoted verbatim more often than prose.
- **Specific, checkable numbers with a date.** The Princeton/IIT Delhi GEO paper (arXiv 2311.09735, KDD 2024) measured nine content strategies against a generative-engine prototype and found **adding statistics, quotations and cited sources produced ~30–40% relative improvement** on its position-adjusted word-count visibility metric. **[C — but note: prototype engine, synthetic benchmark of 10k queries, not measured on live ChatGPT/Perplexity. The "40% more visibility" figure circulating in marketing decks is the maximum, not the average.]**

**A TL;DR block is worth having**, but keep it as normal visible prose at the top of the page. Do not hide it, do not mark it `data-nosnippet`, and do not build a separate "AI version" of the page — Google's May 2026 guidance explicitly calls AI-specific content rewriting unnecessary. **[C]**

### 2.3 Speed and timeouts

Live fetchers (`ChatGPT-User`, `Perplexity-User`, `Claude-User`) retrieve while a human waits for a streaming answer, so their patience is measured in single-digit seconds. The commonly cited figures — **1–5 second fetch timeouts, TTFB under 200ms as a target** — are repeated across dozens of 2026 GEO blogs but I found **no vendor documentation stating a timeout value**. **[S — treat as a sensible engineering target, not a published threshold.]**

What is defensible without a source: your *worst-case* server response under load determines retrieval success, not your median; cache HTML at the edge; avoid origin round-trips for bot traffic; don't gate content behind cookie walls, consent interstitials or lazy-loaded "read more" toggles.

---

## 3. Structured data and feeds

### 3.1 Do LLMs read JSON-LD? The honest answer

This is the most over-claimed area in GEO, and the evidence genuinely points both ways:

**Against direct effect.** A Search/Atlas analysis (December 2024) found **no correlation between schema coverage and citation rate**. Mark Williams-Cook demonstrated that typical page-to-text pipelines *strip* `<script type="application/ld+json">` before the model ever sees it. Controlled tests on ~1,885 pages reported a null result for citation lift. Google's own May 2026 guide lists "special schema for AI" under mythbusting. **[C]**

**For indirect effect.** Schema feeds Google's Knowledge Graph and Merchant Center/Shopping Graph, and AI Overviews and AI Mode are built on Google's core Search systems — so schema reaches AI Overviews *through the ranking pipeline*, not through the LLM's eyes. Product/Offer/AggregateRating, Review, Event, Recipe and Video remain live rich-result types and continue to drive eligibility. Structured data also removes ambiguity for anything that gets ingested as text (a `priceValidUntil` or `openingHours` field is unambiguous where prose is not). **[C]**

**Working conclusion:** implement structured data because it is the entry ticket to Google's product surfaces and costs you nothing, **not** because it makes ChatGPT cite you. If a fact matters, it must also be **visible in the HTML**. Schema that describes content not on the page is both a Google policy violation and invisible to LLM retrieval.

### 3.2 What to implement, by business type

**Every site — `Organization` on the homepage, with a stable `@id`:**

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "@id": "https://example.si/#organization",
  "name": "Vodovodar Novak d.o.o.",
  "url": "https://example.si/",
  "logo": "https://example.si/logo.png",
  "description": "24/7 emergency plumbing and heating in Ljubljana since 2004.",
  "foundingDate": "2004-03-01",
  "vatID": "SI12345678",
  "telephone": "+386-1-234-5678",
  "email": "info@example.si",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Dunajska cesta 12",
    "addressLocality": "Ljubljana",
    "postalCode": "1000",
    "addressCountry": "SI"
  },
  "sameAs": [
    "https://www.wikidata.org/wiki/Q000000",
    "https://www.linkedin.com/company/example",
    "https://www.facebook.com/example",
    "https://www.google.com/maps/place/?q=place_id:ChIJ...",
    "https://www.bizi.si/example/"
  ]
}
</script>
```

**Local / service business — `LocalBusiness` (use the most specific subtype: `Plumber`, `Dentist`, `Restaurant`, `HVACBusiness`…) plus `Service` per service page:**

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Service",
  "serviceType": "Emergency plumbing",
  "provider": { "@id": "https://example.si/#organization" },
  "areaServed": [
    { "@type": "City", "name": "Ljubljana" },
    { "@type": "City", "name": "Domžale" }
  ],
  "hoursAvailable": {
    "@type": "OpeningHoursSpecification",
    "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"],
    "opens": "00:00", "closes": "23:59"
  },
  "offers": {
    "@type": "Offer",
    "priceCurrency": "EUR",
    "priceSpecification": {
      "@type": "PriceSpecification",
      "minPrice": 70, "maxPrice": 110, "priceCurrency": "EUR",
      "description": "Call-out fee; €70 weekdays, €110 nights and weekends"
    },
    "availability": "https://schema.org/InStock"
  }
}
</script>
```

**Shop / product — `Product` + `Offer` + `AggregateRating` + `Review`, with real identifiers:**

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Merino Base Layer — Men's",
  "sku": "MBL-M-BLK-L",
  "gtin13": "4006381333931",
  "brand": { "@type": "Brand", "name": "Example" },
  "image": ["https://example.com/p/mbl-1.jpg"],
  "description": "190 g/m² 100% merino long-sleeve base layer.",
  "offers": {
    "@type": "Offer",
    "url": "https://example.com/p/merino-base-layer",
    "priceCurrency": "EUR",
    "price": "89.00",
    "priceValidUntil": "2026-12-31",
    "availability": "https://schema.org/InStock",
    "itemCondition": "https://schema.org/NewCondition",
    "shippingDetails": { "@type": "OfferShippingDetails", "shippingDestination": {"@type":"DefinedRegion","addressCountry":"SI"} },
    "hasMerchantReturnPolicy": { "@type":"MerchantReturnPolicy", "returnPolicyCategory":"https://schema.org/MerchantReturnFiniteReturnWindow", "merchantReturnDays": 30 }
  },
  "aggregateRating": { "@type": "AggregateRating", "ratingValue": "4.6", "reviewCount": "312" }
}
</script>
```

**Content pages — `Article` / `BlogPosting` with a real `author` as a `Person` entity** (`@id`, `sameAs` to LinkedIn/ORCID, `jobTitle`, `knowsAbout`), plus `dateModified` that is honest.

**`FAQPage` and `HowTo` — a status change you must know about.** Google restricted FAQ rich results in August 2023 and added a deprecation notice on **7 May 2026**; FAQ rich results no longer render, the Search Console report and Rich Results Test support were removed in June 2026. HowTo was deprecated earlier. **[C]** The markup is still valid schema.org and other consumers may parse it, so it is harmless to keep where it describes genuinely visible Q&A — but **do not add FAQ schema expecting a SERP benefit, and never add invisible FAQ blocks purely for markup.** The visible Q&A itself is what earns AI citations.

### 3.3 Product feeds — where shops actually win

For commerce, feeds now matter *more* than on-page markup, because AI shopping surfaces read merchant catalogues, not your HTML.

- **Google Merchant Center / Shopping Graph.** AI Mode shopping draws on Merchant Center feeds and the Shopping Graph (reported at 50B+ listings). **GTIN accuracy is the strongest matching signal** — a missing or invented GTIN keeps you out of the clustered product comparisons AI Mode builds. Feed freshness matters more than in classic Shopping. Agentic checkout rolled out on Search/AI Mode for eligible US merchants from late 2025, coordinated through the **Universal Commerce Protocol (UCP)**. **[C/V]**
- **OpenAI product feed (ChatGPT Shopping / Instant Checkout).** Spec version **2026-01-30**. Formats: gzipped JSONL/CSV/TSV or zstd Parquet, UTF-8. Nine required fields per purchasable item or variant (id, title, description, link, brand, seller name, image, availability, price). Push model over SFTP after merchant verification; updates as often as every 15 minutes. US-first, expanding through 2026. **[C]**
- **Microsoft Merchant Center.** Powers Copilot shopping answers across Copilot, Bing and Edge; added **Offer Highlights** (April 2026) for product differentiators inside Copilot conversations and **UCP-ready feeds** for US businesses; Copilot Checkout launched January 2026 with Shopify/PayPal/Stripe. **[C/V]**

The practical rule: your catalogue must exist in **three places** — on-page `Product`/`Offer` JSON-LD, a Google Merchant Center feed, and a Microsoft Merchant Center feed — with prices and availability identical in all three. Divergence between feed price and page price is a common cause of disapproval and of AI answers quoting stale prices.

### 3.4 Bing Webmaster Tools + IndexNow

Copilot is grounded in Bing's index, so Bing Webmaster Tools is not optional for Copilot visibility.

- **AI Performance report** — public preview since **10 February 2026** — shows which of your URLs are cited in Copilot, Bing AI summaries and partner integrations, and how citation activity changes over time. Microsoft's PMs describe it as "an early step toward GEO tooling." **[C]** This is the only first-party AI-citation reporting available from any engine today.
- **IndexNow** — a one-line ping that tells participating engines a URL changed. Participants include Bing, Yandex, Seznam, Naver and Yep. **Google is not a participant**; it said in November 2021 it would evaluate the protocol and has published nothing since. **[C]** So IndexNow helps Copilot/ChatGPT-via-Bing freshness and does nothing for Google.

```bash
curl -s "https://api.indexnow.org/indexnow?url=https://example.com/new-page&key=YOUR_KEY"
# key file must exist at https://example.com/YOUR_KEY.txt containing YOUR_KEY
```

---

## 4. llms.txt, markdown and meta-directives

### 4.1 llms.txt: the evidence

**Origin.** Proposed by Jeremy Howard (Answer.AI) in September 2024 at llmstxt.org: a markdown file at `/llms.txt` giving an LLM a curated index of a site, with an optional `/llms-full.txt` containing the full concatenated text. The original motivation was *context-window efficiency for coding assistants reading developer documentation*, not search visibility.

**The evidence against it working for search visibility is now strong:**

- **Ahrefs analysed server logs from ~137,000 domains (published 2026).** 28% published an `llms.txt`. **97% of those files received zero requests** in the sample month. Of the 3% that got any request, 96% of those requests came from bots — mostly SEO audit tools, not AI retrieval bots. No AI bot requested an `llms.txt` that didn't already exist, i.e. nothing is probing for it. **[C]**
- **John Mueller (Google)**, on *Search Off the Record*, said LLM systems cannot use `llms.txt` to decide which site to surface, and described it as "a temporary crutch, perhaps to save some tokens" for AI coding tools parsing developer docs. **[C]**
- **Google's official guide** (published 15 May 2026, `developers.google.com/search/docs/fundamentals/ai-optimization-guide`) puts `llms.txt` in a section explicitly titled mythbusting: Googlebot may discover the file but treats it as any other text file, with no special handling. The same section says **content chunking, AI-specific content rewriting and special schema are also not needed**. **[C]**
- **No AI vendor has documented reading it in production.** **[C, by absence]**

**[MYTH]** "Adding llms.txt gets you into ChatGPT." There is no measurement supporting this. The occasional case study claiming otherwise (e.g. "we submitted llms.txt and three days later it was powering AI answers") is uncontrolled and confounded with the site simply being crawled normally.

**Should you ship one?** It costs an hour and breaks nothing. If you do it, do it properly and keep it accurate — a stale llms.txt is worse than none:

```markdown
# Vodovodar Novak d.o.o.

> 24/7 emergency plumbing, heating and drain services in Ljubljana and
> the surrounding municipalities. Founded 2004. Licensed, VAT SI12345678.
> Call-out €70 weekdays / €110 nights and weekends.

## Services
- [Emergency plumbing](https://example.si/en/emergency-plumbing): 45–90 min response, €70–€110 call-out
- [Boiler repair and service](https://example.si/en/boiler-repair): all major brands, €89 annual service
- [Drain unblocking](https://example.si/en/drain-unblocking): CCTV survey included

## Pricing and terms
- [Full price list 2026](https://example.si/en/pricing)
- [Service areas](https://example.si/en/service-areas)
- [Warranty and complaints](https://example.si/en/warranty)

## About
- [About us and licences](https://example.si/en/about)
- [Contact](https://example.si/en/contact)

## Optional
- [Blog](https://example.si/en/blog)
```

Spend the hour on the visible pricing page instead if you have to choose.

### 4.2 ai.txt

Distinct from llms.txt and often confused with it. The version with real deployments is **Spawning's `ai.txt` (announced 30 May 2023)**, a *permissions* file for AI training opt-out — a door, not a map. Several unrelated proposals (a 2023 GitHub proposal, a 2025 arXiv paper, a June 2026 individual IETF draft) also use the name. **None is a standard and no major AI vendor commits to honouring any of them.** **[C]** Low priority.

### 4.3 Markdown and content negotiation

More promising than llms.txt because agents actually request it. A **February 2026 Checkly survey of seven widely used coding agents** found three (Claude Code, Cursor, OpenCode) send `Accept: text/markdown`; the other four request HTML. Cloudflare ships a zone-level "Markdown for Agents" feature and Vercel documents the pattern for Next.js. **[C]**

```nginx
# Nginx: serve pre-rendered markdown when an agent asks for it
map $http_accept $want_md {
    default            0;
    "~*text/markdown"  1;
}
location / {
    if ($want_md) { rewrite ^/(.*)$ /md/$1.md last; }
    try_files $uri $uri/ /index.html;
}
```

Also expose `https://example.com/page.md` alongside `https://example.com/page`, and cross-link them with `<link rel="alternate" type="text/markdown" href="/page.md">`. Keep both in sync automatically — hand-maintained duplicates rot.

**Caveat:** this is currently useful for *coding agents and agentic browsing*, not for ChatGPT/Perplexity search indexing, which fetch HTML. **[C]**

### 4.4 Robots meta directives vs AI Overviews

This is the area with the most dangerous misconfiguration risk, because the control that opts you out of AI Overviews also destroys your normal search snippets.

- `Google-Extended` (robots.txt token) controls **Gemini training and grounding**. It does **not** affect Googlebot, indexing, ranking, or AI Overviews/AI Mode eligibility. **[C]** **[MYTH]** "Block Google-Extended to stay out of AI Overviews" — false.
- `nosnippet`, `max-snippet:0`, `data-nosnippet` **do** remove a page from use as direct input to AI Overviews and AI Mode — and simultaneously remove your ordinary text snippet from blue-link results. **[C]** This is a blunt instrument. `data-nosnippet` on a specific `<span>`/`<div>` is the surgical version.
- For AI *visibility*, you want the opposite: `max-snippet:-1, max-image-preview:large, max-video-preview:-1`.

```html
<meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large, max-video-preview:-1">
```

---

## 5. Entity and brand consistency

Retrieval systems resolve your business to an **entity** before they decide whether to recommend it. A brand that resolves to one confident node gets recommended; a brand that resolves to three conflicting fragments gets skipped.

**NAP consistency.** Name, address and phone identical (character for character, including legal suffix and phone format) across your website, Google Business Profile, Bing Places, Apple Business Connect, Facebook, and the major national directories for your market. In smaller European markets the national business registry and the one or two dominant local directories matter more than the global aggregators. The industry rule of thumb — *20 consistent citations beat 100 inconsistent ones* — is **[V]** but conceptually right: conflicting data creates entity ambiguity, which is the failure mode.

**Google Business Profile.** For local service businesses GBP is frequently the *only* thing an AI answer cites, because Gemini and AI Overviews read it directly and ChatGPT pulls local data from third-party providers including maps and review platforms. Complete every field: categories (primary category is the strongest signal), service areas, hours including holiday hours, services with prices, attributes, and photos.

**Wikidata.** The cheapest structured entity anchor available and the one most likely to be consumed by multiple engines. Create an item with proper statements (instance of, country, inception, official website, industry, VAT/registry ID) and cite sources. Then link to it from your `Organization.sameAs`. Wikipedia is much harder and requires genuine notability — do not attempt to force it.

**Brand mentions vs links.** An Ahrefs study of ~75,000 brands (November 2025) reported unlinked brand mentions correlating with AI Overviews visibility at r ≈ 0.664 versus r ≈ 0.218 for backlinks. **[V — correlational, single vendor, not causal; but consistent with the entity-resolution model.]**

**Consistent descriptions.** Write one 150-character and one 50-word boilerplate description of the business and use it verbatim everywhere: meta description, `Organization.description`, GBP, LinkedIn, directories, press releases. Models aggregate; repetition of identical phrasing across independent sources is what makes a claim "confident" in an entity graph.

**About and author pages.** A real About page with founding date, registration number, team, physical address and licences; author bios as `Person` entities with `sameAs`. This is the machine-readable substrate for E-E-A-T-style trust assessment.

**Sitemaps and canonicals.**
- `<lastmod>` must be **true**. Google uses it only when it is consistently accurate; generators that stamp the sitemap build date on every URL cause Google to ignore the field for that domain entirely. **[C]**
- Sitemap contains only 200-status, canonical, indexable URLs. No redirects, no `noindex`, no parameter variants.
- One self-referencing canonical per page; never canonicalise a paginated or filtered page to the category root if it holds unique content.

**hreflang / multilingual — critical for smaller European markets.** The consistent finding across 2026 analyses is that **AI engines prefer passages written in the language of the query**, even when an English page on the same topic has stronger authority signals. **[V, multiple independent vendor studies]** For a Slovenian, Croatian, Czech, Danish or Greek market this means:

- Genuinely localise — translated-and-checked content, local prices in local currency, local phone format, local legal terms. Machine translation left unedited underperforms.
- Full bidirectional `hreflang` in the XML sitemap (more robust at scale than head tags), including `x-default`.
- Do **not** delete or de-prioritise the English version: Copilot data suggests sites with an English section receive more citations than those without (reported ~892 vs ~585 per 10,000 impressions). **[V]** Both, not either.
- Localise the *entity* too: local business registry entry, local directories, local-language Wikidata labels and descriptions.

```xml
<url>
  <loc>https://example.si/sl/nujna-vodoinstalaterska-pomoc</loc>
  <lastmod>2026-09-14</lastmod>
  <xhtml:link rel="alternate" hreflang="sl" href="https://example.si/sl/nujna-vodoinstalaterska-pomoc"/>
  <xhtml:link rel="alternate" hreflang="en" href="https://example.si/en/emergency-plumbing"/>
  <xhtml:link rel="alternate" hreflang="x-default" href="https://example.si/en/emergency-plumbing"/>
</url>
```

---

## 6. Site architecture for AI retrieval

AI answers are generated for **question-shaped intents**, so your information architecture should mirror the questions, not your org chart.

**Page types that earn citations disproportionately:**

| Page type | Why it gets retrieved | Implementation note |
|---|---|---|
| **Explicit pricing page** | "How much does X cost?" is one of the highest-volume AI query shapes and most competitors hide the answer | Real numbers or real ranges in HTML text + a dated table. "Contact us for a quote" is uncitable. |
| **"X vs Y" comparison** | Direct match to comparison intent; retrieval favours a clear verdict + a stable comparison frame | One table, one explicit verdict sentence, per-use-case recommendations, sources, a visible "last updated" date. Include your own weaknesses — honest comparisons are cited more. |
| **"Best X for Y"** | Matches the constrained-recommendation query shape | Segment by concrete use case ("best CRM for a 5-person agency"), not by generic superlative. |
| **Service-area / location pages** | Local intent resolution | One page per real service area with genuinely different content (travel time, local references, local pricing). Thin doorway pages are a Google policy risk *and* uncitable. |
| **Glossary / definition pages** | "What is X?" | 40–80 word definition first, then depth. One concept per URL. |
| **FAQ (visible, not just schema)** | Direct question-answer pairs are the ideal chunk | Real customer questions in customer phrasing. |
| **Topical hub** | Establishes topical coverage and distributes link equity | Hub links to every spoke; every spoke links back with descriptive anchor text. |

**Internal linking for AI.** Descriptive anchor text is doing double duty: it is a crawl path *and* a labelled entity relation. "Read more" teaches nothing; "emergency drain unblocking in Domžale" teaches the relation. Keep important pages within three clicks of the homepage, and ensure links exist in the server-rendered HTML (not injected by a JS menu component).

**A caution for Perplexity in particular:** analysis of Perplexity citations found a large share (~45% in one dataset) of cited pages had minimal organic traffic — retrieval there rewards structural fit with the query more than domain authority. **[V]** That is genuinely good news for small businesses: a well-structured pricing page from an unknown plumber can be cited over a large aggregator's vague listing.

---

## 7. Testing, validation and pre-launch checklist

### 7.1 Fetch-as-bot

```bash
# Is the page reachable at all for each retrieval agent?
for UA in "OAI-SearchBot/1.0" "ChatGPT-User/1.0" \
          "Mozilla/5.0 AppleWebKit/537.36 (KHTML, like Gecko; compatible; PerplexityBot/1.0; +https://perplexity.ai/perplexitybot)" \
          "ClaudeBot/1.0" "Claude-User/1.0" \
          "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" \
          "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"; do
  code=$(curl -s -o /tmp/body -w "%{http_code}" -A "$UA" -L --max-time 15 https://example.com/pricing)
  echo "$code  $(wc -c </tmp/body) bytes  ${UA:0:40}"
done
```

Read the checklist against the output:
- **403 / 429** → WAF or CDN rule. Fix at the edge, not in robots.txt.
- **200 but <5 KB** → JS challenge or an empty SPA shell.
- **200 and full HTML** → good; now check the *text*.

```bash
# Does the answer survive HTML stripping? (This is roughly what the model sees.)
curl -s -A "OAI-SearchBot/1.0" https://example.com/pricing \
  | sed -e 's/<script[^>]*>.*<\/script>//g' -e 's/<[^>]*>/ /g' \
  | tr -s ' \n' ' \n' | head -60
```

### 7.2 Live behavioural tests

- **ChatGPT:** paste the URL and ask "Summarise this page and list its prices." If it says it cannot access the page, `ChatGPT-User` is blocked. If it summarises but invents or omits prices, your prices are not in the raw HTML.
- **Perplexity:** search a query your page should answer, then check the citation list. Also paste the URL directly.
- **Claude:** paste the URL with web access enabled; `Claude-User` failures look identical to ChatGPT's.
- **Gemini / AI Mode:** query the target question and see whether the domain appears; use Google Search Console URL Inspection → "View crawled page" for the rendered HTML Google actually holds.
- **Copilot:** query Bing Chat and cross-check against **Bing Webmaster Tools → AI Performance** for citation counts.

Run each of these in a fresh/logged-out session; personalisation and memory contaminate the result.

### 7.3 Validators

- **Schema Markup Validator** (`validator.schema.org`) — validates *all* schema.org types. Use this one for `Service`, `FAQPage`, `Person` etc.
- **Google Rich Results Test** — only tests types Google still supports; note FAQ/HowTo support was removed in June 2026, so a "not eligible" result there is expected and not an error.
- **Google Search Console** — Coverage, URL Inspection (rendered HTML + blocked resources), Merchant listings report.
- **Bing Webmaster Tools** — URL Inspection, IndexNow status, AI Performance.
- **Cloudflare AI Crawl Control** — per-crawler traffic, robots.txt compliance tab.
- **Server logs / GoAccess / Logpush → BigQuery** — the ground truth for "did an AI bot actually fetch this."

### 7.4 Pre-launch checklist

```
ACCESS
[ ] robots.txt allows all retrieval + user-triggered agents; training decision made deliberately
[ ] curl-as-bot returns 200 with full HTML for 6+ agents
[ ] No Cloudflare/WAF/security-plugin "block AI bots" toggle left on
[ ] No cookie wall or consent interstitial blocking content for non-EU-IP bots
[ ] IP-range verification implemented before any bot-specific allow rule

RENDERING
[ ] Core content, prices, specs and reviews present with JS disabled
[ ] TTFB measured under load, not just from a warm cache
[ ] No lazy-loaded "read more" hiding the answer
[ ] Images have real alt text; key facts are not image-only

STRUCTURE
[ ] One H1; H2/H3 phrased as user questions
[ ] Each section answers in its first 40–80 words and stands alone
[ ] At least one comparison or specification table on commercial pages
[ ] Visible "Last updated: <date>" that is true

STRUCTURED DATA
[ ] Organization with @id + sameAs (incl. Wikidata if it exists)
[ ] LocalBusiness/Service or Product/Offer as appropriate
[ ] Every schema fact also visible in the HTML
[ ] Validates clean in validator.schema.org

FEEDS (commerce)
[ ] Google Merchant Center feed live, GTINs correct, price == page price
[ ] Microsoft Merchant Center feed live
[ ] OpenAI product feed submitted (if eligible market)

DISCOVERY
[ ] XML sitemap: canonical 200 URLs only, honest lastmod
[ ] hreflang complete and bidirectional, with x-default
[ ] IndexNow key file live and pings firing on publish
[ ] Verified in GSC and Bing Webmaster Tools

DIRECTIVES
[ ] meta robots = index, follow, max-snippet:-1, max-image-preview:large
[ ] No stray nosnippet / noindex / max-snippet:0
[ ] Canonical self-referencing

ENTITY
[ ] NAP identical on site, GBP, Bing Places, Apple, top national directories
[ ] GBP fully completed incl. services, prices, service areas, hours
[ ] One boilerplate description used verbatim everywhere

MONITORING
[ ] Log filter for AI user agents, with status-code breakdown, running weekly
[ ] GA4 channel group for chatgpt.com / perplexity.ai / copilot.microsoft.com / gemini.google.com referrals
[ ] Bing AI Performance report reviewed monthly
```

---

## Myths to retire

1. **[MYTH] "llms.txt gets you into AI answers."** 97% of published files were never requested in a 137k-domain log study; Google calls it unnecessary; no vendor documents reading it.
2. **[MYTH] "Block Google-Extended to opt out of AI Overviews."** Google-Extended governs Gemini training/grounding only. `nosnippet` / `max-snippet:0` are the AI Overviews controls — and they also kill your normal snippet.
3. **[MYTH] "Blocking GPTBot removes you from ChatGPT."** GPTBot is the *training* crawler. `OAI-SearchBot` and `ChatGPT-User` are what serve answers.
4. **[MYTH] "Schema markup makes LLMs cite you."** No controlled evidence of direct citation lift; typical HTML-to-text pipelines strip JSON-LD. It works *indirectly* via Google's Knowledge Graph and product surfaces. Implement it — for the right reason.
5. **[MYTH] "Add FAQ schema for rich results."** Deprecated by Google as of 7 May 2026. The *visible* Q&A still helps; the markup no longer earns a SERP feature.
6. **[MYTH] "IndexNow speeds up Google."** Google is not an IndexNow participant and has said nothing since a 2021 statement that it would evaluate it.
7. **[MYTH] "Write a separate AI-optimised version of the page."** Google's May 2026 guidance explicitly lists AI-specific rewriting and chunking as unnecessary. Cloaking a bot-only variant is also a policy violation.
8. **[UNVERIFIED, not myth] "TTFB must be under 200ms / crawlers time out at 3 seconds."** Sensible engineering targets, but no vendor has published a timeout value. Optimise worst-case response time on principle, not because of a documented threshold.

---

## Sources

**Platform / primary**
- Google, *Optimizing your website for generative AI features on Google Search* — https://developers.google.com/search/docs/fundamentals/ai-optimization-guide (published 15 May 2026; includes the mythbusting section on llms.txt, chunking, AI-specific rewriting and special schema)
- Google Search Central Blog, *A new resource for optimizing for generative AI in Google Search* — https://developers.google.com/search/blog/2026/05/a-new-resource-for-optimizing (15 May 2026)
- Google, FAQPage structured data documentation (deprecation notice added 7 May 2026) — https://developers.google.com/search/docs/appearance/structured-data/faqpage
- OpenAI, bots documentation — https://platform.openai.com/docs/bots ; IP ranges at https://openai.com/gptbot.json, /searchbot.json, /chatgpt-user.json
- OpenAI, *Product feeds — Agentic Commerce* spec (version 2026-01-30) — https://developers.openai.com/commerce/specs ; merchant portal https://chatgpt.com/merchants/
- Anthropic, crawler support article and IP ranges — https://support.anthropic.com/en/articles/8896518 ; https://claude.com/crawling/bots.json
- Perplexity bot documentation and IP ranges — https://www.perplexity.ai/perplexitybot.json, /perplexity-user.json
- Cloudflare Blog, *Your site, your rules: new AI traffic options for all customers* — https://blog.cloudflare.com/content-independence-day-ai-options/ (1 July 2025 — default AI blocking, Pay Per Crawl, HTTP 402)
- Cloudflare Blog, *Content Signals Policy* — https://blog.cloudflare.com/content-signals-policy (24 September 2025 — `search` / `ai-input` / `ai-train`)
- Cloudflare press release on Content Signals — https://www.cloudflare.com/press/press-releases/2025/cloudflare-gives-creators-new-tool-to-control-use-of-their-content/
- Cloudflare Blog, *Control content use for AI training with Cloudflare's managed robots.txt* — https://blog.cloudflare.com/control-content-use-for-ai-training/
- Cloudflare AI Crawl Control docs — https://developers.cloudflare.com/ai-crawl-control/ ; robots.txt tracking changelog https://developers.cloudflare.com/changelog/2025-10-21-track-robots-txt/ ; crawler info https://developers.cloudflare.com/changelog/2025-11-10-ai-crawl-control-crawler-info/
- Cloudflare, *How to detect AI crawlers* — https://www.cloudflare.com/learning/ai/how-to-detect-which-ai-bots-crawl/
- Cloudflare Blog, *The crawl before the fall… of referrals* — https://blog.cloudflare.com/ai-search-crawl-refer-ratio-on-radar/ ; *The crawl-to-click gap* — https://blog.cloudflare.com/crawlers-click-ai-bots-training/ ; *Content Independence Day, one year on* — https://blog.cloudflare.com/agentic-internet-bot-report/
- Bing Webmaster Blog, *Introducing AI Performance in Bing Webmaster Tools (Public Preview)* — https://blogs.bing.com/webmaster/February-2026/Introducing-AI-Performance-in-Bing-Webmaster-Tools-Public-Preview (10 February 2026)
- Google, *Universal Commerce Protocol (UCP)* — https://developers.google.com/merchant/ucp
- Google Blog, *Google Shopping launches agentic checkout* — https://blog.google/products-and-platforms/products/shopping/agentic-checkout-holiday-ai-shopping/
- llms.txt specification — https://llmstxt.org/

**Measurement and research**
- Vercel + MERJ, *The rise of the AI crawler* — https://vercel.com/blog/the-rise-of-the-ai-crawler (December 2024; >500M fetches; no JS execution by any AI crawler; JS-file fetch rates ChatGPT 11.50%, Claude 23.84%)
- Vercel, *How AI is changing SEO: lessons from a billion crawler requests* — https://vercel.com/i/how-ai-is-changing-seo
- Ahrefs, *We Analyzed 137K Sites: 97% of llms.txt Files Never Get Read* — https://ahrefs.com/blog/llmstxt-study/
- Aggarwal, Murahari, Rajpurohit, Kalyan, Narasimhan, Deshpande, *GEO: Generative Engine Optimization*, arXiv:2311.09735, KDD 2024 — https://arxiv.org/abs/2311.09735 (GEO-BENCH, 10k queries; 30–40% relative lift from statistics/quotation/citation addition on a prototype engine)
- Checkly, *The Current State of Content Negotiation for AI Agents* — https://www.checklyhq.com/blog/state-of-ai-agent-content-negotation/ (February 2026; 3 of 7 coding agents send `Accept: text/markdown`)
- Search Engine Land, *Inside ChatGPT's retrieval stack: the index, cache, and pages it actually reads* — https://searchengineland.com/chatgpt-retrieval-stack-index-cache-pages-485036
- Search Engine Land, *How schema markup fits into AI search — without the hype* — https://searchengineland.com/schema-markup-ai-search-no-hype-472339
- Search Engine Roundtable, *Structured Data & Schema Does Not Help With Visibility In AI Search* — https://www.seroundtable.com/structured-data-schema-ai-search-visibility-40099.html
- Search Engine Roundtable, *Google Says No AI System Currently Uses LLMs.txt* — https://www.seroundtable.com/google-ai-llms-txt-39607.html
- Search Engine Land, *Should multilingual websites add English pages for AI visibility?* — https://searchengineland.com/multilingual-websites-english-pages-ai-visibility-484251
- Weglot, *Does AI favor translated content? (1.3M citations analyzed)* — https://www.weglot.com/blog/multilingual-seo-ai-visibility ; *Untranslated means invisible* — https://www.weglot.com/blog/ai-search-and-language
- Duane Forrester, *Your AI Visibility Strategy Doesn't Work Outside English* — https://duaneforresterdecodes.substack.com/p/your-ai-visibility-strategy-doesnt
- G. Gagliardi (GSQI), *How to remove content and links from Google's AI Overviews and AI Mode using preview controls* — https://www.gsqi.com/marketing-blog/how-to-remove-content-and-links-from-google-ai-overviews/
- Lumar, *Content chunking & AI extractability* — https://www.lumar.io/blog/best-practice/content-chunking-ai-extractability-geo-aeo-explainer/
- TechnologyChecker, *robots.txt across Cloudflare's network — publishers block training bots, allow answering bots* — https://technologychecker.io/blog/robots-txt-ai-crawlers-blocking-report (September 2026 update)

**Incidents and context**
- Cloudflare / Search Engine Journal, *Cloudflare delists and blocks Perplexity from crawling websites* — https://www.searchenginejournal.com/cloudflare-delists-and-blocks-perplexity-from-crawling-websites/552899/ (August 2025)
- Daring Fireball, *Cloudflare: "Perplexity is using stealth, undeclared crawlers"* — https://daringfireball.net/linked/2025/08/05/cloudflare-perplexity
- MIT Technology Review, *Cloudflare will now block AI bots from crawling its clients' websites by default* — https://www.technologyreview.com/2025/07/01/1119498/cloudflare-will-now-by-default-block-ai-bots-from-crawling-its-clients-websites/
- TechCrunch, *Cloudflare's new policy pushes AI companies to pay for publishers' content* — https://techcrunch.com/2026/07/01/cloudflares-new-policy-pushes-ai-companies-to-pay-for-publishers-content/
- Help Net Security, *Cloudflare changes AI crawler access rules* — https://www.helpnetsecurity.com/2026/07/02/cloudflare-ai-crawler-controls/
- Search Engine Journal, *Google drops FAQ rich results from Search* — https://www.searchenginejournal.com/google-drops-faq-rich-results-from-search/574429/
- Search Engine Journal, *Google's new AI search guide calls AEO and GEO "still SEO"* — https://www.searchenginejournal.com/googles-new-ai-search-guide-calls-aeo-and-geo-still-seo/575026/
- PPC News Feed, *Microsoft Merchant Center supports Highlights for Copilot* — https://ppcnewsfeed.com/ppc-news/2026-04/microsoft-merchant-center-supports-highlights-copilot/
- DEV, *Your robots.txt says GPTBot is welcome. Your server says 403.* — https://dev.to/orzmar/your-robotstxt-says-gptbot-is-welcome-your-server-says-403-9f2
- ramhee98/ai-crawler-ipranges (daily-updated vendor IP lists) — https://github.com/ramhee98/ai-crawler-ipranges

**Research caveat:** this environment's network policy blocked direct fetching of most primary sources (platform.openai.com, developers.google.com, blog.cloudflare.com, ahrefs.com, vercel.com, llmstxt.org and others returned proxy 403s). Dates, figures and quotations above were assembled from search-result extracts of those pages plus corroborating secondary coverage. Before acting on any single number — particularly the OpenAI feed spec fields, the exact IP-range URLs, and the Cloudflare September 2026 default — verify against the primary URL listed.
