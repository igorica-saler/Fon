# Creative, High-Leverage and Emerging Strategies for Getting Recommended by AI Assistants
**Research date: 2026-09-21** · Scope: ChatGPT, Google AI Mode/Gemini, Perplexity, Microsoft Copilot, Claude

---

## Method & honesty note

This report separates three tiers of evidence:

- **[A] Strong** — peer-reviewed/preprint controlled experiments, large-n correlational studies (100k+ domains), or first-party platform documentation.
- **[B] Moderate** — credible trade journalism with named sources and dates, or vendor studies with disclosed methodology.
- **[C] Weak** — vendor content marketing, agency case studies with unfalsifiable numbers, single-anecdote claims. **The GEO/AEO content ecosystem is overwhelmingly tier C and is itself an SEO play.** Treat any "we increased AI visibility 400%" post as marketing until methodology is shown.

Two research constraints apply to this document: the session's web-search budget was exhausted partway through, and direct page fetching was blocked by the network egress proxy. Claims below are marked **[verified this session]** where they come from live search results retrieved today, and **[prior knowledge]** where they come from the model's training data (cutoff May 2026) and could not be re-verified. Anything unmarked in the analysis is interpretation, not fact.

---

## Tactic scorecard

Impact / Effort / Risk on 1–5 (5 = highest). Evidence = strength of public proof it works.

