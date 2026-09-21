# AI Search Visibility Playbook (GEO / AEO) — September 2026

**Goal:** get a business's website, shop, or service recommended when someone asks ChatGPT, Google (AI Overviews / AI Mode / Gemini), Perplexity, Copilot, or Claude for "the best X", "X vs Y", "how much does X cost", or "recommend an X in [city]".

**How this was produced:** seven parallel deep-research passes (engine mechanics, technical on-site, content, off-site authority, local + e-commerce, measurement, creative/emerging tactics), ~120 web searches, synthesised on 2026-09-21. The seven detailed reports with full source lists live in [`research/`](research/). Read the caveat in the last section before quoting any number externally.

---

## 1. The ten things that actually matter (ranked by evidence × impact)

| # | Lever | Why it wins | Evidence |
|---|---|---|---|
| 1 | **Be talked about on third-party sites** (listicles, review platforms, news, YouTube, forums) | 79–93% of commercial-query citations go to third parties, not brand sites. Brand mentions correlate with AI visibility at r≈0.66 vs r≈0.22 for backlinks. Earned media = 84% of AI citations. | Strong (multiple 100k+ datasets) |
| 2 | **Get into the specific roundups the engines already cite** for your prompts | Listicles ≈ 22% of all citations, ~41% on commercial prompts. Syndicating one story across third-party sites gave a +239% median citation lift. | Strong |
| 3 | **Make sure AI bots can fetch you at all** (WAF/CDN, robots.txt, server-side rendering) | Cloudflare blocks AI crawlers *by default* on new/free zones since Jul 2025. No AI crawler executes JavaScript. The most common cause of invisibility is a 403 or an empty SPA shell. | Strong (official docs + 500M-fetch study) |
| 4 | **Rank in classic search anyway** | Google page-1 rank correlates ~0.65 with AI brand mentions. Bing index presence is a prerequisite for ChatGPT. C-SEO Bench (NeurIPS 2025) found traditional SEO beats "LLM tricks". | Strong |
| 5 | **Reviews with specific attributes** (GBP, Yelp, Trustpilot, G2) | GBP = 28.5% of local AI citations. Brands with even a tiny Trustpilot profile jump from ~1% to ~53% median citation rate. Yelp reviews are now a licensed feed inside ChatGPT (Jul 2026). Engines extract *attributes* from review prose, not stars. | Strong direction, vendor magnitudes |
| 6 | **Push structured product feeds** (Google Merchant Center, Perplexity Merchant Program, OpenAI feed, Shopify Agentic Storefronts) | Every AI shopping surface prefers a feed over a crawl. Perplexity's program is free and accepts the same Google Shopping CSV. Shopify's channel is a toggle. | Strong (official) |
| 7 | **Titles + first 30% of the page written for the sub-questions** a prompt fans out into | ~79 of every 80 retrieved pages are judged on title + URL + ~200 chars only. Title↔fan-out-query similarity is the top predictor of ChatGPT citation. 44% of citations come from the first 30% of a page. | Strong |
| 8 | **Explicit numbers, dates, prices in visible HTML** | DATE and NUMBER entities best predict citation. "Contact us for pricing" hands the answer to a third party. A 252k-trial factorial found prices and recent dates move citation; formatting alone barely does. | Strong |
| 9 | **YouTube long-form, chaptered, with corrected transcripts** | YouTube mentions are the strongest single correlate of AI visibility (r≈0.74). Views don't matter (r≈−0.03); 40% of cited videos had <1,000 views. Half of AI Overview YouTube citations are timestamped, so chapters = extra citable units. | Moderate–strong |
| 10 | **Measure per engine, with a real prompt panel** | 91% of citations appear on only one engine. Bing Webmaster Tools' Citation Share and Google Search Console's Generative AI report are free first-party data. | Strong |

**What does NOT work (retire these):** `llms.txt` (97% of files never requested, Google says unsupported); schema markup as a *direct* AI lever (controlled test: ~0% change; still do it for Google's Knowledge Graph and Merchant Center); FAQ schema (deprecated by Google May 2026, pages without it were cited slightly more); keyword stuffing (−8.7%); hidden prompt injection (0–31% success, actively defended, legally sanctioned); Reddit-only strategies (ChatGPT's Reddit citations fell ~86% in four days in Aug 2026); blocking `Google-Extended` to opt out of AI Overviews (it doesn't); blocking `GPTBot` thinking it removes you from ChatGPT (it doesn't; `OAI-SearchBot` does).

