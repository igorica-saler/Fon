# Off-Site Authority for AI Answer Engines
### What actually makes ChatGPT, Gemini/AI Mode, Perplexity, Copilot and Claude recommend a business
*Research compiled 2026-09-21. Evidence window: Aug 2024 – Sep 2026.*

---

## Top findings

1. **The premise holds, but with a sharper edge than "off-site > on-site."** The strongest *measured* correlations with AI visibility are off-site, text-based brand signals, not links. Ahrefs' study of 75,000 brands found web/brand mentions correlate with AI Overview presence at **r ≈ 0.664 vs r ≈ 0.218 for backlinks** — roughly 3:1 ([Ahrefs, Aug 2025, extended Dec 2025](https://ahrefs.com/blog/ai-overview-brand-correlation/)). A 2026 extension puts **YouTube mentions at r ≈ 0.737**, the single strongest predictor tested. *Correlation, not causation* — the authors say so explicitly.
2. **Earned media, not owned content, carries most citations.** Muck Rack's *Generative Pulse* (May 2026) analysed **25M+ cited links** across ChatGPT, Claude and Gemini in 17 industries: **earned media = 84% of citations** (82–89% across three editions since July 2025), journalism alone 25–27%, **paid/advertorial 0.3%** ([Muck Rack, 7 May 2026](https://muckrack.com/blog/what-is-ai-reading-may-2026)).
3. **Distribution multiplies the same content.** Stacker + Scrunch (16 Mar 2026), 87 stories / 30 clients / 2,600+ prompts / 8 AI platforms: syndicating a story across third-party news sites produced a **median +239% lift in brand citations**, and cross-platform AI coverage rose **5.4% → 17.9%**; 97% of syndicated stories earned ≥1 AI citation vs 82% of owned content ([GlobeNewswire, 16 Mar 2026](https://www.globenewswire.com/news-release/2026/03/16/3256365/0/en/New-Stacker-Research-Earned-Media-Distribution-Triples-AI-Search-Visibility-Delivers-239-Median-Lift-in-Brand-Citations.html)).
4. **Engines disagree almost completely about sources.** Only ~1.4% of cited URLs overlap across the four major platforms for identical queries; Perplexity↔ChatGPT is the highest domain overlap at ~25% ([SE Ranking](https://seranking.com/blog/chatgpt-vs-perplexity-vs-google-vs-bing-comparison-research/)). Kevin Indig found **91% of citations appear in only one of ChatGPT, Perplexity or AI Overviews**. There is no single "AI SEO" target list.
5. **Citation ≠ recommendation.** Semrush + Kevin Indig's *Ghost Citations* study (115 prompts, 14 countries, 4 engines, 3,981 domain appearances) found **62% of citations never produce a brand mention in the answer text**. Gemini names brands 83.7% of the time but cites them as a source only 21.4%; ChatGPT is the mirror image (87% cite / 20.7% mention) ([Semrush, Jun 2026](https://www.semrush.com/blog/the-ghost-citations-study/)). **Comparative content produced 2.4x more brand mentions** than informational.
6. **Reddit was the biggest single lever — and it just got yanked.** In mid-August 2026 ChatGPT's Reddit citation share fell ~**86%** (8.0% → 2.9% of citations on one measure; 3.83% → 0.52% on another). YouTube −88%, TikTok −72%, LinkedIn −36% ([Axios, 20 Aug 2026](https://www.axios.com/2026/08/20/chatgpt-reddit-citations-geo-strategy)). Crucially, **Petra Labs found brand *recommendation* share largely intact** — the engine changed which URLs it shows, not which brands it names ([Mi3, 21 Aug 2026](https://www.mi-3.com.au/21-08-2026/very-rare-and-very-curious-chatgpt-guts-reddit-youtube-and-tiktok-citations-keeps)). This is the single most important strategic fact in this report: **optimise for being talked about, not for a specific domain's citation share.**
7. **Reviews are the highest-leverage off-site asset for local and B2B.** Google Business Profile is ~**28.5% of local AI citations**, Yelp ~8.5% (BrightLocal, 60,970 checks across 1,355 locations). Yelp now licenses 330M reviews directly into ChatGPT ([Axios, 23 Jul 2026](https://www.axios.com/2026/07/23/yelp-reviews-chatgpt-geo-partnership)). In B2B, five review platforms account for **88% of review-platform citations in AI Overviews**: Gartner Peer Insights 26.0%, G2 23.1%, Capterra 17.8%, Software Advice 12.8%, TrustRadius 8.3%.
8. **Listicles are the dominant citable *format*.** Best-of/"top X" listicles take ~**21.9% of all citations** across AI Mode, ChatGPT and Perplexity, and ~41% of citations on commercial queries. Getting added to existing, already-cited roundups is the fastest measurable off-site win.
9. **Manipulation is now actively policed on three fronts:** Reddit's own AI spam detection (~25,000 spammy posts/day caught in Q1 2026; Bloomberg, 6 Jul 2026), Google's site-reputation-abuse enforcement (algorithmic since the Aug 2025 spam update), and the **FTC Consumer Review Rule** — first warning letters issued 22 Dec 2025, civil penalties up to **$53,088 per violation** ([FTC, 22 Dec 2025](https://www.ftc.gov/news-events/news/press-releases/2025/12/ftc-warns-10-companies-about-possible-violations-agencys-new-consumer-review-rule)).

> **Evidence quality warning.** Much of the 2026 "AI citation" literature is vendor marketing recycling a handful of primary studies. I have flagged each claim as **[measured]**, **[vendor-measured]** (a tool company measuring its own data, directionally useful but self-interested) or **[opinion]**. Direct fetching of several primary sources (Ahrefs, Semrush, Search Engine Land, Profound, SE Ranking, Otterly) was blocked by the network policy in this environment, so figures are as reported in search-result synthesis and secondary coverage; **verify headline numbers against the primary URL before quoting externally.**

---

## 1. Which domains get cited, and by whom

**[measured]** Semrush analysed **230,000 prompts / 100M+ citations** across ChatGPT, Google AI Mode and Perplexity over 13 weeks (Aug–Oct 2025). Top domains overall: **Reddit, LinkedIn, Wikipedia, Medium, YouTube**, plus Google properties. Semrush later expanded this to an **AI Visibility Index covering 126M prompts** (2026). G2 is the only B2B review platform in the top 20.

**[measured]** Per-engine character, as of the most recent data:

| Engine | Retrieval | Source personality |
|---|---|---|
| **ChatGPT** | Own index + Bing + ≥8 partner feeds (Yelp, TripAdvisor, Foursquare, Web IQ) | Was Reddit/Wikipedia-heavy; since Aug 2026 skews to **help centres, product docs, editorial** (Forbes, Reuters, NYT, Business Insider). Heavy Google-rank dependence: of ChatGPT-cited pages also in Google's top 20, **43.2% held #1**, vs 12.3% cited beyond position 20. |
| **Google AI Overviews / AI Mode** | Existing organic index + query fan-out | Self-referential: ~**43% of AIO citations point to Google-owned properties**, incl. YouTube. Oldest domains (49.2% >15 years old). Reddit ~21% of AIO citations. |
| **Perplexity** | Live retrieval per query | Favours **review aggregators and structured comparison**: G2, Gartner, NerdWallet, PCMag, TripAdvisor, Yelp (Profound, 1.4M citations / 6 models). New content can appear within hours. |
| **Claude** | Brave-backed search | **379,321 citations / 16,406 domains** (Otterly, Jun 2026): **63% niche SaaS blogs, docs and practitioner articles; only 7% mainstream news.** 86.7% overlap with Brave top organic (Profound, 2025). No YouTube viewing. |
| **Gemini** | Google index + can actually watch YouTube | Websites 52.1% of citations; names brands in-text far more than it cites them. |
| **Copilot** | Bing | Closest to classic Bing organic; least studied. |

**[vendor-measured]** Yext's 17.2M-citation analysis (Oct 2025) found **Gemini favoured websites (52.1%), OpenAI leaned on listings (48.7%), Perplexity diversified** (MapQuest, TripAdvisor). Yext's 2025 study claims **86% of AI citations come from sources brands can control** — websites, listings, help content. That framing is self-serving (Yext sells listings management) but is corroborated by the Aug-2026 shift toward docs and help centres.

**[measured]** Conductor's analysis of 21.9M queries: **AI Overviews appear in 25.11% of Google searches**, up from 13.14% in March 2025. AI referral traffic ≈ **1.08% of all web traffic**, of which ChatGPT drives 87.4%.

**Interpretation [opinion]:** the top-5 domains are a *ceiling you cannot occupy*, not a target. Even the most-cited domain rarely exceeds ~5% of total citations on any engine; ~95% of citations spread across thousands of domains. The practical target is the **long tail of category-specific sources** — the three listicles, two review sites, one forum and one YouTube channel that your category's prompts actually surface.

---

## 2. Reddit, Quora and communities

**Why Reddit was over-represented [measured]:** Google signed a ~**$60M/year** content-licensing deal with Reddit in Feb 2024 (announced the day Reddit filed for IPO), giving Google real-time access and "more content-forward displays." OpenAI followed with a deal estimated at ~$70M/year — ~$140M total AI licensing revenue in 2025 ([CJR](https://www.cjr.org/analysis/reddit-winning-ai-licensing-deals-openai-google-gemini-answers-rsl.php)). Reddit was reportedly weighing non-renewal with Google as of July 2026 ([CNBC, 22 Jul 2026](https://www.cnbc.com/2026/07/22/reddit-stock-google-ai-content-deal.html)).

**Evidence Reddit content steers answers [measured]:** Cornell Tech researchers showed that **as few as 13 words in a Reddit comment** can steer an AI-generated answer toward a chosen product. Practitioner tests (reported, not peer-reviewed) show a brand moving from ~8–9% to ~3x that share of 80 AI Overview prompts within two weeks of seeding, reverting when seeding stopped — repeatable across several runs. **[vendor/practitioner-measured, treat as directional.]**

**What changed [measured]:** the August 2026 collapse. Tracking firms attribute it to ChatGPT's use of the `site:` operator in fan-out queries jumping from ~0.4% to ~17% on 8 Aug 2026 — a retrieval-strategy change, not a Reddit penalty. Reddit simultaneously deployed its own AI moderation, catching ~25,000 spammy posts/comments per day in Q1 2026 and cutting spam exposure ~20% YoY ([Bloomberg, 6 Jul 2026](https://www.bloomberg.com/news/articles/2026-07-06/reddit-is-cracking-down-on-ai-marketing-slop-with-its-own-ai); [Forbes, 7 Jul 2026](https://www.forbes.com/sites/codyluongo/2026/07/07/reddit-cracks-down-on-bots-and-spam-but-ai-search-manipulation-may-be-harder-to-stop/)). 404 Media documented coordinated vendor seeding in r/biohackers (~830k members) in June 2026, after which mods banned standalone posts on those topics.

**Legitimate participation [opinion, community-standard]:**
- Follow the **90/10 rule** (Reddit's own guideline) or the stricter **9:1** practitioner norm: nine genuinely helpful contributions per promotional one.
- **Disclose affiliation** in the comment itself ("I work at X, so discount accordingly — but..."). This is also the FTC-safe position.
- **Never sockpuppet.** Multi-account upvoting/recommendation rings get all linked accounts permanently suspended and Reddit's detection is now ML-driven.
- Select subreddits where the *question* recurs (search `site:reddit.com "best [category]"`), not where the audience is biggest.
- AMAs work only with a genuine credential or dataset to offer.
- A single well-upvoted, substantive comment on an evergreen "best X" thread outperforms a dozen thin posts.

**Other communities:**
- **Quora** — still retrievable and cited; lower volume than Reddit but far lower moderation risk. Pairs well with expert-author profiles.
- **Stack Exchange / Stack Overflow** — heavily cited for technical/developer categories; Stack Exchange appears among the most frequently cited sources in AI results.
- **Niche vertical forums** (industry-specific boards, Houzz/contractor forums, hobbyist boards) — under-exploited, persistent, and often the only deep source in a narrow category.
- **LinkedIn** — top-5 cited domain on several engines; posts and company pages are retrievable. Founder posting is a real entity-building channel.
- **Discord and most Slack communities — not retrievable.** Contributions never surface in AI citations. Valuable for customers, worthless for AI visibility. **[measured/structural]**
- **GitHub Discussions / Hugging Face** — cited for technical categories.

---

## 3. Reviews and ratings

**Local [measured]:** BrightLocal ran **60,970 checks** (5 prompts × 9 geo-points × 1,355 locations, US/UK/AU) across AI Overviews, AI Mode and ChatGPT, identifying **415 directories** used as sources. **Google Business Profile = 28.5% of all citations; Yelp = 8.53%** (higher on ChatGPT). Legacy directories — BBB, Angi, MapQuest — recur, plus vertical specialists (e.g. Healthy Paws, Scratchpay for vets) ([BrightLocal](https://www.brightlocal.com/resources/ai-directory-sources/)).

**[measured]** Yelp tracked **512,680 citations** across ChatGPT, Gemini, Perplexity and AI Mode, appearing ~3.3–3.4x more often than any rival local source; restaurants Q4 2025: Yelp 14,100 citations vs TripAdvisor 4,327, UberEats 4,123, DoorDash 2,506, OpenTable 2,357 combined ([ppc.land](https://ppc.land/yelp-gets-3-4x-more-ai-citations-than-any-rival-in-new-local-search-data/)). **July 2026: Yelp licensed 330M reviews directly to OpenAI** ([Search Engine Land, Jul 2026](https://searchengineland.com/openai-yelp-deal-483326)). Foursquare reportedly supplies ~70% of ChatGPT's local business data **[single-source claim, verify]**.

**Reality check [measured]:** SOCi's 2026 Local Visibility Index (~350,000 locations) found brands appear in Google's local 3-pack **35.9%** of the time but ChatGPT recommends only **1.2%** of those same locations — local AI recommendation is currently far scarcer and more concentrated than local search.

**B2B [measured]:** In AI Overviews, the top five review platforms take **88%** of review-platform citations — Gartner Peer Insights 26.0%, G2 23.1%, Capterra 17.8%, Software Advice 12.8%, TrustRadius 8.3%. **49% of AI Overviews for explicit "review" searches include a review platform vs only 17.1% for "best/top" searches** — meaning review sites win the validation query, listicles win the discovery query. G2's 2025 Buyer Behavior Report reports generative AI chatbots are now the **#1 influence on vendor shortlists**, ahead of review sites and vendor websites.

**Velocity, recency, response [mixed evidence]:**
- **[measured]** BrightLocal's Local Consumer Review Survey 2026: **74% of consumers look for reviews written in the past three months.**
- **[vendor claim]** Widely repeated figures — reviews under 30 days carrying "full weight," decaying to 10–20% after six months; 4–8 new reviews/month for SMBs, 10–15 for high-traffic categories — come from review-platform vendors with no published methodology. Treat as **operating heuristics, not findings.**
- **[vendor claim, high-leverage if true]** One study reports brands with an active, responded-to review profile cited in **75.3%** of AI answers vs **1%** for brands with no active profile. The magnitude is implausibly large and the methodology is unpublished; the *direction* is consistent with everything else here.
- **[opinion, well-founded]** **Review content specificity is the underrated variable.** LLMs extract attributes, not stars. A review saying "they re-piped a 1920s bungalow in Oakland in two days" is retrievable evidence for "best plumber for old homes in Oakland"; "great service, 5 stars" is not. Ask customers (never scripting the sentiment) what *specific job* you did. This is the one review tactic that directly maps to how retrieval works.
- Responding to reviews creates additional first-party text on a third-party page — free, specific, keyword-bearing content on a domain AI already trusts.

**E-commerce [measured]:** LLM Pulse's July 2026 study of **391,000 citations** on non-branded US shopping questions found **retailers as a category took just 2.9% of citations** — Reddit and YouTube out-cited every big-box store combined. **ChatGPT cited amazon.com 6 times in the entire sample; Perplexity zero.** Amazon's AI visibility comes almost entirely via Google's AI. Yet **50.9% of AI-influenced purchases still completed on Amazon**. Implication: Amazon is the *conversion* surface, not the *discovery* surface — editorial reviews, Reddit and YouTube are where the recommendation is formed.

---

## 4. Digital PR and earned media

**[measured]** Muck Rack's 84% earned-media figure (above) is the strongest single data point for PR's role. Journalism = 25–27% of citations; **paid/advertorial = 0.3%** — sponsored content is close to worthless for AI citation.

**[measured]** Stacker/Scrunch's 239% median lift shows the mechanism is **distribution breadth**, not a single hit: the same story on 20 local news sites outperforms the same story on one.

**[vendor claim]** Digital PR reportedly accounts for ~25% of LLM citations while only ~6% of practitioners use it — the widest evidence-to-adoption gap in GEO. Unverified, but consistent with the field's observable behaviour.

**What to actually do:**
- **Original data studies.** The peer-reviewed GEO paper (Princeton / Georgia Tech / IIT Delhi) found that **adding statistics improved generative-engine visibility by ~41%** — the single most effective technique tested; quoting sources and citing authorities helped further. Original research is simultaneously a citable asset *and* a journalist magnet. **[measured, peer-reviewed]**
- **Expert quotes.** HARO is gone; the 2026 stack is **Qwoted Pro (~$99/mo), Featured.com (~$49/mo), Help a B2B Writer (free)**, plus Qwoted's free tier and SourceBottle. Featured is the closest HARO successor structurally (journalists post questions, answers get published in roundups). **[measured pricing/market structure]**
- **Podcasts.** Transcripts and show-note pages are indexed and retrievable; a mention in a well-transcribed episode behaves like a mention in an article. Given YouTube mentions' r=0.737, **video podcasts are the highest-value format**. **[opinion, grounded in the Ahrefs correlation]**
- **Local news.** For local service businesses, a handful of local-news mentions plus sponsorship coverage is disproportionately effective because the geographic corpus is thin.
- **Links vs unlinked mentions.** The honest read: **backlinks are a threshold condition, not the driver.** Seer Interactive found domain rank r≈0.25 and backlinks r≈0.10 against ChatGPT brand visibility, while **Google page-1 ranking correlated ~0.65** — i.e. links matter mainly because they produce rankings, and rankings feed retrieval. For AI specifically, an **unlinked mention is nearly as valuable as a linked one, because the model reads words, not hrefs.** Stop refusing placements that won't link. **[measured + opinion]**

---

## 5. Listicles, directories and entities

**Listicles [measured]:** ~21.9% of all citations across AI Mode, ChatGPT and Perplexity; ~41% on commercial queries. **Process:**
1. Run your 30–50 real buyer prompts through each engine and log every cited URL.
2. Cluster by domain; the 5–15 listicles that recur are your target list.
3. Check which already mention you **without linking** — a warm "you already mention us, could you link/update?" email converts at the highest rate of any outreach.
4. For new inclusion, pitch short: **one sentence on what the product is, one differentiator, one ask.** Editors almost never act on long pitches asking them to write new content.
5. Offer something real: fresh 2026 pricing/data for their table, a screenshot set, a free account for testing, an expert quote, or a correction to an out-of-date entry. "We just refreshed our data for 2026" is a legitimate re-pitch hook.
6. Re-check quarterly — listicle refreshes are when entries get added and dropped.

**Directories [measured]:** Local-service directories (Angi, Thumbtack) account for ~**10.3%** of local AI citations, while business websites are ~**59.9%** of Gemini's local citations. Critically, **Thumbtack now has native integrations with ChatGPT (Oct 2025), Alexa+ (Feb 2025), OpenAI Operator (Jan 2025) and Claude (23 Apr 2026)**; Angi, TaskRabbit and Yelp have struck similar deals. For home services, **a complete Thumbtack/Angi profile is now a direct AI distribution channel, not just a lead source.** Houzz has de-emphasised its marketplace in favour of SaaS, lowering its priority.

**Wikipedia / Wikidata [measured policy]:** Wikipedia requires **significant coverage in independent secondary sources** — a bar most SMBs cannot clear, and attempting to force it is counterproductive. **Wikidata has a materially lower notability bar** (verifiable + clearly identified, often satisfied by registry listings, Crunchbase/D&B/Bloomberg presence, or being referenced by another notable item) and feeds Google's Knowledge Graph directly. **Wikidata is the realistic entity play; Wikipedia is a by-product of genuine notability.**
**Cautionary [measured]:** the Wikimedia Terms of Use **require disclosure of employer, client and affiliation for any paid contribution**; undisclosed paid editing is prohibited and has produced repeated public scandals (Wiki-PR 2012, Operation Orangemoody 2015). Use Talk-page requests and disclosed editors only.

**Entity consistency [opinion, strongly supported by mechanism]:** maintain one canonical entity sentence — *"[Name] is a [category] that [does what] for [whom] in [where]"* — and replicate it verbatim across website, LinkedIn, Crunchbase, Google Business Profile, G2/Capterra, Wikidata, press releases and author bylines. Contradictions between Crunchbase and LinkedIn give models conflicting attributes to weigh. Claude's base model in particular "knows" brands that appear consistently across Wikipedia, Crunchbase, LinkedIn and major editorial domains.

**Co-citation [measured mechanism]:** the Ghost Citations finding that **comparative content yields 2.4x more brand mentions** than informational content means "X vs Y" and "alternatives to X" pages *on third-party sites* are the highest-yield placement type. Getting into a competitor's category comparison — on a neutral third-party domain — puts your name in the same retrieval chunk as the market leader. Track "best X", "X vs Y", "X alternatives", "X for [industry]" as your prompt set.

---

## 6. YouTube and video

**[measured]** YouTube mentions are the **strongest single correlate of AI visibility (r ≈ 0.737)** in Ahrefs' 2026 data. **Perplexity (38.7%) and Google AI Overviews (36.6%) drive the majority of YouTube citations.** ~43% of AIO citations go to Google-owned properties.

**[vendor-measured]** Otterly's 2026 YouTube citation study found:
- **94% of YouTube AI citations go to long-form video**, not Shorts.
- **Views and subscriber counts have near-zero correlation with citation frequency.** Description length and chapter structure are the strongest predictors.
- **78% of timestamped videos were cited across 2–5 different chapters** — chapters function as independently citable units, turning one video into several sources.
- Timestamped citations are concentrated in Google's ecosystem; ChatGPT, Copilot and Perplexity did not surface timestamped citations in the sample.

**[measured, structural]** Only **Gemini can actually process the video**; every other engine reads the transcript and metadata. **Claude has no YouTube access at all** — which is why a text page with the full transcript on your own domain remains necessary.

**Practical:** title the video as the *prompt* ("Best CRM for a 5-person agency in 2026", not "Our Q3 Product Update"); upload a **corrected transcript** (auto-captions routinely mangle brand names and technical terms, and those errors propagate); write a long, substantive description; add chapters named as sub-questions; republish the transcript on your site.

---

## 7. Case studies and cautionary tales

**Documented positives**
- **Stacker/Scrunch (Mar 2026)** — +239% median citation lift from earned syndication; 5.4%→17.9% cross-platform coverage. *The best-controlled off-site case study available.*
- **Listicle placement** — Position Digital documented being added to Exposure Ninja's "Best AI Search Optimisation Agencies in 2026"; because ChatGPT cited that listicle, the agency began appearing in AI answers. *n=1, but a clean demonstration of the mechanism.*
- **Reddit seeding tests** — repeatable ~3x lift in AI Overview prompt share during seeding, reverting on stop. *Practitioner-reported; and see the caveat below.*

**Cautionary**
- **The August 2026 rug-pull.** Anyone whose strategy was "own Reddit" lost ~86% of that citation channel in two weeks. Brand recommendation share, however, held. **Lesson: build mentions across many surfaces; never index a strategy on one domain's citation share.**
- **Reddit enforcement.** Agencies specialising in AI-citation seeding are having posts removed; coordinated vendor campaigns (r/biohackers, June 2026) triggered blanket topic bans that harmed legitimate participants too.
- **FTC Consumer Review Rule.** Final Aug 2024; first enforcement wave **22 Dec 2025** (warning letters to 10 companies); **up to $53,088 per violation**, and each fake review can count separately. The FTC explicitly cited generative AI as making fake reviews easier — undisclosed paid seeding is a Section 5 violation.
- **Google site reputation abuse.** Manual actions from March 2024, **fully algorithmic by the August 2025 spam update**, with Google able to evaluate domain sections independently of parent authority — the core parasite-SEO mechanism no longer works. Note the EEA carve-out on manual demotions from 30 Aug 2026 (a DMA accommodation, not a change of heart).
- **Wikipedia COI.** Undisclosed paid editing risks deletion, flagging and negative press.

---

## 8. Prioritised playbooks

Effort: **L** ≤ 1 person-week setup, **M** = ongoing weeks, **H** = quarters. Impact is *expected* effect on AI recommendation rate, my judgement given the evidence above.

### (a) Local service business
| # | Action | Effort | Impact | Why |
|---|---|---|---|---|
| 1 | Complete/expand Google Business Profile: every category, service, attribute, Q&A | L | **High** | 28.5% of local AI citations |
| 2 | Claim + enrich **Yelp** (and Foursquare) | L | **High** | Direct OpenAI licensing since Jul 2026 |
| 3 | Review engine: steady velocity + **ask for specifics** (job type, neighbourhood, problem solved) + reply to all | M | **High** | Reviews are the corpus AI summarises; specificity is what's retrievable |
| 4 | Thumbtack / Angi / TaskRabbit profiles | L | **High** | Native ChatGPT + Claude integrations |
| 5 | Vertical directories + BBB + MapQuest + Apple Maps; NAP consistency | L | Medium | 415 directories feed local answers |
| 6 | Get into "best [service] in [city]" local listicles and local-news roundups | M | **High** | Listicles ≈ 22% of all citations |
| 7 | Local news mentions, sponsorships, awards | M | Medium | Thin local corpus = outsized effect |
| 8 | YouTube: 6–10 long-form job walkthroughs, prompt-shaped titles, chapters, clean transcripts | M | Medium-High | Strongest mention correlate; low local competition |
| 9 | Wikidata item; consistent entity sentence everywhere | L | Medium | Feeds Knowledge Graph |

### (b) E-commerce shop
| # | Action | Effort | Impact |
|---|---|---|---|
| 1 | Get into category "best X" roundups on editorial/enthusiast sites (Wirecutter-tier down to niche blogs) | H | **Very high** — retailers take only 2.9% of shopping citations; editorial owns discovery |
| 2 | YouTube reviews/unboxings by creators; your own long-form comparison videos | M | **High** — YouTube out-cites every big-box retailer |
| 3 | Reddit/forum presence in the buying subreddits — disclosed, expertise-led | M | High but volatile |
| 4 | Trustpilot profile + steady, specific reviews | L | High |
| 5 | Amazon listing/review quality | M | Medium for AI *discovery*, **high for conversion** (50.9% of AI-influenced purchases close there) |
| 6 | Comparison content placed on third parties ("X vs Y") | M | High — 2.4x brand-mention rate |
| 7 | Original data/trend study for press pickup + syndication | H | High — 239% distribution lift; +41% from statistics |
| 8 | Entity hygiene: Wikidata, Crunchbase, consistent description | L | Medium |

### (c) B2B / SaaS
| # | Action | Effort | Impact |
|---|---|---|---|
| 1 | **G2 + Capterra + Gartner Peer Insights + TrustRadius**: complete profiles, sustained review velocity, category placement | M | **Very high** — 88% of review-platform citations; AI chatbots now the #1 shortlist influence |
| 2 | Get into "best [category] software 2026" listicles; audit which ones your prompts actually cite | M | **Very high** — ~41% of commercial-query citations |
| 3 | Publish original benchmark/industry data annually; syndicate hard | H | **High** — +41% statistics effect, 239% distribution lift |
| 4 | Expert-quote programme via Qwoted/Featured/Help a B2B Writer; founder as named source | M | High — journalism = 25–27% of citations |
| 5 | Third-party "X vs Y" and "alternatives to X" placements | M | High — 2.4x mention rate |
| 6 | **Public docs + help centre, crawlable and thorough** | M | **High and rising** — the biggest winner of the Aug 2026 reallocation; 63% of Claude citations are docs/practitioner content |
| 7 | Reddit/Stack Exchange/niche forum answers from real engineers, disclosed | M | Medium-High, volatile |
| 8 | LinkedIn: company page + founder posting cadence | M | Medium-High — top-5 cited domain |
| 9 | Crunchbase, Wikidata, consistent entity sentence; Wikipedia only if genuinely notable | L | Medium |
| 10 | Video podcast circuit + your own long-form YouTube | M | High — r=0.737 |

**Measurement [opinion].** Track **brand recommendation share** (does the engine name you?) as the primary KPI and citation share as secondary — the August 2026 event proved they decouple. Run a fixed prompt set (30–50 real buyer questions) across ChatGPT, AI Mode, Perplexity, Gemini and Claude monthly; log both *whether you're named* and *which URLs are cited*, because the cited URLs are your outreach target list for the next quarter. Given ~91% single-engine citation overlap, measure each engine separately.

---

## Sources

**Correlation & ranking-factor studies**
- Ahrefs, *An Analysis of AI Overview Brand Visibility Factors (75K Brands)* — https://ahrefs.com/blog/ai-overview-brand-correlation/ (Aug 2025; extended Dec 2025); https://ahrefs.com/blog/ai-brand-visibility-correlations
- Seer Interactive, *What Drives Brand Mentions in AI Answers?* — https://www.seerinteractive.com/insights/what-drives-brand-mentions-in-ai-answers
- Princeton / Georgia Tech / IIT Delhi GEO paper (statistics +41%) — summarised at https://ziptie.dev/blog/how-original-research-wins-ai-citations/
- Machine Relations, *AI Search Citation Factors 2026* — https://machinerelations.ai/research/ai-search-citation-factors-2026

**Citation-source datasets**
- Semrush, *The Most-Cited Domains in AI: A 3-Month Study* (230k prompts, Aug–Oct 2025) — https://www.semrush.com/blog/most-cited-domains-ai/
- Semrush + Kevin Indig, *Ghost Citations* (62%) — https://www.semrush.com/blog/the-ghost-citations-study/ (Jun 2026)
- Semrush, *2026 AI Visibility Index, 126M prompts* — https://www.semrush.com/news/463141-semrush-releases-expanded-2026-ai-visibility-index-analyzing-126-million-ai-search-prompts/
- Profound, *AI Platform Citation Patterns* — https://www.tryprofound.com/blog/ai-platform-citation-patterns
- Search Engine Land, *AI search engines cite Reddit, YouTube and LinkedIn most* — https://searchengineland.com/ai-search-engines-cite-reddit-youtube-and-linkedin-most-study-473138
- SE Ranking, *ChatGPT vs Perplexity vs Google vs Bing* — https://seranking.com/blog/chatgpt-vs-perplexity-vs-google-vs-bing-comparison-research/
- Otterly, *Claude Citation Study* (379,321 citations, Jun 2026) — https://otterly.ai/blog/claude-ai-citation-study/
- Otterly, *YouTube AI Citation Study 2026* — https://otterly.ai/blog/youtube-ai-citation-study-2026/
- Yext Research — https://www.yext.com/research/ ; https://www.yext.com/blog/what-brands-need-to-know-about-ai-search-2026
- Peec AI, *ChatGPT built its own search index* — https://peec.ai/blog/chatgpt-built-its-own-search-index
- Kevin Indig, *2026 Growth Memo research summary* — https://www.growth-memo.com/p/2026-growth-memo-research-summary

**Earned media / PR**
- Muck Rack *Generative Pulse* (25M links, May 2026) — https://muckrack.com/blog/what-is-ai-reading-may-2026 ; https://www.globenewswire.com/news-release/2026/05/07/3290268/0/en/generative-pulse-earned-media-consistently-drives-ai-citations-holding-at-84.html
- Stacker + Scrunch (16 Mar 2026) — https://www.globenewswire.com/news-release/2026/03/16/3256365/0/en/New-Stacker-Research-Earned-Media-Distribution-Triples-AI-Search-Visibility-Delivers-239-Median-Lift-in-Brand-Citations.html ; https://stacker.com/resources/how-earned-distribution-drives-ai-search-citations
- Position Digital, *Listicle Outreach Guide* — https://www.position.digital/blog/listicle-outreach-guide/ ; *Digital PR Tactics* — https://www.position.digital/blog/digital-pr-tactics/
- Featured *Find Awards* launch (15 Sep 2026) — https://www.globenewswire.com/news-release/2026/09/15/3362268/0/en/featured-launches-find-awards-to-search-4-596-business-awards-rankings-and-best-of-lists-across-43-industries.html
- HARO successor landscape — https://www.prezly.com/academy/the-best-haro-alternatives ; https://www.buzzstream.com/blog/haro-alternatives/

**Reddit & communities**
- CJR, *Reddit Is Winning the AI Game* — https://www.cjr.org/analysis/reddit-winning-ai-licensing-deals-openai-google-gemini-answers-rsl.php
- CNBC (22 Jul 2026), Reddit–Google deal renewal — https://www.cnbc.com/2026/07/22/reddit-stock-google-ai-content-deal.html
- Axios (20 Aug 2026), *Reddit fades from ChatGPT citations* — https://www.axios.com/2026/08/20/chatgpt-reddit-citations-geo-strategy
- Mi3 (21 Aug 2026), Petra Labs analysis — https://www.mi-3.com.au/21-08-2026/very-rare-and-very-curious-chatgpt-guts-reddit-youtube-and-tiktok-citations-keeps
- Bloomberg (6 Jul 2026), Reddit AI anti-spam — https://www.bloomberg.com/news/articles/2026-07-06/reddit-is-cracking-down-on-ai-marketing-slop-with-its-own-ai
- Forbes (7 Jul 2026) — https://www.forbes.com/sites/codyluongo/2026/07/07/reddit-cracks-down-on-bots-and-spam-but-ai-search-manipulation-may-be-harder-to-stop/
- Reddit seeding / Cornell 13-word finding — https://aiweekly.co/alerts/reddit-fights-marketers-seeding-chatgpt-and-gemini-answers
- Sitebulb, *Reddit for SEO / AIO visibility* — https://sitebulb.com/resources/guides/reddit-is-no-longer-just-a-nerd-forum-its-an-ai-visibility-lever/

**Reviews, local, directories**
- BrightLocal, *Where to get local citations for AI search* (60,970 checks) — https://www.brightlocal.com/resources/ai-directory-sources/
- Axios (23 Jul 2026), Yelp–OpenAI — https://www.axios.com/2026/07/23/yelp-reviews-chatgpt-geo-partnership ; Search Engine Land — https://searchengineland.com/openai-yelp-deal-483326
- ppc.land, Yelp 3.4x AI citations — https://ppc.land/yelp-gets-3-4x-more-ai-citations-than-any-rival-in-new-local-search-data/
- Foundation Inc, *Yelp is the Most Trusted Source for AI Local Discovery* — https://foundationinc.co/lab/yelp-ai-local-discovery/
- AirOps, *How Review Sites Drive AI Citations* — https://www.airops.com/blog/review-sites-ai-citations
- G2 Learn, *Does G2 Get Ranked in AI LLM Search?* — https://learn.g2.com/tech-signals-does-g2-get-ranked-in-ai-llm-search
- 561 Media, Thumbtack–Claude integration (23 Apr 2026) — https://www.561media.com/blog/ai-visibility-strategy-home-services
- Trustmary, *The Impact of Reviews on AI Search in 2026* — https://trustmary.com/ai-visibility/the-impact-of-reviews-on-ai-search/

**E-commerce**
- LLM Pulse, *Where AI Actually Sends Shoppers* (391k citations, Jul 2026) — https://llmpulse.ai/blog/where-ai-sends-shoppers/
- eMarketer, *AI is quietly deciding which products consumers never see* — https://www.emarketer.com/content/ai-quietly-deciding-which-products-consumers-never-see
- Azoma, *What Sources Does ChatGPT Cite for Products?* (26 Aug 2026) — https://www.globenewswire.com/news-release/2026/08/26/3351631/0/en/what-sources-does-chatgpt-cite-for-products-azoma-publishes-the-data-and-explains-how-brands-get-recommended.html

**Entities & risk**
- Wikipedia:Paid-contribution disclosure — https://en.wikipedia.org/wiki/Wikipedia:Paid-contribution_disclosure ; Wikipedia:Conflict of interest — https://en.wikipedia.org/wiki/Wikipedia:Conflict_of_interest
- MLforSEO, *Wikidata for Brands: Notability Criteria* — https://www.mlforseo.com/knowledge-graph-strategy/wikidata-for-brands-notability-criteria-and-a-realistic-path/
- FTC, *Final Rule Banning Fake Reviews* (Aug 2024) — https://www.ftc.gov/news-events/news/press-releases/2024/08/federal-trade-commission-announces-final-rule-banning-fake-reviews-testimonials
- FTC, *Warning letters to 10 companies* (22 Dec 2025) — https://www.ftc.gov/news-events/news/press-releases/2025/12/ftc-warns-10-companies-about-possible-violations-agencys-new-consumer-review-rule
- Google site reputation abuse enforcement — https://www.digitalhitmen.com.au/blog/googles-site-reputation-abuse-policy-explained/

*Note: several primary sources (Ahrefs, Semrush, Search Engine Land, SE Ranking, Profound, Otterly, PRNewswire, Wikipedia) could not be fetched directly from this environment due to network egress policy; their figures here are as reported via search-result synthesis and secondary coverage and should be re-verified at the primary URL before external publication.*
