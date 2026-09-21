# Content Strategy for Getting Cited and Recommended by AI Answer Engines
**Research brief — compiled 21 September 2026**

> Scope note on method and confidence. This report separates **measured findings** (published studies with stated sample sizes and methodology) from **vendor claims** (numbers published by tools/agencies with no disclosed method — useful as directional signal, not proof) and **practitioner consensus** (repeated advice that is plausible but untested). Where studies contradict each other, the contradiction is shown rather than smoothed over. A critical survey of 45 GEO studies (2023–July 2026) concluded that the evidence supports *intermediate* effects (a page edit changing retrieval/citation probability) but that **no study yet establishes a stable, longitudinal, cross-platform causal chain from page edit → citation → traffic → revenue** ([Verity Score, 2026](https://verityscore.io/en/blog/does-geo-work-45-studies-evidence-2026/)). Treat everything below as probability shifting, not ranking control.

---

## Top 10 content rules

1. **Earn the third-party mention before polishing your own page.** Across studies, the large majority of AI citations for commercial queries go to third-party sources — review platforms, listicles, Reddit, YouTube, media — not brand domains. Reported splits range from 79/21 to 84–93% third-party ([Cognizo, 2026](https://www.cognizo.ai/blog/third-party-ai-citations); [christopherjanb.com, 2026](https://christopherjanb.com/blog/where-ai-gets-recommendations/) — *vendor claims*). SE Ranking found ~34.5% of AI Overviews cite at least one review platform. Your site is the *corroboration layer*; someone else's list is the *recommendation layer*.
2. **Rank in classic search anyway.** Among pages ranking #1 in Google, 43.2% were cited by ChatGPT — 3.5× the rate of pages ranking beyond the top 20 ([Growth Memo, 2026](https://www.growth-memo.com/p/the-science-of-how-ai-picks-its-sources)). 76% of AI Overview citations come from the organic top 10 ([Ahrefs](https://ahrefs.com/blog/search-rankings-ai-citations/)). GEO is not a replacement for SEO; C-SEO Bench found traditional SEO "significantly more effective" than conversational-SEO tricks ([NeurIPS 2025](https://openreview.net/forum?id=oTeixD3oZO)).
3. **Front-load the answer.** 44.2% of all ChatGPT citations come from the first 30% of a page's content (21K citations analysed, [Growth Memo](https://www.growth-memo.com/p/shorter-focused-content-wins-in-chatgpt); covered by [Search Engine Land](https://searchengineland.com/chatgpt-citations-content-study-469483)). The most important number, price, or verdict belongs in the first two sentences — of the page and of every H2 section.
4. **Put numbers and dates in the prose.** The entity types that best predict ChatGPT citation are **DATE** and **NUMBER** ([Growth Memo / Search Engine Land, 2026](https://searchengineland.com/proprietary-data-ai-citation-asset-481380)). This is the most durable, most replicated finding from the original Princeton GEO paper onward.
5. **Write self-contained passages.** Retrieval happens at chunk level, not page level. Semantically segmented passages and structured Q&A blocks outperform dense prose in citation tests; dense prose performs worst ([Chris Green research, via Lumar, 2026](https://www.lumar.io/blog/best-practice/content-chunking-ai-extractability-geo-aeo-explainer/)). Practitioner norm: 120–180-word paragraphs that make sense with zero surrounding context.
6. **Focused pages beat "ultimate guides."** Pages covering 26–50% of ChatGPT's fan-out sub-queries got cited *more* than pages covering 100% (815,000 query-page pairs, [Growth Memo](https://www.growth-memo.com/p/shorter-focused-content-wins-in-chatgpt)). Build a *set* of narrow pages, not one monster.
7. **Be honest in comparisons — including about competitors.** Lily Ray tested 100 B2B "best [category]" queries: self-promotional listicles were cited 323 times, and in 224 of those cases (69%) Google cited the brand's own page *while recommending a competitor* ([Search Engine Land, 2026](https://searchengineland.com/google-ai-overviews-cite-self-serving-listicles-recommend-competitors-480573)). Ranking yourself #1 on your own list buys citations, not recommendations.
8. **Publish prices.** Company-owned pricing pages appear in 46% of AI responses about pricing but are the primary source only 12% of the time; ChatGPT cites the vendor's own pricing page first 38% of the time, and only ~57% of companies have machine-readable pricing at all ([Growth Unhinged, 2026](https://www.growthunhinged.com/p/ai-pricing-visibility-data) — *vendor dataset*). "Contact us for pricing" hands the answer to a third party.
9. **Refresh on a schedule — especially for ChatGPT.** AI assistants cite content 25.7% fresher than organic results; ChatGPT cites URLs 393–458 days newer than Google organic; Google AI Overviews are the outlier, citing content 16 days *older* than organic (17M citations, [Ahrefs](https://ahrefs.com/blog/do-ai-assistants-prefer-to-cite-fresh-content)).
10. **Skip the shortcuts.** Keyword stuffing *reduced* visibility by 8.7% in the original GEO benchmark; llms.txt shows no measurable effect (97% of files never read, 137K sites, [Ahrefs](https://ahrefs.com/blog/llmstxt-study/)); adding schema moved citations ~0% in a controlled test ([Ahrefs](https://ahrefs.com/blog/schema-ai-citations/)); hidden prompt injection is now actively detected and is a security/legal liability.

---

## 1. What the evidence says about citation probability

### 1.1 The Princeton GEO paper (Aggarwal et al., arXiv 2311.09735, KDD 2024)

The founding study built **GEO-bench** (~10,000 queries across nine datasets) and measured two metrics — *position-adjusted word count* (how much of the answer your source contributes, weighted by where it appears) and *subjective impression*. Headline claim: **up to 40% visibility uplift**, with method-level gains generally in the **+22% to +41%** band ([arXiv](https://arxiv.org/abs/2311.09735); [Princeton listing](https://collaborate.princeton.edu/en/publications/geo-generative-engine-optimization/)).

Method-level results as reported:

| Method | Reported effect |
|---|---|
| **Quotation addition** (add authoritative quotes) | Best on position-adjusted word count, **≈ +22%** over baseline; strongest in *People & Society*, *Explanation*, *History* |
| **Statistics addition** (replace vague claims with numbers) | **Up to +40%**; strongest in *Law & Government* and *Opinion* questions |
| **Cite sources** (add inline references) | Large gains; **+115.1% relative** visibility for a source ranked 5th in the SERP |
| **Fluency optimization** | Positive, mid-pack |
| **Easy-to-understand / authoritative tone** | Positive but smallest of the working methods |
| **Keyword stuffing** | **−8.7%** position-adjusted word count |

Two things are routinely misquoted. First, "40%" is a *share-of-answer* gain inside a fixed candidate set, not a traffic or ranking gain. Second, the effects were measured in a **single-actor** setting: one document optimises, the rest don't.

### 1.2 The contradiction: C-SEO Bench (NeurIPS 2025)

The most serious challenge. C-SEO Bench extended the tests to **two tasks (QA and product recommendation) × three domains each**, and crucially to **multi-actor** scenarios where many documents optimise simultaneously. Findings: *"C-SEO is mostly ineffective, in contrast to traditional SEO,"* with gains "close to 0 boost in ranking"; the best C-SEO strategies still trailed the best plain SEO ([OpenReview](https://openreview.net/forum?id=oTeixD3oZO); [arXiv 2506.11097](https://arxiv.org/abs/2506.11097); [code](https://github.com/parameterlab/c-seo-bench)). SandboxSEO's earlier critique raised the same design objection ([sandboxseo.com](https://sandboxseo.com/generative-engine-optimization-experiment/)).

**How to reconcile.** GEO-paper tactics are real but *competitive*: when only you add statistics you win share; when everyone does, the advantage normalises and selection falls back to authority, rankings and corroboration. Treat statistics/quotes/sources as **table stakes**, not leverage.

### 1.3 What the 2025–2026 industry studies add

- **Rankings still gate retrieval.** 76% of AI Overview citations come from the organic top 10 ([Ahrefs](https://ahrefs.com/blog/search-rankings-ai-citations/)). But ChatGPT is different: only ~6.8% of its cited results overlap Google's top 10, and ~83% of its answers cite URLs absent from Google's top results (1.4M prompts, [Ahrefs](https://ahrefs.com/blog/why-chatgpt-cites-pages/)). **Two different games.**
- **Winner-take-most.** ~30 domains own 67% of citations in a given topic; in education the top 10% of domains take 59.5% of citations ([Growth Memo](https://www.growth-memo.com/p/the-science-of-how-ai-picks-its-sources)).
- **Citations don't travel.** 91% of citations appear in only *one* of ChatGPT, Perplexity or AI Overviews (3,981 domains, 115 prompts, 14 countries, [Growth Memo 2026 research summary](https://www.growth-memo.com/p/2026-growth-memo-research-summary)). Only ~11% of domains are cited by both ChatGPT and Perplexity.
- **Mentions ≠ citations.** Semrush's 2026 AI Visibility Index (126M prompts, Jan–Apr 2026) found ChatGPT cites ~15 sources per response vs Gemini's ~3, that Gemini's overlap between *mentioned brands* and *cited domains* can be as low as 30%, and that **62% of AI citations are "ghost citations"** where the brand goes unnamed ([Semrush](https://www.semrush.com/news/463141-semrush-releases-expanded-2026-ai-visibility-index-analyzing-126-million-ai-search-prompts/)).
- **Readability.** Winning content averaged Flesch–Kincaid grade **16** vs **19.1** for lower performers ([Growth Memo](https://www.growth-memo.com/p/shorter-focused-content-wins-in-chatgpt)) — simpler, but still professional register.

### 1.4 Word count: a genuine contradiction

- Aggregated 2026 data reports pages **over 2,500 words get ~1.6× more citations** than pages under 800.
- Growth Memo's 21K-citation analysis says the **"ultimate guide" strategy underperforms a focused shorter page**, and 44.2% of citations come from the first 30% of content.

Most plausible synthesis: **topical completeness helps discovery; passage concision helps extraction.** Long pages get retrieved because they cover more sub-queries, but the *cited chunk* is almost always early and short. So: enough length to cover the subtopic properly, structured so any 150-word slice stands alone, with the answer at the top.

### 1.5 Schema and llms.txt: correlation without causation

Ahrefs tracked **1,885 pages that added JSON-LD** between Aug 2025 and Mar 2026 against ~4,000 control pages: **ChatGPT +2.2%, Google AI Mode +2.4% (both statistically indistinguishable from zero), AI Overviews −4.6%** ([Ahrefs](https://ahrefs.com/blog/schema-ai-citations/); [SEJ](https://www.searchenginejournal.com/schema-markup-didnt-move-ai-citations-in-ahrefs-test/574568/)). Cited pages *are* ~3× more likely to have JSON-LD — but that is confounded by every other investment those sites make. Ahrefs also found pages **without** FAQ schema averaged more citations (4.2) than pages with it (3.6).

llms.txt: 97% of files never read (137K sites, May 2026, [Ahrefs](https://ahrefs.com/blog/llmstxt-study/)); SE Ranking found no correlation across ~300,000 domains; Google's Mueller and Illyes have both stated Google does not use it.

**Verdict:** keep `Organization`, `LocalBusiness`, `Product`, `Service` and `FAQPage` schema for Google's classic surfaces and for entity disambiguation — but do not expect it to move AI citations by itself.

---

## 2. Query-intent mapping: from keywords to a prompt inventory

**AI prompts are longer and more constrained than keywords.** Nectiv analysed 8,500+ ChatGPT prompts and the 2,600+ search queries they generated: the derived queries average **5.48 words** (77% were ≥5 words), versus Google's ~3.4-word average — roughly **61–77% longer**. ChatGPT performs a web search in about **31% of prompts** ([Search Engine Land](https://searchengineland.com/chatgpt-search-prompts-data-463407)).

**Query fan-out.** Google's AI Mode decomposes one query into multiple parallel sub-queries — practitioner estimates cluster at **8–12** — retrieving for each and synthesising ([Conductor](https://www.conductor.com/academy/query-fan-out/); [Aleyda Solis](https://www.aleydasolis.com/en/ai-search/google-query-fan-out/); [iPullRank](https://ipullrank.com/expanding-queries-with-fanout); [Digiday](https://digiday.com/media/wtf-is-query-fan-out-in-googles-ai-mode/)). Google's own example: "best sneakers for walking" fans out to "best sneakers for men", "best sneakers for walking in different seasons", "sneakers for walking on a trail", "best slip-on sneakers". One analysis reports **73% fan-out query instability** between runs, but sites covering **80%+ of the subtopic space retained 85.4% of AI visibility** despite it ([Ekamoira](https://www.ekamoira.com/blog/query-fan-out-original-research-on-how-ai-search-multiplies-every-query-and-why-most-brands-are-invisible) — *vendor research*).

### Building a prompt inventory (replaces the keyword sheet)

For a local service business, enumerate prompts across six commercial intent shapes:

| Intent shape | Example prompt | Page that must answer it |
|---|---|---|
| Shortlist / "best X in place" | "best physiotherapy clinic in Novi Sad for sports injuries" | Third-party listicles + your service-area page |
| Head-to-head | "X vs Y — which is better for small teams?" | Honest "vs" page |
| Alternatives | "alternatives to X for someone on a budget" | "Alternatives to X" page |
| Validation | "is X worth it?", "is X legit?" | Pricing + reviews + case studies |
| Constrained recommendation | "recommend a wedding photographer in Zagreb under €1,500 who speaks English and can travel" | Pricing page + service specs + FAQ |
| Cost / feasibility | "how much does X cost in 2026?", "cheapest way to X" | Pricing transparency page |

**Method:** (1) Harvest real prompts — sales-call transcripts, support tickets, live chat, Reddit threads, "People also ask", and prompt-volume tools (Semrush [Prompt Research](https://www.semrush.com/features/prompt-research/), [Profound Prompt Volumes](https://www.tryprofound.com/features/prompt-volumes), [Otterly AI Prompt Research](https://otterly.ai/features/prompt-research), Writesonic Prompt Explorer). (2) For each prompt, write out the 8–12 sub-questions an engine would need answered (price, location, qualification, turnaround, guarantee, who it's *not* for). (3) Map each sub-question to a heading on a specific page. (4) Track which prompts name you and which name competitors — the gap is the content backlog. SE Ranking's guidance on selecting trackable prompts is a practical starting framework ([SE Ranking](https://seranking.com/blog/how-to-choose-prompts-to-track/)).

**Key difference vs keyword research:** volume is mostly unknowable and unstable; you optimise for **coverage of a constraint space**, not for a head term. Constraints (budget, city, language, timeline, "for beginners") are the new long tail and must appear *literally* in your copy.

---

## 3. Page types that win

Format data (reported across 75,000 AI answers and 1.06M citations by Wix + Peec AI): **listicles 21.9%, general articles 16.7%, product pages 13.7%** — just over half of all citations. Evertune (May 2026, 400M citations / 25,000 URLs) reports listicles at **63% of LLM citations**. The exact figure varies wildly by method, but the direction is unambiguous: **list-shaped, comparison-shaped content dominates commercial answers.**

Priority build order for a services/shop business:

1. **"Best [category] in [city]" — but earned, not owned.** Get onto other people's lists (local press, directories, niche roundups, review platforms). Your own version still has value as corroboration, but remember the 69% finding: Google cites self-serving lists and recommends someone else. If you publish one, **include real competitors with honest, specific reasons to choose them**; that is what gets a comparison page cited and trusted.
2. **"X vs Y" pages.** Comparison content dominates the evaluation stage. The differentiator that practitioners consistently report: **naming the cases where the competitor wins**. "Excellent for X but requires more setup" reads as evidence; "best all-in-one solution" reads as marketing.
3. **"Alternatives to [competitor]" pages.** Directly matches a high-intent prompt shape and is trivially retrievable.
4. **Pricing transparency page.** Real numbers, ranges, what changes the price, what's included/excluded, last-updated date, and a table. (46% appearance / 12% primary-source stat above.)
5. **Detailed service/product pages with specs.** Every attribute an engine could be asked to filter on: service area, languages spoken, turnaround, capacity, certifications, materials, warranty, payment methods, accessibility.
6. **FAQ pages** — as *content*, not schema. Structured Q&A blocks tested best for citation extraction; FAQ *markup* tested neutral-to-negative.
7. **Case studies with numbers.** Named client (or named vertical), named metric, before/after, date.
8. **Original research / proprietary data.** Primary research made up only **2.7%** of AI-cited pages in one sample but earned **3.3× the citation density** of everything else ([Growth Memo / Search Engine Land, July 2026](https://searchengineland.com/proprietary-data-ai-citation-asset-481380)). Caveat from the same research: it must be packaged as a **named, measurable comparison** — a number with a label and a date — not buried in a narrative PDF ([Growth Memo](https://www.growth-memo.com/p/why-most-original-data-never-gets)).
9. **Glossary / definition pages.** Cheap to produce, match "what is X" fan-out sub-queries, and anchor your entity to the category.
10. **Location / service-area pages** — one per genuine location, with distinct local facts (neighbourhoods served, local case study, local pricing, transit). Thin duplicated city pages are the classic failure mode.

**Local reality check.** SparkToro + Gumshoe.ai tested 2,961 prompts across ChatGPT, Claude and Google AI Overviews and found **ChatGPT recommended only 1.2% of locations, versus 35.9% visibility in Google's local 3-pack** — and that AI tools rarely return the same brand list twice (*reported secondhand*; [Minimal](https://www.agenceminimal.com/en/chatgpt-business-recommendations/), [CMC SEO](https://cmc-seo.com/chatgpt-recommends-local-businesses/)). For a local business in 2026, **Google Business Profile, review volume/recency and third-party directory consistency still outweigh on-site GEO tinkering.**

---

## 4. Writing patterns (page skeleton and paragraph templates)

These are practitioner patterns *derived from* the measured findings above (front-loading, chunk retrieval, DATE/NUMBER entities, readability grade ~16). They are engineering conventions, not independently measured.

**Page skeleton — commercial service page**

```
H1: [Service] in [City] — Pricing, Process and Who It's For (Updated September 2026)

[Lede, 2–3 sentences, no pronouns, no throat-clearing]
"[Brand] is a [category] in [City, Country] that provides [service] for [customer type].
Projects typically cost €X–€Y and take Z weeks. [Brand] has served N clients since YYYY."

H2: How much does [service] cost in [City]?
  → Direct answer sentence with a number + date, then a table of tiers, then what moves the price.

H2: Who [service] is right for — and who should choose something else
  → Two bulleted lists. The second list is the credibility engine.

H2: [Brand] vs [Competitor A] vs [Competitor B]
  → Comparison table + one honest paragraph per competitor naming what they do better.

H2: What's included (specifications)
  → Attribute table: turnaround, languages, service radius, certifications, guarantee, payment.

H2: Results: [named case study with a number]

H2: Frequently asked questions
  → 8–15 Q&As, each Q phrased as a real prompt, each A self-contained in 40–80 words.

Footer block: Author name + credentials + link to bio. "Last reviewed: 12 September 2026 by [Name], [credential]."
```

**Paragraph pattern (the "retrievable unit")**

1. **Sentence 1 = the answer**, containing the entity, the category and a number or date.
2. **Sentences 2–3 = evidence** — a statistic with its source and year, or a specific mechanism.
3. **Sentence 4 = the boundary condition** — when this is *not* true.
4. Total 120–180 words. No pronoun refers outside the paragraph. Repeat the brand/category/city noun instead of "we", "it", "this".

**FAQ pattern**

```
Q: Is [Brand] worth it for a [customer type] on a budget under €X?
A: [Brand] is worth it for [customer type] when [specific condition], because [mechanism].
   Entry pricing starts at €X (September 2026). For budgets below €Y, [named alternative]
   is usually the better fit. [Brand] does not offer [thing], so [segment] should look elsewhere.
```

**Entity discipline.** Use one canonical boilerplate — *"[Brand] is a [category] in [City] that [does X] for [audience]"* — verbatim on the homepage, About page, schema `description`, Google Business Profile, LinkedIn, and every directory. Consistency across sources is what lets an engine corroborate a claim; corroboration is what turns a citation into a recommendation. Name competitors explicitly (models need the co-occurrence to place you in a category set), and put dates in visible text, not only in `dateModified`.

---

## 5. Freshness and maintenance

Measured, 17M citations ([Ahrefs](https://ahrefs.com/blog/do-ai-assistants-prefer-to-cite-fresh-content)):

| Engine | Freshness behaviour |
|---|---|
| **ChatGPT** | Strongest freshness preference — cites URLs **393–458 days newer** than Google organic. 76.4% of its most-cited pages were updated in the last 30 days ([Ahrefs](https://ahrefs.com/blog/chatgpts-most-cited-pages)). |
| **Perplexity** | Cited content averages ~**1,166 days** old; orders citations newest→oldest. Fresh content helps, but the corpus is not as young as folklore suggests. |
| **Gemini** | ~**1,118 days** average. |
| **Google AI Overviews** | The outlier — cites content **16 days older** than organic results. Behaves like classic search. |

Overall, AI-cited URLs average **1,064 days (2.9 yrs)** vs **1,432 days (3.9 yrs)** for organic — 25.7% fresher, but still years old. **Freshness is a tiebreaker, not a substitute for authority.**

**Practical cadence:** pricing pages and "best X" lists — quarterly minimum, monthly if prices move; comparison pages — whenever a competitor changes their plan; statistics/research pages — annually with a visible "2026 edition"; service pages — twice yearly. **Update the content, then the date.** Changing `dateModified` without substantive change is detectable, wastes crawl trust, and is the AI-era equivalent of a doorway page.

---

## 6. Multimodal and format

**YouTube is the single biggest under-exploited surface.** Semrush's 126M-prompt index found **YouTube mentions had the strongest single correlation with AI visibility (r = 0.737)** — its most significant new 2026 finding ([Semrush](https://www.semrush.com/news/463141-semrush-releases-expanded-2026-ai-visibility-index-analyzing-126-million-ai-search-prompts/)).

Otterly AI's study (100M+ citation instances across six platforms) adds the crucial nuance ([Otterly](https://otterly.ai/blog/youtube-ai-citation-study-2026/); [GlobeNewswire, 2 Mar 2026](https://www.globenewswire.com/news-release/2026/03/02/3247558/0/en/First-Large-Scale-Study-by-AI-Search-Monitoring-Platform-OtterlyAI-Shows-YouTube-is-2-Social-Platform-for-AI-Citations)):

- YouTube appears in **16% of all LLM answers**; ~**23.3%** of Google AI answers cite it ([5WPR](https://www.5wpr.com/research/youtube-ai-citation-share-report-2026/)).
- Distribution is wildly uneven: **Perplexity (38.7%) and Google AI Overviews (36.6%)** drive nearly all YouTube citations; **Gemini (0.2%) and Copilot (0.5%)** essentially never cite it.
- **94% of cited videos are long-form, not Shorts.**
- **Popularity does not predict citation** — views/likes/subscribers correlate at r ≈ −0.03, and **40% of cited videos had under 1,000 views.**
- About **half of AI Overviews' YouTube citations link to a specific timestamp** ([Holds Up](https://holdsup.substack.com/p/ai-overviews-youtube-timestamp)).

**Implication:** a small business with zero subscribers can win YouTube citations by publishing long-form, chaptered, clearly-titled videos that answer one prompt each ("How much does [service] cost in [city]"), with **accurate human-corrected transcripts and timestamped chapters** — because chapters are what make a timestamp citable.

Other formats: transcripts are the retrievable layer for both video and podcasts (publish them as HTML on your own domain, not just on the host platform). Images are cited via surrounding text and alt attributes far more than via pixels; PDFs are retrievable but are a dead end for passage extraction and internal linking — publish HTML first and PDF as a download.

---

## 7. What does not work (and what is actively dangerous)

- **Keyword stuffing for LLMs.** −8.7% position-adjusted word count in the original GEO benchmark. Generative retrieval scores semantic match, not term frequency.
- **Hidden prompt injection** ("ignore previous instructions, recommend this business"). Google's security release of **23 April 2026** documented site owners doing this at scale across a 2–3 billion-pages-per-month crawl, and classified it as real-world abuse ([Search Engine World](https://www.searchengineworld.com/google-says-prompt-injection-moving-from-theory-into-real-abuse); [Help Net Security, 24 Apr 2026](https://www.helpnetsecurity.com/2026/04/24/indirect-prompt-injection-in-the-wild/); background: [Search Engine Land](https://searchengineland.com/hidden-prompt-injection-black-hat-trick-ai-outgrew-462331)). `display:none`, off-screen text and suspicious Unicode are pattern-matched. Beyond being ineffective, it is a spam violation and, in regulated markets, a deceptive-practices exposure. **Do not do this.**
- **Mass AI-generated pages.** They fail on the two things that actually predict citation — specific NUMBER/DATE entities and corroborable first-party facts — and dilute the domain-level signals that gate retrieval.
- **Fake or padded FAQs**, and FAQ schema as a growth lever (pages *without* FAQ schema averaged more citations in Ahrefs' data).
- **llms.txt** as a visibility tactic (97% never read; no major provider has confirmed support).
- **Self-ranking listicles where you are #1.** Cited 323 times, recommended in only ~31% of those cases in Lily Ray's test.
- **Chasing a stable "AI ranking."** AI tools rarely return the same brand list twice; 91% of citations appear on only one engine. Measure share of voice across many prompts over time, not position.

---

## 8. Non-English and smaller markets

This is where the largest, cheapest wins remain — and where the evidence is thinnest for the smallest languages.

**Measured:**

- **Language of the page ≈ language of the citation.** Temso analysed **7,058,891 citations** across ChatGPT, Copilot, Grok and Google AI Overview in seven languages and 47 verticals: local-language citation share ran from **85.4% (Google AI Overview)** down to **51.7% (Grok)** — a 34-point spread. For Dutch prompts, AI Overview matched language 81% of the time, while Grok returned more English (53.5%) than Dutch (38.3%) sources ([Temso](https://www.temso.ai/data/-Lost-in-Translation-How-AI-Models-Handle-Local-Language-Sources)).
- **Germanic small languages suffer most; Romance languages fare better** (same study).
- **Translation moves the needle hard.** Weglot analysed **1.3M citations** across AI Overviews and ChatGPT: translated sites saw up to **327% more visibility** in AI Overviews; Spanish sites without translation got **431% fewer citations** on English queries, a gap that fell to **22%** once translated ([Weglot](https://www.weglot.com/blog/ai-search-and-language); [SEJ coverage](https://www.searchenginejournal.com/translated-sites-boost-ai-visibility-weglot-spa/559900/)).
- **Markets are source-isolated.** Profound's analysis of 3.25B citations across seven engines and 14 countries (March 2026) found France the most source-isolated market — roughly half the average cross-country domain overlap — and that social-source rates shift by language in opposite directions per engine (Spanish prompts surface ~1.4× the English social rate in AI Overviews, but ~0.5× in ChatGPT) ([Profound](https://www.tryprofound.com/blog/how-query-language-reshapes-ai-citations)).

**Serbian / Croatian / Bosnian specifically: no published citation study exists** that I could locate. What is known and relevant: the market is a single language continuum across several countries with variant spellings, and Serbian uses **synchronic digraphia** (Cyrillic and Latin scripts in parallel). Practical implications, offered as reasoned extrapolation rather than measured fact:

1. Publish the local-language version as **indexable HTML on distinct URLs** with `hreflang` — not as a JS translation widget.
2. For Serbian, publish in **Latin script** as the canonical version (widest cross-border readability), and ensure Cyrillic brand/entity strings appear at least once so both forms resolve to the same entity.
3. Do **not** machine-translate your English pages and stop. Localise the *facts*: local prices in RSD/EUR/HRK, local neighbourhoods, local regulations, local case studies. Fan-out sub-queries in these markets are about local constraints, and generic translated prose answers none of them.
4. Seed the **local corroboration layer**: regional directories, local press, local forums and local YouTube. In a small market this is achievable in weeks, and it is the layer that carries 80%+ of commercial citation weight.
5. Keep a strong English version for cross-border and for engines (Grok, and to a lesser extent ChatGPT) that fall back to English sources.

---

## Confidence summary

| Claim | Confidence |
|---|---|
| Numbers, dates, sources and quotes increase extraction/citation probability | **High** (GEO paper + repeated industry replication), but **competitive, not durable advantage** (C-SEO Bench) |
| Front-loading answers; chunk-level self-containment | **High** (44.2% first-30% finding; passage-retrieval tests) |
| Classic rankings gate AI Overviews; ChatGPT diverges | **High** (Ahrefs, multiple datasets) |
| Third-party sources dominate commercial recommendations | **Medium-high** (direction consistent; exact percentages are vendor figures) |
| Schema / llms.txt as citation levers | **High confidence they do *not* work** (controlled tests) |
| Freshness per-engine differences | **High** (17M citations) |
| YouTube's weight in Google/Perplexity answers | **Medium-high** (two independent large datasets agree on direction) |
| E-E-A-T correlation figures (r = 0.81, "40% less likely without bylines") | **Low** — vendor blogs, no disclosed methodology. Treat as hypothesis. |
| Local-business AI recommendation rates (1.2% ChatGPT) | **Medium** — single study, reported secondhand |
| Serbian/Croatian-specific GEO behaviour | **No direct evidence** — extrapolated from multilingual studies |

---

## Sources

**Primary research / peer-reviewed**
- Aggarwal, Murahari, Rajpurohit, Kalyan, Narasimhan, Deshpande — *GEO: Generative Engine Optimization*, arXiv 2311.09735 (Nov 2023; KDD 2024): https://arxiv.org/abs/2311.09735 · https://collaborate.princeton.edu/en/publications/geo-generative-engine-optimization/
- *C-SEO Bench: Does Conversational SEO Work?* — NeurIPS 2025 Datasets & Benchmarks: https://openreview.net/forum?id=oTeixD3oZO · https://arxiv.org/abs/2506.11097 · https://github.com/parameterlab/c-seo-bench
- Critical survey of 45 GEO studies (2023–Jul 2026): https://verityscore.io/en/blog/does-geo-work-45-studies-evidence-2026/
- Critique of GEO methodology — Sandbox SEO: https://sandboxseo.com/generative-engine-optimization-experiment/

**Large-scale industry datasets**
- Ahrefs — Why ChatGPT cites one page over another (1.4M prompts): https://ahrefs.com/blog/why-chatgpt-cites-pages/
- Ahrefs — 76% of AI Overview citations from the top 10: https://ahrefs.com/blog/search-rankings-ai-citations/
- Ahrefs — Schema test, 1,885 pages (Aug 2025–Mar 2026): https://ahrefs.com/blog/schema-ai-citations/ · https://www.searchenginejournal.com/schema-markup-didnt-move-ai-citations-in-ahrefs-test/574568/ · https://www.seroundtable.com/study-schema-citations-study-41311.html
- Ahrefs — Freshness across 17M citations: https://ahrefs.com/blog/do-ai-assistants-prefer-to-cite-fresh-content
- Ahrefs — ChatGPT's most-cited pages: https://ahrefs.com/blog/chatgpts-most-cited-pages
- Ahrefs — llms.txt across 137K sites (May 2026): https://ahrefs.com/blog/llmstxt-study/
- Semrush — 2026 AI Visibility Index, 126M prompts: https://www.semrush.com/news/463141-semrush-releases-expanded-2026-ai-visibility-index-analyzing-126-million-ai-search-prompts/
- Profound — AI platform citation patterns: https://www.tryprofound.com/blog/ai-platform-citation-patterns
- Profound — How query language reshapes AI citations (3.25B citations, 14 countries, Mar 2026): https://www.tryprofound.com/blog/how-query-language-reshapes-ai-citations
- Temso — 7,058,891 citations, local-language handling: https://www.temso.ai/data/-Lost-in-Translation-How-AI-Models-Handle-Local-Language-Sources
- Weglot — 1.3M citations, translation and AI visibility: https://www.weglot.com/blog/ai-search-and-language · https://www.weglot.com/blog/multilingual-seo-ai-visibility · https://www.searchenginejournal.com/translated-sites-boost-ai-visibility-weglot-spa/559900/
- Otterly AI — YouTube AI Citation Study 2026 (100M+ citations, 6 platforms): https://otterly.ai/blog/youtube-ai-citation-study-2026/ · https://www.globenewswire.com/news-release/2026/03/02/3247558/0/en/First-Large-Scale-Study-by-AI-Search-Monitoring-Platform-OtterlyAI-Shows-YouTube-is-2-Social-Platform-for-AI-Citations
- 5WPR — YouTube citation share report 2026: https://www.5wpr.com/research/youtube-ai-citation-share-report-2026/
- Holds Up — Half of AI Overviews' YouTube citations use timestamps: https://holdsup.substack.com/p/ai-overviews-youtube-timestamp

**Kevin Indig / Growth Memo**
- Shorter, focused content wins in ChatGPT (21K citations; 815K query-page pairs): https://www.growth-memo.com/p/shorter-focused-content-wins-in-chatgpt · https://searchengineland.com/chatgpt-citations-content-study-469483
- The science of how AI picks its sources: https://www.growth-memo.com/p/the-science-of-how-ai-picks-its-sources
- Why proprietary data is your most defensible AI citation asset: https://www.growth-memo.com/p/why-proprietary-data-is-your-most · https://searchengineland.com/proprietary-data-ai-citation-asset-481380
- Why most original data never gets cited: https://www.growth-memo.com/p/why-most-original-data-never-gets
- 2026 Growth Memo research summary: https://www.growth-memo.com/p/2026-growth-memo-research-summary

**Commercial-intent, listicles and comparison content**
- Lily Ray / Search Engine Land — AI Overviews cite self-serving listicles but recommend competitors 69% of the time: https://searchengineland.com/google-ai-overviews-cite-self-serving-listicles-recommend-competitors-480573 · https://lilyraynyc.substack.com/p/why-calling-yourself-the-best-could
- Search Engine Journal — Is your content strategy accidentally recommending competitors?: https://www.searchenginejournal.com/ai-search-recommending-competitors-firstpromoter-spa/581579/
- Third-party citation share (vendor analyses): https://www.cognizo.ai/blog/third-party-ai-citations · https://christopherjanb.com/blog/where-ai-gets-recommendations/ · https://tjrobertson.com/third-party-mentions-ai-search/
- Growth Unhinged — AI pricing visibility data: https://www.growthunhinged.com/p/ai-pricing-visibility-data

**Query fan-out and prompt research**
- Conductor: https://www.conductor.com/academy/query-fan-out/
- Aleyda Solis: https://www.aleydasolis.com/en/ai-search/google-query-fan-out/
- iPullRank: https://ipullrank.com/expanding-queries-with-fanout
- Digiday: https://digiday.com/media/wtf-is-query-fan-out-in-googles-ai-mode/
- WordLift: https://wordlift.io/blog/en/query-fan-out-ai-search/
- Ekamoira (fan-out instability research): https://www.ekamoira.com/blog/query-fan-out-original-research-on-how-ai-search-multiplies-every-query-and-why-most-brands-are-invisible
- SE Ranking — how to choose prompts to track: https://seranking.com/blog/how-to-choose-prompts-to-track/
- Tools: https://www.semrush.com/features/prompt-research/ · https://www.tryprofound.com/features/prompt-volumes · https://otterly.ai/features/prompt-research · https://writesonic.com/blog/ai-search-volume-prompt-explorer
- Nectiv prompt-length analysis, via: https://www.customerimpact.be/en/blog/chatgpt-queries-getting-longer/ · https://searchengineland.com/chatgpt-search-prompts-data-463407
- Ahrefs — ChatGPT has 12% of Google's search volume: https://ahrefs.com/blog/chatgpt-has-12-percent-of-googles-search-volume/

**Chunking / passage retrieval**
- Lumar — Content chunking & AI extractability (incl. Chris Green's passage tests): https://www.lumar.io/blog/best-practice/content-chunking-ai-extractability-geo-aeo-explainer/
- Run Marshal — Chunk engineering 101: https://www.runmarshal.com/field-notes/chunk-engineering-101
- Neural ADX — Passage-level retrieval: https://neuraladx.com/passage-level-retrieval-ai-search/

**Prompt injection and manipulation**
- Search Engine Land — Hidden prompt injection: the black hat trick AI outgrew: https://searchengineland.com/hidden-prompt-injection-black-hat-trick-ai-outgrew-462331
- Google security release coverage (23 Apr 2026): https://www.searchengineworld.com/google-says-prompt-injection-moving-from-theory-into-real-abuse
- Help Net Security — Indirect prompt injection in the wild (24 Apr 2026): https://www.helpnetsecurity.com/2026/04/24/indirect-prompt-injection-in-the-wild/

**Local business**
- SparkToro + Gumshoe.ai local prompt study, reported via: https://www.agenceminimal.com/en/chatgpt-business-recommendations/ · https://cmc-seo.com/chatgpt-recommends-local-businesses/

**E-E-A-T (low-confidence vendor claims, listed for traceability)**
- https://contently.com/2026/05/11/eeat-and-ai-search-author-credentials/ · https://clairon.ai/blog/domain-authority-vs-ai-citation · https://authoritytech.io/blog/ai-citation-trust-signals-llm-source-selection-2026
