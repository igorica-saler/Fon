# How Local Businesses and Online Shops Get Recommended by AI Assistants (State of Play, September 2026)

**Research date:** 2026-09-21.
**Evidence policy:** every claim is tagged **[OFFICIAL]** (platform documentation / first-party announcement), **[REPORTED]** (credible trade press or published study), **[VENDOR]** (agency/tool-vendor claim, directionally useful but unaudited), or **[INFERENCE]** (my reasoning, not sourced).
**Method note / limitation:** this pass used ~25 web searches. Direct page fetches were blocked by the network egress proxy for most domains (openai.com, stripe.com, blog.google, developers.google.com, brightlocal.com, perplexity.ai, cnbc.com and others all returned `EGRESS_BLOCKED`); only GitHub-hosted sources could be read in full. Where a claim rests on a search-engine summary of a primary source rather than the primary source itself, it is tagged **[REPORTED]** rather than **[OFFICIAL]**. The one place I could read primary spec text is the Agentic Commerce Protocol repository, and that reading materially contradicts several vendor claims — see §6.

---

## Top actions for local businesses

1. **Treat Google Business Profile as the database record, not a marketing page.** GBP signals carry ~32% of local pack influence in Whitespark's 2026 study, with *primary category* the single strongest individual factor [REPORTED]. Gemini and AI Overviews treat the profile as the authoritative entity record.
2. **Get your opening hours exactly right, including holidays.** "Business is open at the time of search" rose into the top five Google local ranking factors in 2026, with visible ranking decay in the final hour before close [REPORTED]. This is the cheapest ranking win available in 2026.
3. **Claim and complete Yelp and Foursquare, not just Google.** Yelp is now a *licensed* feed inside ChatGPT (agreement disclosed in Yelp's FY2025 earnings, 12 Feb 2026; expanded July 2026) [REPORTED], and Foursquare's place data is reported to sit behind a large share of ChatGPT's local tool calls [VENDOR].
4. **Claim Apple Business Connect.** Siri/Apple Intelligence local answers are built on Apple Maps place cards, which Business Connect controls [REPORTED]. It is uncontested inventory in most markets.
5. **Make yourself bookable by machine.** Agentic restaurant booking went global in Google AI Mode in April 2026 and reads OpenTable/Resy/Tock-class inventory [REPORTED]; Yelp reservations and waitlist now execute inside ChatGPT [REPORTED]. If your availability is not in a bookable system an agent can reach, you are not a candidate for these flows.
6. **Write the review text you want quoted.** Review signals climbed to ~20% of local weight in 2026 (from ~16% in 2023) [REPORTED], and assistants summarise review *language*, not just star counts. Ask customers (compliantly, no incentives, no gating) about the specific attribute you want to own — "same-day", "emergency", "speaks English", "wheelchair accessible", "good with anxious patients".
7. **Publish the answers people actually ask an assistant**: price ranges ("cost of a root canal in Zagreb"), service-area pages with real local detail, FAQs, named staff bios with credentials. On-page content is the heaviest single input (24%) in Whitespark's new *AI Search Visibility* category — where GBP drops to only 12% [REPORTED]. AI visibility and map-pack visibility are *not* the same optimisation.
8. **Ship `LocalBusiness` (or the right subtype) + `Service` + `areaServed` + `openingHoursSpecification` + `geo` schema**, consistent with GBP to the character [INFERENCE from long-standing Google structured-data guidance; I could not re-verify developers.google.com this pass].
9. **Be in the curated lists.** "Three Best Rated"-style curated directories and chamber-of-commerce listings punch far above their traffic in ChatGPT local citations [REPORTED/VENDOR].
10. **Track by location, not globally.** ChatGPT shipped precise device-location sharing on 26 Mar 2026 [REPORTED]; local answers vary by >50% between cities [VENDOR]. Rank tracking without a location grid is meaningless.

## Top actions for shops

1. **Feeds beat crawling — everywhere.** Every major surface in 2026 (Google Merchant Center → Shopping Graph, OpenAI's merchant feed, Perplexity Merchant Program, Microsoft Merchant Center) ingests a pushed structured catalogue. OpenAI's public post-mortem on Instant Checkout was explicitly that scraping retail sites could not reflect real inventory and price [REPORTED].
2. **Price and availability accuracy are ranking factors, not hygiene.** Merchants are ranked on availability, price, quality and whether they are the maker/primary seller [VENDOR summarising OpenAI help-centre text]. Stale feeds get suppressed before they get demoted.
3. **Fix GTINs.** GTIN is the join key for product clustering in Google's ~50-billion-listing Shopping Graph; missing or invented GTINs drop you out of the clusters AI Mode aggregates [VENDOR, consistent with long-standing Merchant Center rules].
4. **Plan for UCP, not just ACP.** Google + Shopify announced the Universal Commerce Protocol at NRF on 11 Jan 2026, co-developed with Etsy, Wayfair, Target and Walmart and endorsed by 20+ partners; it is being consumed by Google AI Mode, Gemini, Microsoft Copilot and ChatGPT [REPORTED]. The public spec defines capabilities (Checkout, Identity Linking, Order, Payment Token Exchange) exposed over REST, **MCP** or A2A [OFFICIAL — GitHub README read directly].
5. **Do not architect around in-chat checkout alone.** OpenAI retired Instant Checkout on 24 Mar 2026 after ~30 Shopify merchants ever went live, and pivoted to discovery-in-chat + retailer apps/redirect [REPORTED]. Walmart reported ChatGPT checkout converting ~3x worse than its own site [REPORTED]. "Discover in AI, buy on site" is the 2026 default.
6. **Join the free programs.** Perplexity's Merchant Program is free, requires US sell/ship, takes ~5 form steps, and gates the "Buy with Pro" affordance plus prioritised indexing and a merchant dashboard [REPORTED/VENDOR]. Copilot Checkout reached 500k+ merchants, with Shopify stores auto-enrolled and PayPal/Stripe merchants onboarding by form [REPORTED].
7. **Write product pages for extraction, not persuasion**: spec tables, explicit use-case statements ("for X, if Y"), sizing/compat data, return and shipping policy in text, Q&A blocks, `Product` + `Offer` + `AggregateRating`/`Review` schema.
8. **For Amazon, optimise for Rufus' six inputs**: attributes, title/bullets, A+ content, reviews, Q&A, and text inside image infographics [VENDOR, consistent across multiple independent 2026 guides].
9. **Earn third-party corroboration.** A product that appears in a feed *and* in independent "best X for Y" coverage ranks higher [VENDOR]. Reddit threads, YouTube reviews and Wirecutter-class roundups are the corroboration layer.
10. **Measure AI referrals properly.** AI-referred retail traffic converted ~42% better than non-AI traffic in March 2026 — a reversal of ~80 percentage points year over year [REPORTED]. Volume is still 47–190x smaller than Google organic [VENDOR]. Segment it, don't average it away.

---

# Part 1 — Local & services

## 1. What each engine actually reads for "best X near me"

**ChatGPT.** ChatGPT does not have a map index of its own; it resolves local prompts through tool calls to licensed place/review APIs plus web search. Three layers are consistently described:

- **Place data.** Foursquare's places API is reported to back the large majority of ChatGPT local business results [VENDOR: Local Falcon claims >70%; unverified methodology, treat as directional].
- **Reviews and ratings.** Yelp. This moved from "a page ChatGPT might read" to a contractual feed: Yelp disclosed an OpenAI agreement in its FY2025 results on 12 Feb 2026, and Axios reported on 23 Jul 2026 that Yelp licenses reviews, photos and business info to OpenAI, with Yelp branding and links surfaced in ChatGPT answers, plus Yelp "Request a Quote" coming to ChatGPT for local service providers [REPORTED].
- **Web.** Bing's index as the catch-all, plus the business's own website — which BrightLocal's *Uncovering ChatGPT Search Sources* study found to be the largest single citation bucket at **58%** of local sources, ahead of business mentions (27%) and directories (15%); within directories, Three Best Rated alone was ~24%, while Yelp, Facebook and Google Maps did not appear as directory citations in that sample [REPORTED]. Wikipedia accounted for 39% of "business mention" sources [REPORTED].

A separate analysis (Yext/Cheers) put third-party sites such as Yelp, TripAdvisor and MapQuest at **48.73%** of ChatGPT citations [VENDOR]. The two figures are not reconcilable without methodology detail — note the disagreement rather than averaging it.

**Geolocation.** ChatGPT infers country/region from IP by default, and since **26 Mar 2026** supports explicit precise device-location sharing for local recommendations, news and weather [REPORTED — Search Engine Land, Search Engine Roundtable, PPC Land]. Practical consequence: the same prompt returns materially different businesses by city, with variation reported above 50% for local/regional queries [VENDOR].

**Google (AI Mode, AI Overviews, Gemini).** Google is answering from its own entity graph: Business Profile, Maps reviews, Maps user content, plus the open web via query fan-out. Multiple 2026 analyses describe GBP as the structural foundation of local recommendations in AI Overviews and Gemini, treated as authoritative [VENDOR].

**Apple (Siri / Apple Intelligence).** Apple Maps place cards, fed by Apple Business Connect, plus licensed review data (Yelp has historically licensed to Apple Maps) and web content; Apple additionally layers on-device signals — location history, app usage, on-screen context — that no other assistant has [REPORTED/VENDOR].

**Perplexity and Copilot.** Both are primarily web-index-plus-citation systems for local; neither has announced a proprietary place graph. Copilot inherits Bing Places/Bing's local index [INFERENCE].

## 2. Google Business Profile optimisation for AI Mode and Gemini

Whitespark's 2026 Local Search Ranking Factors report (47 contributors, 187 factors) is the most useful public quantification [REPORTED via multiple secondary summaries; I could not fetch whitespark.ca directly]:

| Signal group | Weight (local pack) |
|---|---|
| Google Business Profile signals | ~32% |
| Review signals | ~20% (up from ~16% in 2023) |
| On-page signals | ~15% |

Within GBP: **primary category** is the single most important individual factor; proximity and keywords in the business name complete the top three; profile completeness and ongoing listing management matter. The notable 2026 addition is **"business is open at time of search" entering the top five**, with rankings observably degrading in the final hour before closing [REPORTED].

Crucially, the 2026 edition introduced a **separate "AI Search Visibility" category** in which the weighting inverts: **on-page content is heaviest at ~24% and GBP falls to ~12%**, with structured data, consistent citations and curated-list mentions named as direct inputs [REPORTED]. This is the single most actionable finding in the local half of this report: the profile wins you the map pack; the *website* wins you the AI answer.

Practical GBP checklist that maps to how assistants consume the record: exact primary category + secondary categories; the full **services** list with descriptions (this is the text an assistant can quote for "do they do X?"); attributes (accessibility, payment, languages, "identifies as"); Q&A seeded with real questions and owner answers; Posts for time-bound facts (holiday hours, new service lines); geotagged, recent photos; and review responses that restate the service and the city in natural language [VENDOR consensus; no first-party Google documentation states that these feed AI answers].

**Schema.** Use `LocalBusiness` or the precise subtype (`Dentist`, `Plumber`, `Restaurant`, `HairSalon`, `MedicalClinic`), plus `Service` nodes with `serviceType`, `areaServed`, `provider`, `offers`/`priceRange`, `openingHoursSpecification`, `geo`, `sameAs` pointing at your Yelp/Facebook/industry profiles, and `hasOfferCatalog` for the service menu [INFERENCE / standard practice — not re-verified against developers.google.com this pass]. The function of schema in an LLM pipeline is disambiguation: it tells the retrieval layer that this page is about *this* entity in *this* place, which is exactly the judgement an assistant has to make before it will name you.

## 3. Site content that wins local AI answers

Because on-page content dominates the AI-visibility weighting, the content types that earn citations are the ones that answer the *whole* prompt:

- **Service-area pages** with genuinely local substance (neighbourhoods served, response times, local regulations, local pricing) — not templated city swaps, which are both an E-E-A-T liability and trivially deduplicated by a retrieval system [INFERENCE].
- **"Cost of X in [city]" pages.** Pricing transparency pages are disproportionately cited because assistants are routinely asked "how much does X cost" and most local sites refuse to answer [VENDOR/INFERENCE]. Publish ranges, what drives the range, and what's included.
- **FAQ blocks** in question-shaped headings, answered in the first 1–2 sentences.
- **Case studies with locations** and **named staff bios with credentials** — the entity corroboration that lets an assistant say "Dr X, a periodontist in Y".
- **Multilingual for non-English markets** (e.g. the Balkans/EU): publish in the local language *and* keep a canonical English surface. Assistants answer in the user's prompt language and retrieve in it; an SR/HR/BS-only site is invisible to English-language prompts about your city, and an English-only site is invisible to local-language prompts. Use `hreflang`, distinct URLs, and localise the *review corpus* too [INFERENCE — no study found in this pass].

## 4. Reviews: what the engines trust and how the text is used

- **Volume, velocity, recency, rating, keywords-in-reviews and response rate** are all named components of the ~20% review weight [REPORTED].
- **Attribute extraction is the real mechanism.** Assistants build the adjectives from review prose: "emergency", "affordable", "English-speaking", "open late", "good with kids". If a filter-style attribute does not appear in your review corpus, you will not be shortlisted when a prompt asks for it [INFERENCE, strongly implied by how the Yelp/Foursquare feeds are structured].
- **Source trust differs by engine and vertical.** Google privileges its own Maps reviews; ChatGPT now has a licensed Yelp corpus; Apple leans Maps/Yelp; vertical sites (Healthgrades, Zocdoc, Angi, TripAdvisor, OpenTable, Booking) act as corroboration and as directory citations [REPORTED/INFERENCE — I could not complete the planned study search on review-site citation share before the search budget was exhausted].
- **Compliance.** Do not gate, incentivise or filter reviews; Google and Yelp both prohibit it and Yelp actively suppresses solicited reviews. The compliant lever is *timing and prompting for specificity*: ask at the moment of value, and ask an open question about the attribute, not for a rating.

## 5. Agentic booking and contact: what a business must expose

The 2026 state of play is that agents book through **partner inventory**, not by improvising on your website:

- **Google AI Mode agentic restaurant booking**: launched Aug 2025 for US Google AI Ultra subscribers; **globally expanded in April 2026** — 8 new markets (Australia, Canada, Hong Kong, India, New Zealand, Singapore, South Africa, UK), and no longer subscription-gated. It queries multiple reservation platforms (OpenTable, Resy, Tock and local partners) in real time against constraints like party size, cuisine, time and "vibe" [REPORTED].
- **Yelp inside ChatGPT**: book a table or join a waitlist without leaving the chat; "Request a Quote" for service businesses announced as coming [REPORTED].
- **Checkout/booking protocols**: ACP (OpenAI/Stripe) and UCP (Google/Shopify) are both product-commerce-shaped today. I found **no** mention of local services or appointment commerce in the UCP README [OFFICIAL — read directly], which is a real gap: service businesses are reached via reservation partners, not via a native protocol.

What that means concretely for a local business: (a) be inside at least one machine-readable booking system in your vertical (Reserve with Google partner, OpenTable/Resy/Tock, Yelp waitlist, Zocdoc, Fresha/Booksy for salons); (b) expose a stable, unauthenticated **booking URL** and a real phone number in GBP, Apple Business Connect, Yelp and schema (`potentialAction` / `ReserveAction`); (c) keep hours and closures accurate, because open-now is both a ranking factor and a booking precondition; (d) expect AI-mediated phone contact — Google has shipped AI calling for local price/availability checks in the US — and make sure whoever answers the phone can handle a scripted, structured enquiry [REPORTED/INFERENCE].

---

# Part 2 — E-commerce & products

## 6. ChatGPT shopping: the 2026 reset

This is the area where the 2026 reality diverges most from 2025 commentary.

**Timeline [REPORTED, CNBC / Modern Retail / Forbes / Retail TouchPoints]:**
- 29 Sep 2025 — OpenAI launches **Instant Checkout** with US Etsy sellers and publishes the **Agentic Commerce Protocol** with Stripe under Apache 2.0. Etsy shares rose ~16% on the news.
- Late Jan 2026 — Shopify merchant onboarding opens; a **4% transaction fee** on completed Instant Checkout purchases is confirmed.
- Feb 2026 — only ~**30 Shopify merchants** are actually live (Forrester's Emily Pfeiffer). Walmart (~200,000 products in ChatGPT) reports checkout converting ~**3x worse** than its own site.
- **24 Mar 2026 — OpenAI retires Instant Checkout**, citing merchant onboarding friction, inaccurate product data (it had been scraping retail sites), and the absence of multi-item carts and loyalty linkage. The replacement strategy is discovery inside ChatGPT plus **retailer apps** and redirect to the retailer's own checkout.

**What survived: the protocol and the feed.** ACP is still being developed — spec releases dated 2025-09-29, 2025-12-12, 2026-01-16, 2026-01-30 and **2026-04-17 (current stable, beta)** [OFFICIAL — GitHub repo read directly]. The 2026-04-17 release adds cart, feed, orders, authentication and MCP. The OpenAPI folder contains `openapi.agentic_checkout.yaml`, `openapi.agentic_checkout_webhook.yaml`, `openapi.cart.yaml`, `openapi.delegate_authentication.yaml`, `openapi.delegate_payment.yaml` and `openapi.feed.yaml` [OFFICIAL].

**A correction to common vendor advice.** I read `openapi.feed.yaml` directly. Required fields are minimal: product `id` and `variants[]`; per variant `id` and `title`; feed-level `target_country` (ISO 3166-1 alpha-2) and `updated_at`. Supported attributes include `price` (minor units + ISO 4217), `list_price`, `unit_price`, `availability.available` and `availability.status` (in_stock / limited_stock / backorder / preorder / out_of_stock / discontinued), `barcodes` (GTIN/UPC/EAN), `media`, `description` (text/HTML/markdown), `categories`, `condition`, `variant_options`, `seller`, `marketplace`. **The ACP feed schema contains no review-rating, review-count, popularity or return-rate fields** [OFFICIAL]. Several 2026 agency guides assert that "popularity score, return rate, review count and average rating" are feed ranking inputs [VENDOR] — those fields exist in OpenAI's *own merchant feed spec* documentation (developers.openai.com, which I could not reach this pass), not in the open ACP feed spec. Do not conflate the two when briefing a dev team.

**Ranking.** OpenAI's help-centre language, as quoted by multiple secondary sources, is that results are **organic and not paid placements**, and that merchants are ranked on factors including **availability, price, quality, and whether the merchant is the maker or primary seller** [REPORTED — secondary quotation of an OFFICIAL source; help.openai.com was unreachable]. I found no evidence as of Sep 2026 that OpenAI has introduced paid ranking in shopping results; the disclosed monetisation was the 4% Instant Checkout fee, which is now moot. Treat "ads in ChatGPT shopping" as **speculation** until OpenAI states otherwise.

## 7. Google: Shopping Graph, AI Mode and UCP

- **Shopping Graph**: >50 billion listings, interpreted with Gemini models; Merchant Center feeds are the input [REPORTED]. AI Mode reads **free listings** — described as not bid-responsive — and weights **catalogue completeness and real-time accuracy** heavily, with feed freshness mattering more than in classic Shopping [VENDOR].
- **Agentic checkout ("let Google buy it for you")**: confirm-then-purchase on the merchant's site via Google Pay, rolling out with select US merchants [REPORTED].
- **Universal Commerce Protocol (UCP)**: announced by Sundar Pichai at NRF on **11 Jan 2026**, co-developed with Shopify and with Etsy, Wayfair, Target and Walmart; endorsed by 20+ partners including Visa, Mastercard, Stripe, Amex, Best Buy and Home Depot; compatible with **AP2** for agent payments; rolling out across Google AI Mode, the Gemini app, **Microsoft Copilot and ChatGPT** [REPORTED]. The spec itself defines modular **Capabilities** (Checkout, Identity Linking, Order, Payment Token Exchange) and **Extensions** (Discounts, Fulfillment); merchants **declare** supported capabilities in a standardised profile and **expose** them over REST, **MCP** or A2A, with OAuth 2.0 identity linking and webhook order updates; the merchant remains merchant of record, keeping pricing, checkout logic and customer data [OFFICIAL — GitHub README read directly]. Apache 2.0, ~3.4k stars, with Python/JS SDKs, a conformance suite and public meeting minutes.
- **Eligibility hygiene**: correct GTINs (the clustering key), complete and fresh Merchant Center feeds, `Product`/`Offer`/`AggregateRating` markup matching feed values, accurate shipping and returns data [VENDOR + standard Merchant Center policy].
- Virtual try-on, price tracking and "buy for me" are consumer-side features built on the same feed; eligibility follows from feed quality and image quality rather than any separate program [INFERENCE].

## 8. Perplexity, Amazon Rufus, Copilot, Meta AI

**Perplexity.** The **Merchant Program** is free, requires selling and shipping to the US, and is a short online form. Members get prioritised indexing, a merchant dashboard with search/shopping trend data, and the **"Buy with Pro"** affordance on product cards (one-click purchase inside the chat, powered by PayPal, with Perplexity-sponsored free shipping); non-members get a plain click-out link. Perplexity has stated that product-data completeness is a direct ranking signal and that catalogue detail (reviews, pricing, specs, images) improves both indexation and recommendation quality. Shopify is the simplest onboarding path; BigCommerce and Adobe Commerce are supported via standard feed/API work [REPORTED/VENDOR].

**Amazon Rufus** (now also surfaced as "Alexa for Shopping" in some 2026 materials [VENDOR]). Rufus reads **six sources**: structured product attributes, listing copy (title/bullets), **A+ content**, customer reviews, customer Q&A, and **text extracted from image infographics**. The "Help Me Decide" comparison feature draws from listing content, reviews and A+ content. Seller implications: answer questions in bullets rather than keyword-stuffing; build A+ content with comparison charts, use-case imagery and Q&A-structured sections; complete every backend attribute (materials, certifications, compatibility); maintain 10+ answered Q&As; and encourage detailed (not just 5-star) reviews, because Rufus needs prose to extract from [VENDOR — consistent across many independent guides, no Amazon first-party spec found].

**Microsoft Copilot.** **Copilot Checkout** launched Jan 2026 and by mid-2026 spanned **500,000+ merchants**, including mobile-app checkout. The merchant route is a **UCP-ready feed in Microsoft Merchant Center**, which carries richer signals such as returns and support policies "so AI can assess products with confidence"; **Shopify merchants are auto-enrolled** with controls in Shopify admin; others onboard via a PayPal or Stripe form. Microsoft also markets "Brand Agents" — merchant-controlled conversational agents inside Copilot [REPORTED, incl. Microsoft Advertising's own January 2026 blog].

**Meta AI.** No merchant feed program comparable to the above was found in this pass. Meta AI's commerce surface is Facebook/Instagram Shops catalogue data plus ads [INFERENCE — flagged as a research gap].

## 9. Product-page content that actually gets cited

Synthesising the ranking language across surfaces [VENDOR consensus + INFERENCE]:

- **Unique descriptions.** Manufacturer boilerplate duplicated across 200 retailers gives a retrieval system no reason to pick you. Original prose is the differentiator when the feed data is identical.
- **Spec tables** in real HTML `<table>` markup — measurements, materials, compatibility, power, certifications. These are the highest-extraction-value blocks on the page.
- **Explicit use-case framing.** Assistants are asked "best X *for Y*". Pages that literally say "best for narrow feet", "for apartments under 40 m²", "for beginners" win those prompts.
- **UGC reviews with attributes** rendered in crawlable HTML (not JS-only widgets), plus `Review`/`AggregateRating` markup.
- **Q&A sections** — the same mechanism that works for Rufus works on the open web.
- **Pricing clarity, shipping cost, delivery windows, and a plain-text return policy.** UCP explicitly elevates returns/support policy to a machine-readable signal [REPORTED], which tells you how these surfaces are starting to score merchants.
- **Your own "best X for Y" guides** that include your products alongside honest alternatives — this is how a merchant domain earns a citation on a comparison prompt rather than only a product card.
- **Merchant listing / Product schema** aligned exactly with the feed. Divergence between schema price and feed price is a disqualifier in Merchant Center and a trust problem for agents.

## 10. Third-party mentions: the corroboration layer

Assistants triangulate. A product in your feed *and* in independent "best X for Y" coverage ranks higher [VENDOR]. The three corroboration sources that matter most:

- **Reddit** — heavily cited by both ChatGPT and Google AI surfaces; Google and OpenAI both have Reddit data arrangements. Genuine subreddit presence and product mentions in organic threads matter; astroturfing is both against subreddit rules and easily detected [INFERENCE — I could not complete the citation-share study search].
- **YouTube reviews** — Gemini can reason over YouTube transcripts natively, making video review coverage a Google-specific advantage [INFERENCE].
- **Affiliate/editorial roundups** (Wirecutter-class) and **Amazon reviews** — the "quality" signal OpenAI references is most plausibly assembled from exactly this corpus [INFERENCE].

For local, the analogue is curated directories (Three Best Rated at ~24% of ChatGPT directory citations), Wikipedia (39% of business-mention citations), chambers of commerce, and local news [REPORTED].

## 11. Measurement

**Referrer strings to segment:** `chatgpt.com`, `chat.openai.com`, `openai.com`, `perplexity.ai`, `gemini.google.com`, `copilot.microsoft.com`, `bing.com/chat`, `claude.ai`, `you.com`, plus `utm_source=chatgpt.com` which OpenAI appends to some outbound links. For feeds, add your own UTMs to feed link fields where the platform permits it, so agent-driven click-outs are attributable [INFERENCE / standard practice].

**Benchmarks found [REPORTED unless noted]:**
- AI-referred retail traffic converted **~42% better** than non-AI traffic in March 2026 — reversing a ~38%-worse position a year earlier, an ~80-point swing.
- Across 94 ecommerce sites, ChatGPT traffic converted **31% higher** than non-branded organic search [VENDOR].
- Reported per-platform conversion rates: ChatGPT **15.9%**, Perplexity **10.5%**, Claude **5.0%** [VENDOR — First Page Sage; B2B-weighted, do not apply to retail].
- Ahrefs: 0.5% of traffic from AI platforms drove **12.1% of signups**, ~**23x** the organic conversion rate [REPORTED, first-party data from Ahrefs].
- Semrush 2026: AI-driven visitors convert at **4.4x** standard organic [VENDOR].
- Volume caveat: AI referral volume remains **47–190x smaller** than Google organic [VENDOR].
- Counterpoint worth keeping: at least one 2026 analysis argues ChatGPT traffic converts *worse* than Google for some site types [VENDOR]. The honest reading is that AI traffic is low-volume, late-funnel and high-variance by vertical — measure your own.

**Local measurement** requires a location grid (ChatGPT answers vary >50% by city [VENDOR]) and prompt-level tracking rather than keyword rank. Category tools in this space include Local Falcon, BrightLocal, Whitespark, Semrush AI toolkit, Profound, Peec and similar; none were independently evaluated in this pass [INFERENCE].

---

## Evidence quality ledger

**Strongest (read directly from primary source):** ACP spec versions and feed schema fields; UCP capability model, transports and governance.
**Strong (credible trade press, multiple corroborating outlets):** Instant Checkout launch, 4% fee, ~30 merchant adoption, 24 Mar 2026 retirement; UCP NRF announcement and partners; Yelp–OpenAI licensing and ChatGPT reservations; ChatGPT location sharing (26 Mar 2026); Google agentic restaurant booking global rollout (Apr 2026); Copilot Checkout scale.
**Directional only (vendor/agency):** Foursquare's >70% share of ChatGPT local results; Whitespark weightings (secondary summaries, not the report itself); all conversion-rate multiples; Rufus' input list; Perplexity ranking-signal language.
**Unresolved gaps in this pass:** review-site citation share by engine; Reddit/YouTube citation share studies; Meta AI merchant path; first-party Google structured-data confirmation for local; OpenAI's own merchant feed spec fields; multilingual/Balkan-market evidence. All were blocked by exhausted search budget or egress restrictions, not by absence of data.

---

## Sources

- [Uncovering ChatGPT Search Sources — BrightLocal](https://www.brightlocal.com/research/uncovering-chatgpt-search-sources/)
- [ChatGPT Local Search Data Sources — Local Falcon](https://www.localfalcon.com/blog/chatgpt-local-search-data-sources-where-does-business-info-come-from)
- [ChatGPT, Gemini, Perplexity Sources for Local Businesses — Cheers](https://www.cheers.tech/geo-academy/ai-search-engine-source-differences)
- [Want to Rank in ChatGPT: Focus on These Review Sites — Whitespark](https://whitespark.ca/blog/want-to-rank-in-chatgpt-focus-on-these-review-sites-new-research/)
- [Whitespark's Guide to Google AI Mode for Local Businesses](https://whitespark.ca/guides/whitesparks-guide-to-googles-ai-mode-for-local-businesses/)
- [Whitespark 2026 Local Search Ranking Factors](https://whitespark.ca/local-search-ranking-factors/)
- [Local Memo: Local Ranking Factors of 2026 Have Arrived — SOCi](https://www.soci.ai/blog/local-memo-local-ranking-factors-of-2026-have-arrived/)
- [Whitespark 2026: Three Insights Every Brand Should Know — Reputation.com](https://reputation.com/resources/articles/whitespark-2026-three-insights-every-brand-should-know)
- [Yelp partners with OpenAI to surface reviews in ChatGPT — Axios, 23 Jul 2026](https://www.axios.com/2026/07/23/yelp-reviews-chatgpt-geo-partnership)
- [ChatGPT gains access to Yelp reviews, ratings, and photos — Search Engine Land](https://searchengineland.com/openai-yelp-deal-483326)
- [Yelp Brings Reservations and Waitlist to ChatGPT — Yelp Official Blog](https://blog.yelp.com/news/yelp-chatgpt-integration/)
- [ChatGPT enables location sharing for more precise local responses — Search Engine Land](https://searchengineland.com/chatgpt-enables-location-sharing-for-more-precise-local-responses-473060)
- [OpenAI ChatGPT Enables Location Sharing — Search Engine Roundtable](https://www.seroundtable.com/chatgpt-location-sharing-41128.html)
- [Google rolls out worldwide agentic restaurant booking via AI Mode — Semrush](https://www.semrush.com/blog/ai-mode-agentic-restaurant-booking/)
- [Use Google Search AI Mode to book restaurants in the UK — blog.google](https://blog.google/company-news/inside-google/around-the-globe/google-europe/united-kingdom/ai-mode-restaurants-uk/)
- [Google AI Mode redesign, agentic booking expands globally — 9to5Google, 10 Apr 2026](https://9to5google.com/2026/04/10/google-ai-mode-redesign/)
- [How Siri Decides Which Businesses to Suggest — The Answer Engine](https://www.theanswerengine.ai/blog/how-siri-decides-which-businesses-to-suggest)
- [Apple Maps Business Listings — Uberall](https://uberall.com/en-us/resources/blog/apple-maps-business-listing)
- [Buy it in ChatGPT: Instant Checkout and the Agentic Commerce Protocol — OpenAI, 29 Sep 2025](https://openai.com/index/buy-it-in-chatgpt/)
- [Stripe powers Instant Checkout in ChatGPT and releases ACP — Stripe Newsroom](https://stripe.com/newsroom/news/stripe-openai-instant-checkout)
- [Agentic Commerce Protocol — GitHub (spec 2026-04-17)](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol)
- [ACP product feed OpenAPI spec](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/tree/main/spec/2026-04-17/openapi)
- [Shopping with ChatGPT Search — OpenAI Help Center](https://help.openai.com/en/articles/11128490-shopping-with-chatgpt-search)
- [OpenAI revamps shopping experience in ChatGPT after struggling with Instant Checkout — CNBC, 24 Mar 2026](https://www.cnbc.com/2026/03/24/openai-revamps-shopping-experience-in-chatgpt-after-instant-checkout.html)
- [OpenAI's first crack at online shopping stumbled — CNBC, 20 Mar 2026](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html)
- [What went wrong with ChatGPT's Instant Checkout — Modern Retail](https://www.modernretail.co/technology/what-went-wrong-with-chatgpts-instant-checkout/)
- [Why OpenAI's Checkout Retreat Spells Trouble — Forbes, 10 Mar 2026](https://www.forbes.com/sites/jasongoldberg/2026/03/10/why-openais-checkout-retreat-spells-trouble-for-its-commerce-strategy/)
- [Why AI Checkout Stalled: Discover in AI, Buy on Site — Digital Applied](https://www.digitalapplied.com/blog/ai-agentic-commerce-discover-in-ai-buy-on-site-2026)
- [Optimizing for ChatGPT Shopping: How product feeds power GEO — Search Engine Land](https://searchengineland.com/optimizing-chatgpt-shopping-463316)
- [ChatGPT Product Feed specification — Lengow](https://www.lengow.com/get-to-know-more/chatgpt-product-feed/)
- [Universal Commerce Protocol — GitHub org](https://github.com/universal-commerce-protocol)
- [Under the Hood: Universal Commerce Protocol — Google Developers Blog](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/)
- [Building the Universal Commerce Protocol — Shopify Engineering](https://shopify.engineering/UCP)
- [Google Shopping launches agentic checkout and more AI shopping tools — blog.google](https://blog.google/products-and-platforms/products/shopping/agentic-checkout-holiday-ai-shopping/)
- [New tech and tools for retailers in an agentic shopping era — blog.google](https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/)
- [Google AI Shopping Features: How to Maximize Your Visibility (2026) — Shopify](https://www.shopify.com/blog/google-ai-shopping)
- [Shop like a Pro — Perplexity Hub](https://www.perplexity.ai/hub/blog/shop-like-a-pro)
- [Perplexity Merchant Program guide — Alhena](https://alhena.ai/blog/perplexity-shopping-merchants-setup-guide/)
- [Conversations that Convert: Copilot Checkout and Brand Agents — Microsoft Advertising, Jan 2026](https://about.ads.microsoft.com/en/blog/post/january-2026/conversations-that-convert-copilot-checkout-and-brand-agents)
- [Microsoft debuts Copilot Checkout — GeekWire, 2026](https://www.geekwire.com/2026/microsoft-launches-copilot-checkout-joining-the-ai-shopping-race-against-amazon-google-and-openai/)
- [Copilot's shopping upgrade adds mobile checkout and 500,000 merchants — Windows Central](https://www.windowscentral.com/microsoft/windows-11/copilots-shopping-upgrade-brings-checkout-to-the-mobile-app-with-deeper-data-from-half-a-million-merchants)
- [Alexa for Shopping (Amazon Rufus): Complete Guide for Brands and Sellers (2026) — Perpetua](https://perpetua.io/blog-alexa-for-shopping-amazon-rufus-the-complete-guide-for-brands-and-sellers/)
- [The 2026 Guide to Optimizing Amazon Listings for Rufus — Sellermetrics](https://sellermetrics.app/amazon-listing-optimization-for-rufus/)
- [AI Shoppers Now Convert 42% Better Than Google Traffic — Digital Applied](https://www.digitalapplied.com/blog/ai-traffic-converts-42-percent-better-2026-channel-strategy)
- [AI Search Visitors Convert 23x Higher — Averi (citing Ahrefs)](https://www.averi.ai/blog/ai-search-visitors-convert-23x-higher.-everyone-s-ignoring-it.)
- [ChatGPT Conversion Rates: 2026 Report — First Page Sage](https://firstpagesage.com/seo-blog/chatgpt-conversion-rates/)
- [Referral Traffic from ChatGPT Hit an All-Time High in May 2026 — SE Ranking](https://seranking.com/blog/chatgpt-referral-traffic-may-2026/)
- [Why ChatGPT traffic converts worse than Google Search — Relevant Audience (counterpoint)](https://www.relevantaudience.com/seo/why-chatgpt-traffic-converts-worse-than-google-search/)
- [AI Search Statistics 2026: 55 Sourced Data Points — Elev8 Operations](https://www.elev8operations.com/guides/ai-search-statistics-for-local-businesses-2026)