---

## 2. The mental model: how AI engines decide who to recommend

1. **There is no single AI index. There are five.** Google/Gemini = Googlebot. Copilot = Bingbot. Perplexity = its own crawler (in-house since Apr 2025). **Claude = Brave Search** (an under-contested index). ChatGPT = a hybrid of its own index ("Labrador", incl. vertical indexes for local, shopping, news) plus Bing and third-party SERP providers, shifting in-house through 2026.
2. **The model brainstorms from memory, then retrieves citations.** Seer's 541k-response study: brands are named from training-data knowledge first, sources fetched second. "Citations are the bibliography, not the brainstorm." So: the *parametric* channel (are you a known entity in the corpora models train on: Wikipedia, Reddit, YouTube, review sites, press) decides whether you are *considered*; the *retrieval* channel (crawlability, titles, passages, feeds, freshness) decides whether you are *linked*.
3. **One prompt becomes 8–12 sub-queries** (query fan-out). You compete for each sub-query, not "the keyword". Cover the constraint space (price, city, use case, language, turnaround, "who it's not for").
4. **Retrieval is passage-level.** The unit of competition is a self-contained 100–200-word chunk with the answer in its first sentence, not the page.
5. **Engines rarely open your page.** RESONEO (Aug 2026): ChatGPT opens ~1 page per 80 retrieved, almost only in paid "thinking" mode. Default evidence is title + URL + snippet. When a page *is* opened it gets cited 3 times in 4.
6. **Citation ≠ recommendation.** 62% of citations are "ghost citations" where the brand isn't named. Gemini names brands but rarely links them; ChatGPT links but rarely names. Track *mention/recommendation share* as the primary KPI.
7. **Commerce runs on feeds; local runs on APIs.** ChatGPT local answers come from Foursquare/Yelp partner data (Yelp reviews licensed Jul 2026), Google's from Business Profile/Maps. Your website is the third input, not the first.
8. **Freshness is engine-specific.** ChatGPT cites content ~400+ days newer than Google organic; ~half of AI citations are <13 weeks old; median citation half-life ≈ 4.5 weeks. Google AI Overviews barely care (cite content 16 days *older* than organic).
9. **Training vs retrieval are separate robots.txt decisions.** OpenAI, Anthropic and Perplexity each split *training* (`GPTBot`, `ClaudeBot`), *search index* (`OAI-SearchBot`, `Claude-SearchBot`, `PerplexityBot`), and *live user fetch* (`ChatGPT-User`, `Claude-User`, `Perplexity-User`). Never block the search/user ones.

---

## 3. The 90-day plan (any business)

### Week 1 — Access and baseline (hours of work, highest impact)
- [ ] `curl -A "OAI-SearchBot/1.0"` (and `ChatGPT-User`, `PerplexityBot`, `ClaudeBot`, `Googlebot`, `bingbot`) against your key pages. Must be HTTP 200 with full HTML, not 403/429 or a "Checking your browser" shell. Fix at the CDN/WAF, not in robots.txt.
- [ ] If on Cloudflare: open **AI Crawl Control** and check the Search / Agent / Training toggles (default-blocked on new/free zones since 1 Jul 2025; further tightening 15 Sep 2026).
- [ ] robots.txt: explicitly `Allow` retrieval + user bots; decide training bots separately (see `research/02-technical.md` for a ready-to-ship file).
- [ ] Disable JavaScript and reload key pages. Prices, stock, reviews, hours, service areas must be in the raw HTML.
- [ ] No `nosnippet` / `max-snippet:0`. Set `max-snippet:-1, max-image-preview:large`.
- [ ] Verify in **Bing Webmaster Tools** (AI Performance + Citation Share reports) and confirm Bing has your pages indexed. Verify GSC and check the Generative AI report.
- [ ] Run your 30 highest-intent conversational prompts in ChatGPT, Google AI Mode, Perplexity, Gemini and Claude (logged out). Log (a) are you *named*, (b) every cited URL. This is your baseline and your outreach target list.