| # | Tactic | Impact | Effort | Risk | Evidence |
|---|---|---|---|---|---|
| 1 | Get into third-party listicles/roundups that already rank for your category | 5 | 3 | 1 | **Strong** |
| 2 | Be present in the retrieval set — classic rank/indexation for the underlying query | 5 | 4 | 1 | **Strong** |
| 3 | Explicit prices, dates, specs as extractable facts on-page | 4 | 2 | 1 | **Strong** |
| 4 | Product feed / catalog syndication to agentic channels (Shopify Agentic Storefronts, Merchant Center, UCP) | 5 | 2 | 1 | **Strong** (if you sell products) |
| 5 | Original research → the "citable statistic" | 4 | 4 | 1 | Moderate |
| 6 | Comparison / "X vs Y" / "alternatives to [competitor]" pages | 4 | 2 | 2 | Moderate |
| 7 | Entity engineering (Wikidata, Crunchbase, consistent descriptors, Knowledge Panel) | 3 | 3 | 2 | Moderate |
| 8 | Publishing your own honest "best X in [city]" roundup incl. competitors | 3 | 2 | 2 | Moderate |
| 9 | Niche directory / resource hub that becomes the cited source | 4 | 5 | 2 | Moderate |
| 10 | Podcast guesting at scale (transcripts → co-occurrence corpus) | 3 | 3 | 1 | Moderate |
| 11 | Freshness signals: dated updates, changelogs, "2026" in title, news pages | 3 | 2 | 2 | Moderate |
| 12 | Digital PR aimed at news-index inclusion | 4 | 4 | 2 | Moderate |
| 13 | Genuine, disclosed participation in Reddit/community threads AI already cites | 3 | 3 | 4 | Moderate → **declining** |
| 14 | ChatGPT Ads (self-serve, live since May 2026) | 4 | 2 | 1 | **Strong** (it's a product) |
| 15 | Google AI Mode / AI Overviews ads via existing Google Ads | 4 | 2 | 1 | **Strong** |
| 16 | Public MCP server for your business (booking/catalog/availability) | 3 | 4 | 1 | Weak (early, few users) |
| 17 | ChatGPT App (Apps SDK) / Claude connector / Gemini Gem | 3 | 4 | 1 | Weak–Moderate |
| 18 | ACP / Instant Checkout integration | 2 | 4 | 2 | Weak (see §2) |
| 19 | Schema.org structured data for AI retrieval specifically | 2 | 2 | 1 | Weak–Moderate |
| 20 | llms.txt | 1 | 1 | 1 | **Weak → refuted** |
| 21 | Markdown mirrors / serving .md to AI user-agents | 2 | 2 | 2 | Weak |
| 22 | "Prompt seeding" (telling customers what to ask the AI) | 2 | 1 | 1 | Weak (anecdotal) |
| 23 | Long-tail conversational phrasing alignment | 3 | 2 | 1 | Weak–Moderate |
| 24 | Astroturfed Reddit/forum posts ("citation hijacking") | 2 | 3 | **5** | Moderate that it worked; strong that it's collapsing |
| 25 | Hidden text / prompt injection in page content | 1 | 2 | **5** | **Refuted** |
| 26 | Parasite pages on high-authority domains | 2 | 3 | **5** | Moderate short-term; strong penalty risk |
| 27 | Mass AI-generated pages | 1 | 2 | **5** | **Refuted** (scaled content abuse) |
| 28 | Fake reviews / review-bombing competitors | 1 | 3 | **5** | **Refuted** + legal exposure (FTC) |
| 29 | Wikipedia editing for your own brand | 1 | 3 | **4** | **Refuted** (COI enforcement) |

---

## 1. What the evidence actually says drives AI recommendations

Strip away the vendor noise and the 2026 academic literature converges on an unglamorous conclusion: **citation is mostly a retrieval problem, not a content-formatting problem.**

A critical survey of GEO covering 2023–2026 (arXiv 2607.14035, dated 15 July 2026) reports that a factorial experiment by Vishwakarma et al. (2026) — **252,000 trials across six LLMs and eighteen factors** — found **relevance and position to be the primary determinants of which source gets cited first**. Puerto et al. (2025) found that *moving a source higher in the retrieved context has a greater effect than most content rewrites.* The same survey notes that "statistics, definitions, comparisons, prices, dates, and references" have a plausible advantage because they form discrete units a generator can select and attribute — and that **explicit prices and recent dates showed measurable effects while formatting changes alone showed weak effects**. [verified this session; tier A]

The practical translation:

1. **You cannot be cited if you are not retrieved.** Being in the top ~10 organic results (or in the platform's licensed/structured data feed) for the query the assistant silently fires is still the dominant lever. Classic SEO isn't "beyond"; it's the floor.
2. **Once retrieved, extractability decides whether you're quoted.** A page with an explicit price, a dated figure, a named comparison, and a specific claim gets lifted verbatim. A page of adjectives does not.
3. **Most citations aren't yours.** The frequently repeated split — roughly **68% of AI citations from third-party sources vs 32% brand-owned** — comes from a vendor (llmrefs.com) and is tier C, but it is directionally consistent with every crawl-based study: assistants prefer to cite the roundup, the review, the forum, the news article. **Your highest-leverage asset is the third-party corpus about you, not your own site.**

Related 2026 preprints worth reading: arXiv 2603.09296 (*Diagnosing and Repairing Citation Failures in GEO*), arXiv 2604.25707 (*From Citation Selection to Citation Absorption*), arXiv 2606.12439 (a position paper arguing GEO creates concentration and disclosure risks needing governance). [verified this session — titles/URLs confirmed]

---

## 2. Agent-native presence: what a small business can realistically do in 2026

This is the most hype-inflated area and also the one with the single clearest ROI for product sellers.

### The real win: catalog syndication

**Shopify Agentic Storefronts** is a sales channel inside Shopify admin that connects a merchant's products to AI shopping platforms — "products syndicated across every major AI channel automatically, no apps to install, no custom integrations," covering ChatGPT, Microsoft Copilot and the Shop App via **Global Catalog** (query products across all Shopify merchants) and **Storefront Catalog** (scoped to one merchant). [verified this session: shopify.dev/docs/agents; help.shopify.com/en/manual/online-sales-channels/agentic-storefronts]

**This is the single highest impact/effort ratio item in the entire report for an e-commerce SMB.** It is a toggle, not a project. The equivalent for non-Shopify merchants is a clean Google Merchant Center feed, which now feeds UCP.

### The protocol layer (and why you should mostly ignore the drama)

- **ACP (Agentic Commerce Protocol)** — OpenAI + Stripe, released Apache 2.0 on **29 Sept 2025**; Etsy US sellers first, then Shopify merchants (Glossier, Vuori, Spanx, SKIMS). PayPal joined as a payment provider **28 Oct 2025**; Stripe shipped the Agentic Commerce Suite **11 Dec 2025**. Still beta, date-versioned, latest stable snapshot **2026-04-17**. [verified this session]
- **The cautionary data point:** reporting indicates **ChatGPT Instant Checkout was retired in March 2026 after only about a dozen Shopify merchants ever shipped against it**, though the protocol itself continued. [verified this session via search summaries of digitalcommerce360.com 2026-03-06 and ekamoira.com; the primary page was egress-blocked — treat as **tier B, verify before acting**.] If accurate, this is the report's best "what didn't work" example at the platform level: an open protocol with enormous PR reach and near-zero merchant adoption, because merchants would not rebuild checkout for an unproven channel.
- **UCP (Universal Commerce Protocol)** — Google + Shopify, announced by Sundar Pichai at **NRF on 11 January 2026**, with Etsy, Wayfair, Target and Walmart and 20+ retailers/networks endorsing. Updated **19 March 2026** with Cart, Catalog and Identity Linking plus **onboarding through Merchant Center**; expanded **20 May 2026** with a multi-retailer Universal Cart paid via Google Pay or the retailer's own checkout. Rolling out across Google AI Mode, the Gemini app, Microsoft Copilot and ChatGPT. [verified this session: shopify.engineering/UCP; semrush.com/blog/universal-commerce-protocol/]

**Verdict:** UCP has displaced ACP as the centre of gravity because its onboarding path is Merchant Center — something SMBs already have. Do not engineer against a protocol directly. Get your feed clean, turn on the channel your platform gives you, and let the platform absorb protocol churn.

### MCP servers, apps and connectors

MCP has become the universal "give an agent a tool" interface, and registries exist (**Smithery** — largest public catalog; **Glama**; AWS Bedrock Agent Registry for enterprise governance). [verified this session] Surfaces where a business can plug in: **ChatGPT Apps SDK / apps directory, custom GPTs, Claude connectors and skills, Gemini Gems/Extensions** [prior knowledge].

Honest assessment for an SMB: **a public MCP server is a brand/positioning play and a B2B lead magnet, not a discovery channel.** Nobody browses MCP registries looking for a dentist. There is no credible public evidence of an MCP server driving meaningful consumer bookings in 2026. It is worth building if (a) you are a software/API business whose buyers are developers, or (b) you want your own customers' agents to transact with you post-discovery. The discovery still has to come from §1, §4 and §5.

Payments rails — **Google AP2, Visa Intelligent Commerce, Mastercard Agent Pay** [prior knowledge] — are infrastructure you consume through your PSP. No direct SMB action.

**Rating: catalog syndication 5/2/1 (strong). MCP server 3/4/1 (weak). ACP direct integration 2/4/2 (weak).**

---

## 3. Structured "AI-facing" content: what's real and what's cargo cult

### llms.txt is dead — this is now well-evidenced

- **SE Ranking** analysed roughly **300,000 domains** and found **no statistically significant correlation** between having llms.txt and citation frequency; removing the variable *improved* their XGBoost model's accuracy.
- **Ahrefs** found **97% of llms.txt files across 137,000 sites received zero traffic in May 2026.**
- **Google stated at Search Central Deep Dive (Bangkok, July 2025) that it does not support llms.txt and has no plans to.** John Mueller has compared it to the keywords meta tag.
[all verified this session: seranking.com/blog/llms-txt/; 1clickreport.com/blog/llms-txt-evidence-2026; contentful.com/blog/llms-txt-search-visibility/; webyes.com/blogs/does-llms-txt-improve-rankings/]

Note the conflict worth flagging: a vendor (llmrefs.com) claims comparison tables, "llm.txt" and FAQ schema each drive **28–34% coverage lift within 14–21 days** across 500+ brands. This is tier C, contradicts two large-n studies on the llms.txt component, and should not be believed. The *comparison table* component is independently supported; the llms.txt component is not.

**Do it anyway?** It costs 20 minutes and carries no risk. But budget zero expectation and never sell it to a client as a driver.

### What does belong in the "AI-facing content" bucket

Supported by the extractability finding in §1: **explicit pricing tables** (not "contact us for pricing"), **comparison matrices**, **a facts/at-a-glance page** with NAP, hours, service area, certifications, founding date, headcount; **dated figures**; **a public API or downloadable dataset** if you have data worth citing. These work not because bots parse them specially, but because they produce liftable units.

### Content negotiation / markdown for bots — proceed carefully

Serving `text/markdown` or a `/page.md` mirror to AI user-agents is increasingly common and defensible when **the content is identical**. The moment the markdown version differs in substance from the HTML a human sees, it is **cloaking** under Google's spam policies. Safest pattern: publish `.md` mirrors as discoverable URLs linked from the HTML — same content, both addressable, no user-agent branching. **Rating 2/2/2, weak evidence.**

---

## 4. Entity engineering

Assistants resolve "best X in Y" partly through entity graphs. The disambiguation layer — **Wikidata item, Crunchbase, LinkedIn company page, Google Business Profile, consistent one-sentence descriptor used verbatim everywhere** — is cheap and reduces the odds an assistant confuses you with a similarly named firm. A **Wikipedia article requires genuine notability** (significant coverage in independent reliable sources); pursuing one without it is wasted effort and, if you edit it yourself, actively harmful (see §10). **Rating 3/3/2, moderate.**

The higher-leverage and more interesting version is **co-occurrence seeding**: engineering the situation where your brand name appears in the same documents as your category terms and your best-known competitors, across many independent domains. Concretely: getting listed in third-party roundups, being quoted in trade press about the category, appearing in "alternatives to [big competitor]" articles you did not write, and **podcast guesting at scale** — each episode produces a transcript, show notes and often a YouTube auto-caption, creating multiple independently-hosted documents where your name sits beside your category language. It is slow, legitimate and compounds. **Rating 3/3/1.**

**The "citable statistic" play** deserves separate emphasis. Publishing original research that produces a specific, quotable number ("X% of dental practices still fax referrals") creates an asset journalists cite, which creates the third-party documents assistants prefer to quote (§1). It's the closest thing to a self-reinforcing loop in legitimate GEO. Requires real data and real rigour; fabricated stats are both detectable and reputationally fatal. **Rating 4/4/1, moderate.**

---

## 5. Listicles, roundups, comparisons and owned directories

Given that the large majority of AI citations come from third-party pages, **the roundup is the atomic unit of AI recommendation.** For "best CRM for nonprofits," the assistant retrieves the roundups and synthesises them. If you're not in them, you don't exist.

Three moves, in descending evidence strength:

1. **Get into existing roundups.** Identify the pages actually cited in answers for your target prompts (any AI-visibility tracker, or just run the prompts and read citations), then approach those publishers with a genuine reason to be added — a differentiated feature, a price point, a customer segment they've omitted. Slow, unglamorous, highest ROI. **5/3/1.**
2. **Build comparison and alternatives pages.** "X vs Y" and "alternatives to [big competitor]" pages are retrieved for exactly the phrasing people use conversationally, and comparison tables are among the formats with the best independent support. The risk is credibility: assistants and readers both discount self-serving comparisons, so include cases where the competitor wins. **4/2/2.**
3. **Publish your own honest roundup including competitors.** Counterintuitive but effective: a genuinely fair "best X in [city]" page that ranks and lists rivals is exactly the document shape assistants like to retrieve, and you control the framing and the criteria. It only works if it is honest — a roundup where you happen to be #1 is transparent and gets discounted. **3/2/2.**
4. **Build a niche directory or resource hub** that becomes *the* reference for a category (a regulatory-deadline tracker, a pricing benchmark, a standards index). Highest effort in the report, but it converts you from "a vendor" into "the source." **4/5/2.**

---

## 6. Reddit and communities: the window is closing

**What was true:** between **August 2024 and June 2025, Reddit was the most-cited domain by Google AI Overviews and Perplexity and second-most by ChatGPT**, with AI Overviews citing Reddit in about **21% of responses**; roughly **84% of recent brand mentions in AI answers traced to the top 100 subreddits**. [verified this session: cmswire.com; mediapost.com article 416312]

**What happened next — the most important single datapoint in this report:** Reddit averaged **3.83% of ChatGPT Search citations from 18 July to 7 August 2026, then fell below 1% by 14 August, averaging 0.52% — an 86.4% drop in days.** Over the same window Reddit citations fell **11% in Google AI Overviews and 31% in Google AI Mode.** Attributed causes: OpenAI's model shifting to heavier use of `site:` operators against specific domains, and reporting that Reddit blocked crawlers at the domain level via robots.txt. Reddit's stock fell on the news. [verified this session: searchengineland.com/reddit-chatgpt-search-citations-fall-report-485473; axios.com/2026/08/20/chatgpt-reddit-citations-geo-strategy; forbes.com 2026-08-20]

**And the abuse:** peptide and HRT companies were documented flooding communities like r/Biohackers with **coordinated promotional posts designed to be scraped and surfaced by ChatGPT and AI Overviews** — openly discussed in the industry as "citation hijacking" or "AI citation poisoning": planting short, natural-sounding text inside threads that assistants already cite, to control the answer without buying an ad. [verified this session: mediapost.com 416312; pymnts.com 2026 "Fake Reddit Posts Are Hijacking What AI Tells You"; thedrum.com]

**Assessment.** The tactic demonstrably worked for a window. It is now simultaneously (a) less valuable, because the citation share collapsed, (b) more dangerous, because Reddit's spam enforcement, subreddit moderators and journalists are actively hunting it, and (c) a reputational liability once named in press. The legitimate version — a real employee, clearly identified, answering a question they're genuinely qualified on, in a subreddit that permits it — is still worth doing and carries most of the upside with none of the ban/exposure risk. **Legitimate: 3/3/4, moderate. Astroturfed: 2/3/5, do not.**

Strategic implication beyond Reddit: **single-platform citation dependency is fragile.** An 86% swing in four days from one vendor's retrieval change is the whole argument for spreading across many independent third-party domains rather than optimising one.

---

## 7. Prompt-level tactics

**Conversational phrasing alignment** is the defensible half: people ask assistants full questions with constraints ("cheapest accountant in Leeds that handles crypto"), so headings and page copy phrased as those questions, answered immediately in the first sentence, map better onto retrieval. This is a repackaging of long-tail SEO and is worth doing. **3/2/1, weak–moderate.**

**"Prompt seeding"** — printing suggested prompts in marketing ("Ask ChatGPT: what's the best X for Y?") — has **no credible public evidence** of moving citations. The mechanism people imagine (user prompts training the model to favour you) does not exist in mainstream deployments. Two honest reasons to still do it: it's free, and it converts customers who then verify you via an assistant that already cites you. **2/1/1, weak.** Do not confuse activity with evidence here.

---

## 8. Freshness

The Vishwakarma et al. factorial result — **explicit recent dates showed a measurable effect where formatting changes did not** — is the strongest evidence for freshness work. [verified this session; tier A] Practically: visible "Updated 2026-09-xx" dates backed by real substantive edits, changelogs, a news/press page that emits fresh URLs, and "2026" in titles where genuinely period-specific.

The stronger version is **digital PR that triggers news-index inclusion.** Perplexity and ChatGPT search lean heavily on recent news for time-sensitive queries; a story in an indexed outlet is both a fresh document and a third-party document (§1), which is why it out-performs refreshing your own blog. **PR: 4/4/2. On-site freshness: 3/2/2.** The risk on freshness is date-stamping without changing anything — that is a known spam signal.

---

## 9. Paid and partnership paths

This is where 2026 changed most, and where the honest answer for many businesses is "buy the placement."

- **ChatGPT Ads.** OpenAI began showing ads to logged-in adult users on Free and ChatGPT Go tiers in the US on **9 February 2026**, and opened a **self-serve ChatGPT Ads Manager to US businesses on 5 May 2026**, since expanded to the UK, Mexico, Brazil, Japan and South Korea. Launch partners included Target, Adobe, Williams-Sonoma and Albertsons. **CPC and CPM bidding, no minimum spend.** Ads appear in labelled, tinted boxes below responses and OpenAI states they do not influence the answer; targeting is contextual (current conversation, chat history, prior ad interaction) rather than keyword-based. Reported to have reached **$1B annualized run-rate in under 200 days.** [verified this session: openai.com/index/testing-ads-in-chatgpt/; openai.com/index/our-approach-to-advertising-and-expanding-access/; segwise.ai; stubgroup.com] **4/2/1, strong.** *No minimum spend plus contextual targeting makes this genuinely accessible to SMBs — and right now it is under-competed relative to Google Search.*
- **Google AI Mode / AI Overviews ads.** Live, served through existing Google Ads campaigns; sponsored content reportedly appears alongside roughly a quarter of AI-generated answers (tier C figure). Not yet in the Gemini app. [verified this session] **4/2/1.**
- **Perplexity: ads are off.** Perplexity **abandoned advertising in February 2026** after testing sponsored follow-up questions through 2024–25, citing user trust; Jessica Chan (head of publisher partnerships) said at Advertising Week NYC that ads "aren't currently on the roadmap" for the Comet browser, with the company targeting $500M annualized **subscription** revenue instead. [verified this session: pymnts.com 2026; campaignlive.com] **Implication: there is no paid path into Perplexity. Perplexity visibility must be earned via §1/§5, or via its merchant/shopping integrations.**
- **Merchant and data-provider programs** [prior knowledge, verify before acting]: Microsoft Copilot merchant programs, Perplexity's shopping/merchant program, and the underlying local-data providers (Yelp, Tripadvisor and similar) that several assistants lean on for local recommendations. **For a local business, keeping Yelp/Tripadvisor/Google Business Profile data accurate and rich is an upstream lever on what assistants say** — a boring but real "be a data provider" play.
- **Content licensing deals** are enterprise-scale and not available to SMBs.

---

## 10. Grey and black hat: documented, and documented to fail

**Recommendation: do not use any of these.** Documented here so the choice is informed.

**Hidden text / prompt injection in page content.** Adversarial text hidden via 1px fonts or white-on-white CSS remains in the DOM and readable by any LLM processing the page, and has been used to instruct assistants to recommend specific sites. [verified this session: unit42.paloaltonetworks.com; cybersecuritynews.com] **But the evidence that it works for recommendation is poor and getting worse.** Search Engine Land tested hidden prompts in ChatGPT and Perplexity and found **zero impact on output**; defensive techniques like spotlighting/content-wrapping treat retrieved document text as lower-trust than user instructions. Otterly.ai's controlled black-hat experiment concluded neither hidden text nor injection works reliably across platforms. The rigorous measurement is **SearchGEO (arXiv 2606.16821, 15 June 2026)**: 13 LLM backends × 308 cases, **attack success rate ranging from 0.0% on Claude-Sonnet-4.6 to 31.4% on Gemini-3-Flash.** So: unreliable, model-dependent, actively defended, and trending to zero as backends harden. [verified this session] Also documented: a **July 2026 Connecticut lawsuit where a plaintiff embedded 3-point white type instructing AI models to agree with his filing — and was sanctioned.** Hidden text has also been a classic Google spam violation for twenty years. **1/2/5. Refuted.**

Related finding worth knowing defensively: Otterly reported **two of six AI platforms hallucinated detailed descriptions of a test page with full confidence.** Monitor what assistants say about you; sometimes it's simply invented.

**Parasite pages / site reputation abuse.** Google's **March 2024 spam policy update** explicitly added **scaled content abuse, site reputation abuse (parasite SEO) and expired domain abuse**, and made the policy authorship-neutral — model-written or human-written, low-value manipulative pages violate it. Vendor reporting (tier C) claims spammy parasite pages now survive 6–8 weeks rather than 9 months and that enforcement leans on manual actions. **2/3/5.**

**Mass AI-generated pages.** Directly covered by scaled content abuse. Penalties range from quiet ranking loss to full deindexation. **1/2/5.**

**Fake reviews / review-bombing competitors.** Beyond platform bans, this is now a legal matter — the FTC's fake-reviews rule carries civil penalties, and review-bombing a competitor adds tortious-interference exposure on top. **1/3/5.**

**Wikipedia editing for your own brand.** Conflict-of-interest editing is explicitly against policy, aggressively detected, and the outcome is usually an article that is *worse* for you plus a permanent public record on the talk page. **1/3/4.**

**Reddit astroturfing.** See §6: worked, is being actively reported on, and the underlying citation share collapsed anyway.

**The common thread:** every one of these optimises for a retrieval regime that is actively adversarially hardened, and each carries a tail risk (deindexation, manual action, press exposure, sanctions) that is catastrophic relative to a modest upside. The asymmetry is terrible.

---

## 11. Case studies and postmortems

Honest caveat: **most published "AI visibility case studies" are tier C** — agency posts with no baseline, no control and no methodology. The defensible examples are the platform-level ones:

- **What didn't work (platform scale): ChatGPT Instant Checkout / ACP.** A protocol backed by OpenAI and Stripe, open-sourced with major retail launch partners, reportedly retired in March 2026 after roughly a dozen Shopify merchants shipped against it. Lesson: agent-commerce plumbing that requires merchant engineering work loses to plumbing that ships as a platform toggle. [tier B — verify]
- **What didn't work (tactic scale): Reddit-first GEO.** Brands that built their AI visibility strategy on Reddit presence saw the channel's ChatGPT citation share fall **86.4% in under a week in August 2026**. Lesson: never let one third-party domain be your citation strategy.
- **What didn't work (tooling scale): llms.txt.** Adopted by tens of thousands of sites on the strength of a plausible-sounding proposal; two large-n studies (300k domains; 137k sites) found no effect and no traffic; Google publicly declined to support it. Lesson: in GEO, plausible mechanism ≠ evidence.
- **What is working, quietly:** catalog/feed syndication for merchants, third-party roundup placement, and — newly — paid placement, with ChatGPT Ads reaching $1B annualized run-rate in under 200 days, which is the market's revealed preference about where AI attention is worth money.

---

## 12. Next 12 months: what to prepare for, and a 90-day plan

**Watch list (announced or shipping):** continued **UCP rollout** across AI Mode, Gemini, Copilot and ChatGPT with Merchant Center as the onboarding path; **ChatGPT Ads international expansion** and likely format/targeting maturation; **Google AI Mode agentic shopping** features building on the May 2026 Universal Cart; **Perplexity Comet** as an agentic browser with no ad inventory — earned-only; **Amazon Rufus**, **Meta AI**, and assistant-adjacent surfaces in messaging apps, each with its own catalog/feed onramp rather than a web-crawl onramp [prior knowledge for the last three — unverified this session].

**The structural bet:** discovery is bifurcating into (1) a **feed/catalog channel** where you are transactable because your structured data is in the platform's index, and (2) an **earned-citation channel** where you are recommendable because the third-party corpus about you is rich, specific and fresh. The middle — your own marketing site — matters mainly as the place that makes extractable facts available to both.

**90 days, in priority order:**
1. Turn on every catalog/feed channel your platform already gives you (Shopify Agentic Storefronts, Merchant Center). Days 1–3.
2. Run your 30 highest-intent conversational prompts across all five assistants; record which URLs are cited. This is your real target list. Week 1.
3. Get into the top 10 cited third-party roundups. Weeks 2–12, ongoing. Highest ROI item.
4. Publish explicit pricing, a comparison matrix, and a facts page with dated figures. Week 2.
5. Test ChatGPT Ads at small spend; it has no minimum and is currently under-competed. Week 3.
6. Fix entity basics: Wikidata, Crunchbase, LinkedIn, GBP, Yelp/Tripadvisor accuracy, one consistent descriptor. Week 4.
7. Commission or extract one piece of original research that yields a quotable statistic; pitch it. Months 2–3.
8. Re-measure at day 90 against the week-1 baseline. Without a baseline you will be unable to distinguish your work from platform drift — and as August 2026 showed, platform drift can be 86% in four days.

---

## Sources

**Retrieved and used this session (2026-09-21):**

- arXiv 2607.14035 — *Optimizing Visibility in Generative Engines: A Critical Survey of GEO (2023–2026)*, dated 15 Jul 2026 — https://arxiv.org/html/2607.14035v1
- arXiv 2606.16821 — *How Much Can We Trust LLM Search Agents? Measuring Endorsement Vulnerability to Web Content Manipulation* (SearchGEO), 15 Jun 2026 — https://arxiv.org/abs/2606.16821
- arXiv 2603.09296 — *Diagnosing and Repairing Citation Failures in GEO* — https://arxiv.org/pdf/2603.09296
- arXiv 2604.25707 — *From Citation Selection to Citation Absorption* — https://arxiv.org/html/2604.25707v2
- arXiv 2606.12439 — *Position: GEO Creates Underexamined Risks* — https://arxiv.org/pdf/2606.12439
- arXiv 2606.13610 — *One Polluted Page Is Enough: Web Content Pollution in LLM Recommenders* — https://arxiv.org/html/2606.13610
- SE Ranking — *LLMs.txt: Why Brands Rely On It and Why It Doesn't Work* (~300k domains) — https://seranking.com/blog/llms-txt/
- 1ClickReport — *llms.txt in 2026: The Evidence Says It Does Nothing* (Ahrefs 137k-site figure) — https://www.1clickreport.com/blog/llms-txt-evidence-2026
- Contentful — *Do llms.txt files actually improve AI search visibility?* — https://www.contentful.com/blog/llms-txt-search-visibility/
- Webyes — *Does llms.txt Help? What John Mueller Says* — https://www.webyes.com/blogs/does-llms-txt-improve-rankings/
- Stripe Newsroom — *Stripe powers Instant Checkout in ChatGPT; ACP released* (29 Sep 2025) — https://stripe.com/newsroom/news/stripe-openai-instant-checkout
- OpenAI — *Buy it in ChatGPT: Instant Checkout and the Agentic Commerce Protocol* — https://openai.com/index/buy-it-in-chatgpt/
- Digital Commerce 360 — *OpenAI shifts checkout plans in its agentic commerce strategy* (6 Mar 2026) — https://www.digitalcommerce360.com/2026/03/06/openai-shifts-checkout-plans-agentic-commerce-strategy/ *(egress-blocked; claim via search summary)*
- Shopify Engineering — *Building the Universal Commerce Protocol (2026)* — https://shopify.engineering/UCP
- Semrush — *Universal Commerce Protocol (UCP): What You Need to Know* — https://www.semrush.com/blog/universal-commerce-protocol/
- Passionfruit — *Google UCP Update 2026: Cart, Catalog, Loyalty* — https://www.getpassionfruit.com/blog/what-is-google-s-ucp-update-carts-catalogs-and-loyalty-in-ai-shopping
- Shopify Dev — *Agentic commerce* — https://shopify.dev/docs/agents
- Shopify Help Center — *Shopify agentic storefronts* — https://help.shopify.com/en/manual/online-sales-channels/agentic-storefronts
- Search Engine Land — *Reddit's ChatGPT Search citations fell 86% in four days* (Aug 2026) — https://searchengineland.com/reddit-chatgpt-search-citations-fall-report-485473
- Axios — *Reddit fades from ChatGPT citations* (20 Aug 2026) — https://www.axios.com/2026/08/20/chatgpt-reddit-citations-geo-strategy
- Forbes — *Reddit Nearly Vanishes From ChatGPT Citations After OpenAI Search Change* (20 Aug 2026) — https://www.forbes.com/sites/gabrielalinzainescu/2026/08/20/reddit-nearly-vanishes-from-chatgpt-citations-after-openai-search-change/
- MediaPost — *Reddit Infiltrated By Stealth AI In Brand Citation Race* — https://www.mediapost.com/publications/article/416312/reddit-infiltrated-by-stealth-ai-in-brand-citation.html
- PYMNTS — *Fake Reddit Posts Are Hijacking What AI Tells You* (2026) — https://www.pymnts.com/news/artificial-intelligence/2026/fake-reddit-posts-are-hijacking-what-ai-tells-you/
- The Drum — *Did the rush of brands to Reddit kill the platform's ChatGPT citations?* — https://www.thedrum.com/news/did-the-brand-rush-to-reddit-kill-the-platform-s-chatgpt-citations
- CMSWire — *Reddit's Rise in AI Citations* (21% of AI Overviews figure) — https://www.cmswire.com/digital-marketing/reddits-rise-in-ai-citations-what-marketers-must-know-about-aeo-strategy/
- Goodfellas Tech — *How 13 Words on Reddit Can Hijack Your AI Citations* — https://www.goodfellastech.com/blog/the-seo-vulnerability-your-competitors-are-already-exploiting-how-13-words-on-reddit-can-hijack-your-ai-citations
- Search Engine Land — *Hidden prompt injection: The black hat trick AI outgrew* — https://searchengineland.com/hidden-prompt-injection-black-hat-trick-ai-outgrew-462331
- Search Engine Land — *Black hat GEO is real – Here's why you should pay attention* — https://searchengineland.com/black-hat-geo-pay-attention-463684
- Otterly.ai — *Black Hat GEO Experiment: Hidden Text* — https://otterly.ai/blog/blackhat-geo-experiment-hidden-text/
- Otterly.ai — *GEO Experiments 2026: What We Tested, What Failed, What Works* — https://otterly.ai/blog/geo-experiments/
- Palo Alto Unit 42 — *Fooling AI Agents: Web-Based Indirect Prompt Injection Observed in the Wild* — https://unit42.paloaltonetworks.com/ai-agent-prompt-injection/
- MADX — *Black Hat SEO: Tactics, Risks and Penalties in 2026* (Google Mar 2024 spam policy additions) — https://www.madx.digital/glossary/black-hat-seo
- OpenAI — *Testing ads in ChatGPT* — https://openai.com/index/testing-ads-in-chatgpt/
- OpenAI — *Our approach to advertising and expanding access* — https://openai.com/index/our-approach-to-advertising-and-expanding-access/
- Segwise — *ChatGPT Ads in 2026: The Complete Marketer's Guide* — https://segwise.ai/blog/chatgpt-ads-2026-guide
- StubGroup — *How to Advertise on ChatGPT in 2026* — https://stubgroup.com/blog/how-to-advertise-on-chatgpt-the-complete-guide-for-2026/
- PYMNTS — *Perplexity Pulling Sponsored Answers From AI Platform* (2026) — https://www.pymnts.com/artificial-intelligence-2/2026/perplexity-pulling-sponsored-answers-from-ai-platform/
- Campaign US — *Perplexity pulls plug on ads, citing trust concerns* — https://www.campaignlive.com/article/perplexity-pulls-plug-ads-citing-trust-concerns-ai/1949142
- LLMrefs — *GEO: The 2026 Guide* (tier C; 68/32 citation split, 28–34% lift claims) — https://llmrefs.com/generative-engine-optimization
- TrueFoundry — *Best MCP Registries in 2026* — https://www.truefoundry.com/blog/best-mcp-registries

**Unverified this session (model prior knowledge, cutoff May 2026 — confirm before acting):** ChatGPT Apps SDK and apps directory; Claude connectors/skills; Gemini Gems/Extensions; Google AP2; Visa Intelligent Commerce; Mastercard Agent Pay; Microsoft Copilot and Perplexity merchant programs; Amazon Rufus; Meta AI and WhatsApp business-search surfaces; llms.txt origin (Jeremy Howard, Sept 2024); FTC fake-reviews rule; Wikipedia COI policy.