### Weeks 2–4 — Extractable facts and entity
- [ ] Publish a real **pricing page** (numbers or ranges, dated table, what moves the price).
- [ ] Rewrite titles/H1/H2s of commercial pages as the *questions* buyers ask; put the answer in the first 1–2 sentences under each heading. Add a "Short answer" block at the top.
- [ ] Add a **facts/at-a-glance page**: founding date, registry/VAT ID, address, service area, hours, certifications, languages, headcount.
- [ ] One canonical boilerplate sentence — "[Brand] is a [category] in [city] that [does X] for [whom]" — used verbatim on the site, GBP, LinkedIn, directories, schema `description`.
- [ ] `Organization` (+ `LocalBusiness`/`Service` or `Product`/`Offer`) JSON-LD with stable `@id` and `sameAs`; create a **Wikidata** item (low notability bar; Wikipedia only if genuinely notable, never self-edited).
- [ ] Honest **"X vs Y"** and **"alternatives to [competitor]"** pages that name where the competitor wins. Comparative content yields 2.4× more brand mentions.
- [ ] (Shops) Google Merchant Center + Microsoft Merchant Center feeds with correct GTINs; Perplexity Merchant Program (free); Shopify Agentic Storefronts toggle; OpenAI product feed if US-eligible.
- [ ] (Local) Complete GBP (primary category, full services list with prices, attributes, Q&A, holiday hours), Yelp, Foursquare, Apple Business Connect, Bing Places. Identical NAP everywhere.

### Months 2–3 — Earn the third-party corpus
- [ ] From the Week-1 citation log, cluster the 5–15 **listicles/roundups** that recur. Pitch inclusion: one sentence on what you are, one differentiator, one ask, plus something real (fresh 2026 pricing, a screenshot set, a free account). Re-check quarterly.
- [ ] **Review engine**: ask at the moment of value, no incentives, open question about the *specific job* ("what did we fix, where?"). Reply to every review restating the service and city.
- [ ] **YouTube**: 6–10 long-form videos, each titled as one prompt ("How much does X cost in [city] in 2026"), with chapters named as sub-questions and a corrected transcript; republish the transcript on your site.
- [ ] **Expert quotes / digital PR**: Qwoted, Featured.com, Help a B2B Writer; local news for local businesses. Unlinked mentions count almost as much as linked ones.
- [ ] **One original data study** yielding a quotable statistic, packaged as a named, dated number, then syndicated.
- [ ] **Community**: a real, disclosed employee answering questions they're qualified on, in threads that already get cited (Reddit, niche forums, Quora, Stack Exchange). 9:1 helpful-to-promotional. No sockpuppets.
- [ ] **Paid**: test **ChatGPT Ads** (self-serve since May 2026, no minimum spend, contextual targeting, currently under-competed). Google AI Mode ads run through existing Google Ads. Perplexity has **no** ad path (ads killed Feb 2026).
- [ ] Day 90: re-run the Week-1 prompt panel, per engine, with confidence intervals. Compare to baseline.

---

## 4. By business type

### Local service business (dentist, plumber, clinic, agency, salon, restaurant)
Reality check: ChatGPT recommends only ~1.2% of locations that appear in Google's local 3-pack (35.9%). Local AI recommendation is scarcer and more concentrated than local search.

| Priority | Action | Why |
|---|---|---|
| 1 | GBP complete to the field; **hours exactly right incl. holidays** ("open at time of search" is now a top-5 local factor) | GBP = 28.5% of local AI citations; Gemini/AI Mode treat it as the entity record |
| 2 | Yelp + Foursquare + Apple Business Connect + Bing Places claimed and rich | Yelp is a licensed ChatGPT feed; Foursquare reportedly backs most ChatGPT local results; Siri runs on Apple Maps |
| 3 | Review velocity + **attribute-rich review text** ("emergency", "English-speaking", "same-day", "good with kids") + reply to all | Engines shortlist on extracted attributes |
| 4 | Website: "cost of X in [city]" pages, genuine service-area pages, FAQ, staff bios with credentials | Whitespark 2026: on-page content is the *heaviest* AI-visibility input (24%) while GBP drops to 12% there. Profile wins the map pack; website wins the AI answer |
| 5 | Be **bookable by machine**: Reserve with Google / OpenTable / Resy / Zocdoc / Fresha, stable booking URL, real phone | Google AI Mode agentic booking went global Apr 2026; Yelp reservations run inside ChatGPT |
| 6 | Local listicles ("best X in [city]"), Three Best Rated-style curated directories, chamber of commerce, local news | Curated lists punch far above their traffic in ChatGPT local citations |
| 7 | Thumbtack / Angi / TaskRabbit (home services) | Native ChatGPT and Claude integrations |
| 8 | Track by location grid, not globally | Local answers vary >50% by city; ChatGPT has precise location sharing since Mar 2026 |

### E-commerce / product seller
| Priority | Action | Why |
|---|---|---|
| 1 | Feeds everywhere: Merchant Center (GTINs correct, price == page price), Microsoft Merchant Center, Perplexity Merchant Program, Shopify Agentic Storefronts, OpenAI feed (US) | Feeds beat crawling on every surface; OpenAI's own post-mortem said scraping couldn't reflect real price/stock |
| 2 | Get into editorial "best X for Y" roundups and YouTube reviews | Retailers take only 2.9% of shopping citations; editorial + YouTube + Reddit form the recommendation. Amazon is where 50.9% of AI-influenced purchases *close*, not where they're discovered |
| 3 | Product pages for extraction: spec `<table>`, explicit "best for [use case]" sentences, UGC reviews in crawlable HTML, Q&A block, plain-text shipping/returns | UCP elevates returns/support policy to a machine-readable signal |
| 4 | Your own honest "best X for Y" guides including competitors | Earns comparison-prompt citations, not just product cards |
| 5 | Trustpilot profile + steady specific reviews | The 1% → 53% median citation jump |
| 6 | Amazon sellers: Rufus reads attributes, title/bullets, A+ content, reviews, Q&A, and text inside infographic images | Six inputs; keyword stuffing doesn't help, prose does |
| 7 | Do **not** architect around in-chat checkout | OpenAI retired Instant Checkout 24 Mar 2026 after ~30 merchants went live; Walmart saw ~3× worse conversion. "Discover in AI, buy on site" is the 2026 default. UCP (Google + Shopify, Jan 2026) onboards via Merchant Center and is consumed by AI Mode, Gemini, Copilot *and* ChatGPT |

### B2B / SaaS
| Priority | Action | Why |
|---|---|---|
| 1 | G2, Capterra, Gartner Peer Insights, Software Advice, TrustRadius profiles with sustained review velocity | 88% of review-platform citations in AI Overviews; AI chatbots are now the #1 shortlist influence |
| 2 | "Best [category] software 2026" listicle placements | ~41% of commercial-query citations |
| 3 | Public docs + help centre, thorough and crawlable | The biggest winner of the Aug 2026 ChatGPT reallocation; 63% of Claude citations are docs/practitioner content |
| 4 | Annual original benchmark study, syndicated | +239% distribution lift, statistics +30–40% |
| 5 | Third-party "X vs Y" and "alternatives to X" placements | 2.4× mention rate |
| 6 | Founder/expert quotes (Qwoted, Featured), LinkedIn cadence, video podcasts | Journalism = 25–27% of citations; LinkedIn is top-5 cited |
| 7 | Public MCP server / ChatGPT app / Claude connector | Positioning and developer lead-gen only; no evidence of consumer discovery |

---

## 5. Per-engine cheat sheet

| Engine | Index | Robots.txt tokens (never block the bold ones) | Source personality | Levers specific to it |
|---|---|---|---|---|
| **ChatGPT** | Own "Labrador" indexes + Bing + SERP providers, shifting in-house | `GPTBot` (train), **`OAI-SearchBot`** (search), **`ChatGPT-User`** (live) | Since Aug 2026: help centres, docs, editorial (Forbes, Reuters, NYT); Wikipedia heavy; Reddit collapsed to <1%. Strongest freshness bias. Cites ~15 sources per answer, names few | Bing index presence; title↔sub-query match; natural-language URL slugs (89.8% vs 81.1% citation rate); Yelp/Foursquare data for local; product feed; ChatGPT Ads |
| **Google AI Overviews / AI Mode / Gemini** | Googlebot | `Google-Extended` (Gemini training only; does NOT remove you from AIO). `nosnippet`/`max-snippet:0` does | ~43% of citations to Google properties incl. YouTube; oldest domains; Reddit still ~21% of AIO. Names brands (83.7%) far more than it cites them (21.4%). Weakest freshness bias | Classic rankings (76% → 38% top-10 overlap, still the majority path); GBP; Merchant Center / Shopping Graph; YouTube with chapters; schema feeds Knowledge Graph; GSC Generative AI report |
| **Perplexity** | Own crawler (Vespa), in-house since Apr 2025 | **`PerplexityBot`**, **`Perplexity-User`** | Review/comparison aggregators (G2, Gartner, TripAdvisor, Yelp), YouTube (38.7% of its YouTube citations), new content within hours. ~45% of cited pages have minimal organic traffic — structure beats authority here | Structural fit to query; freshness; free Merchant Program (Google Shopping CSV via SFTP, 0% commission); **no ads exist** |
| **Copilot** | Bingbot (no separate crawler) | `bingbot` | Closest to Bing organic | Bing Webmaster Tools (AI Performance, Citation Share); IndexNow; Microsoft Merchant Center; Copilot Checkout (Shopify auto-enrolled) |
| **Claude** | **Brave Search API** (~87% citation match with Brave) | `ClaudeBot` (train), **`Claude-SearchBot`**, **`Claude-User`** | 63% niche docs/practitioner content, 7% news; no YouTube access; iterative searching | Get indexed by Brave (far less contested); transcripts as HTML; docs |

---

## 6. Content rules (the retrievable unit)

1. **Sentence 1 = the answer**, containing entity + category + a number or date. Sentences 2–3 = evidence with source and year. Sentence 4 = the boundary condition (when this is *not* true). 120–180 words; no pronoun refers outside the paragraph.
2. **Headings phrased as the prompt** ("How much does an emergency plumber cost in Ljubljana?"), answer in the first 40–80 words.
3. **Focused pages beat "ultimate guides"**: pages covering 26–50% of a prompt's sub-queries were cited more than pages covering 100%. Build a *set* of narrow pages.
4. **Tables and lists** for anything comparable. Include your own weaknesses in comparisons; self-serving lists get cited but the engine recommends a competitor 69% of the time.
5. **Visible, honest "Updated [date]"** backed by real edits. Refresh pricing/best-of quarterly, comparisons when a competitor changes, stats annually. Date-stamping without change is a spam signal.
6. **Readability ≈ grade 16**, not 19. Professional but plain.
7. **Non-English markets**: engines prefer passages in the query language (85% local-language citation share in AIO, 52% in Grok). Translated sites saw up to 327% more AIO visibility. Publish local-language pages on distinct URLs with `hreflang`, localise the *facts* (prices, neighbourhoods, regulations), keep English too. For Serbian: Latin script canonical, Cyrillic brand string present once. Seed local directories, local press, local YouTube. (No Serbian/Croatian-specific study exists; this is extrapolation.)

Page skeletons, JSON-LD examples, a robots.txt, an llms.txt (harmless, inert), nginx markdown negotiation and a fetch-as-bot script are in `research/02-technical.md` and `research/03-content.md`.

---

## 7. Measurement stack (free first)

| Layer | Answers | Tool |
|---|---|---|
| Server / CDN logs | Are AI bots fetching me? Training vs index vs **live user fetch** (`ChatGPT-User`, `Perplexity-User` hits = zero-latency citation alerts) | grep/awk on access logs; Cloudflare AI Audit; verify by published IP ranges + reverse DNS |
| GA4 | Does it convert? | Native "AI Assistant" channel (May 2026, excludes Perplexity) + custom regex channel group; don't strip `utm_source=chatgpt.com` |
| Platform reports | Did I appear? | GSC Generative AI performance report (impressions only, global since 31 Aug 2026); **Bing Webmaster Tools Citation Share** (the only census-level share-of-voice metric anywhere; leading indicator for ChatGPT) |
| DIY prompt panel | Share of voice, citation rate, position, who's cited instead | OpenAI Responses `web_search` + Perplexity Sonar + Gemini grounding; ~$30–120/mo for 5 runs × 3 engines × 100 prompts; store raw answers; reference implementation: `elmohq/elmo` (MIT) |
| Commercial | Consumer-surface parity, prompt volumes | Otterly ($29+), Peec (~€89+), Semrush AI Toolkit ($99, only 25 prompts — too few), Ahrefs Brand Radar ($398 + base), Profound ($499+), Scrunch |

Rules: spend budget on prompts > engines > locales > repeats (stop at ~5 repeats); report Wilson confidence intervals; **report per engine, never blended** (a 13-week test found +11% on Claude/Perplexity and zero on ChatGPT/AIO); distrust the "4.4×/23× conversion" stats (self-selection); expect time-to-effect of days (Perplexity), 2–4 weeks (ChatGPT), 4–8 weeks (Google).

The highest-ROI query in the whole discipline: *which domains win the prompts where I'm invisible?* Cluster those URLs — they are your outreach list.

---

## 8. Creative / emerging plays worth a small bet

- **Brave index for Claude**: nobody optimises for it; Claude's search is Brave.
- **Podcast guesting at scale**: every episode = transcript + show notes + auto-captions, i.e. several independently hosted documents where your brand sits next to category terms.
- **Become the source**: a niche directory, pricing benchmark, or regulatory tracker that AI cites as *the* reference for a category. Highest effort, converts you from vendor to authority.
- **Own the citable statistic**: one real number ("X% of [industry] still …") that journalists repeat.
- **ChatGPT Ads early**: no minimum, contextual, $1B run-rate in <200 days means the market is arriving; CPCs are still low.
- **Markdown mirrors** (`/page.md`, `Accept: text/markdown`): useful for coding agents and agentic browsers, not for search indexing; only with identical content (otherwise cloaking).
- **"Prompt seeding"** (telling customers what to ask): free, unproven, harmless.

## 9. Grey / black hat: documented to fail
Hidden prompt injection (0% success on Claude, 31% on Gemini Flash, actively defended, a litigant was sanctioned for it in Jul 2026); mass AI-generated pages (scaled content abuse); parasite pages (algorithmic site-reputation-abuse enforcement since Aug 2025); fake reviews (FTC rule, up to $53,088 per violation, first enforcement Dec 2025); astroturfed Reddit "citation hijacking" (Reddit catches ~25k spam posts/day, press is naming offenders, and the channel's ChatGPT value collapsed anyway); self-editing Wikipedia (COI enforcement). Terrible risk asymmetry in every case.

---

## 10. Evidence quality and what to re-verify

**Research constraints:** the environment's network policy blocked direct fetching of nearly every primary source (OpenAI, Google, Cloudflare, Ahrefs, Semrush, Search Engine Land, arXiv…); only GitHub was readable in full. The shared web-search budget was exhausted mid-run. Every figure therefore comes from search-result extracts plus corroborating secondary coverage, and the detailed reports tag each claim (confirmed / vendor / speculative / prior-knowledge). **Before quoting a number externally, open the primary URL listed in the relevant report.**

Load-bearing facts to re-verify first: the ChatGPT Instant Checkout retirement date and merchant count (reports disagree: ~12 vs ~30); the exact OpenAI merchant-feed fields (the open ACP feed spec has *no* rating/review fields, contrary to vendor guides); Cloudflare's 15 Sep 2026 "mixed-use crawler" default; the Foursquare "70% of ChatGPT local results" claim (single vendor source); all conversion-rate multiples; Whitespark's AI-visibility weightings (seen via secondary summaries).

**Where sources genuinely disagree:** Google top-10 ↔ AI Overview citation overlap (76% → 38% → 17% depending on study, date and whether "citation" means inline or sidebar; all agree the trend is down); schema's effect (2.5× cross-sectional vs ~0% controlled — the controlled test is better designed); word count (long pages get *retrieved* for more sub-queries, but the *cited* chunk is early and short).

**Detailed reports:**
1. [`research/01-mechanics.md`](research/01-mechanics.md) — indexes, crawlers, fan-out, what gets read, studies, feeds, local APIs
2. [`research/02-technical.md`](research/02-technical.md) — robots.txt, CDN pitfalls, SSR, schema, llms.txt, hreflang, pre-launch checklist, code
3. [`research/03-content.md`](research/03-content.md) — GEO paper vs C-SEO Bench, prompt inventory, page types, writing templates, freshness, YouTube, non-English
4. [`research/04-offsite.md`](research/04-offsite.md) — which domains get cited per engine, Reddit, reviews, PR, listicles, playbooks by business type
5. [`research/05-local-ecommerce.md`](research/05-local-ecommerce.md) — GBP, Yelp/Foursquare, agentic booking, ChatGPT shopping reset, UCP vs ACP, Perplexity/Copilot/Rufus, product pages
6. [`research/06-measurement.md`](research/06-measurement.md) — metrics, sample size, tools and pricing, GA4/GSC/Bing, log analysis, DIY tracker code, experiments
7. [`research/07-creative-emerging.md`](research/07-creative-emerging.md) — 29-tactic scorecard, agent-native presence, entity engineering, paid paths, black hat, next 12 months
