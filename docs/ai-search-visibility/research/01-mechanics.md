# How AI Answer Engines Retrieve, Rank and Cite Web Sources
### The mechanics a site owner must understand to get recommended
*Research compiled 2026-09-21*

> **Methodology note / confidence caveat.** Direct page fetching (WebFetch/curl) was blocked by the network egress policy in this environment. Every claim below is sourced to a URL discovered and summarised via web search, and I have graded each block as **[CONFIRMED]** (vendor/official documentation), **[VENDOR/STUDY]** (a named company's own large-scale measurement, methodology not independently verified), or **[CONTESTED/SPECULATIVE]** (third-party inference, reverse engineering, or sources that disagree). Where studies disagree I say so explicitly rather than picking a number. Treat single-vendor statistics as directional, not as ground truth.

---

## Key takeaways for site owners

1. **There is no single "AI index". There are at least five.** Google/Gemini use Googlebot's index; Microsoft Copilot uses Bing's (bingbot) with no separate Copilot crawler; Perplexity runs its own crawler and Vespa-based index; Claude's web search is powered by the **Brave Search API**; and OpenAI has moved from pure Bing dependence to a family of its own indexes (reported internal name "Labrador") plus third-party SERP/scraping providers. You must be present in *several different indexes*, not one.
2. **Robots.txt is now three separate decisions, not one.** OpenAI, Anthropic and Perplexity all split *training* from *search-index inclusion* from *live user-triggered fetch*. Blocking `GPTBot` (training) is fine; blocking `OAI-SearchBot` removes you from ChatGPT search answers. Blocking `ClaudeBot` is fine; blocking `Claude-SearchBot` removes you from Claude's search index. There is **no** equivalent split on Google: `Google-Extended` controls Gemini training/grounding but **cannot** remove you from AI Overviews without also removing you from Search.
3. **Almost no AI fetcher executes JavaScript.** Vercel/MERJ measured zero JS execution across ~500M GPTBot fetches; ClaudeBot and PerplexityBot behave the same. If your price, availability, reviews or core copy only exist after hydration, most engines literally cannot see them. Gemini/AI Overviews is the exception because it inherits Googlebot's rendering.
4. **Retrieval is passage-level and fan-out driven, not page-level.** A single user prompt becomes ~8–12 (sometimes far more) synthetic sub-queries. You are not competing for "the query" — you are competing for each sub-query. The strongest measured predictor of ChatGPT citation in Ahrefs' 1.4M-prompt study was *cosine similarity between your page title and the fan-out sub-query* (~0.656 for cited URLs).
5. **Most engines never open your page.** RESONEO's corpus of 1,200 ChatGPT answers / 88,000 results found roughly **1 page opened per 80 retrieved**, and almost exclusively in paid "thinking" mode. The default unit of evidence is title + URL + ~200-character snippet. Optimise the snippet-visible layer first.
6. **Ranking in Google's top 10 no longer guarantees AI citation.** Overlap estimates for AI Overviews range from 76% (Ahrefs 2025) down to 38% (Ahrefs/AirOps 2026), ~17% (BrightEdge), and under 20% (5W). For ChatGPT, Ahrefs found only ~12% of AI-cited URLs rank in Google's top 10 for the original prompt. Direction agrees; magnitude does not.
7. **Third-party corroboration beats on-site optimisation.** Reddit, Wikipedia, YouTube, LinkedIn, G2, Trustpilot, Yelp and review aggregators dominate citation share. Seer found brands with *any* Trustpilot profile (1–13 reviews) jumped from ~1% to ~53.5% median AI citation rate.
8. **Freshness is a position you re-earn.** Ahrefs measured ~17M citations: cited URLs averaged 1,064 days old vs 1,432 for organic; ~half of AI citations trace to content updated in the last 13 weeks; median citation half-life ≈ 4.5 weeks.
9. **Commerce runs on feeds, not crawling.** ChatGPT Shopping, Google AI Mode and Perplexity Shopping all prefer a **pushed structured product feed** over crawled product pages. If you sell things and have no feed in OpenAI's product feed spec, Google Merchant Center and Perplexity's Merchant Program, you are competing with one hand tied.
10. **Local is an API problem, not an SEO problem.** ChatGPT's local answers lean heavily on Foursquare Places and Yelp (partner integrations); Google AI Mode leans on Google Business Profile / Maps. Your website is the *third* input, not the first.
11. **`llms.txt` is, on current evidence, inert.** Google (Illyes, Mueller) says it is unsupported; Ahrefs found 97% of llms.txt files received zero traffic; no major provider documents it as a signal. Do not spend budget there.

---

## 1. Indexes, crawlers and what robots.txt actually controls

### OpenAI / ChatGPT
**[CONFIRMED]** OpenAI documents three agents with separate robots.txt tokens ([developers.openai.com/api/docs/bots](https://developers.openai.com/api/docs/bots)):
- **GPTBot** — collects public web content that *may be used to train* foundation models. Disallowing it signals "do not train on me."
- **OAI-SearchBot** — exists to *surface sites in ChatGPT search*. Opting out means you will not appear in ChatGPT search answers. This is the one that matters commercially.
- **ChatGPT-User** — fires when a *user action* in ChatGPT or a GPT causes a page visit. OpenAI now documents that robots.txt **may not apply** here because a human initiated the fetch.

These are independent, so allow-search / block-training is a legitimate configuration ([xseek.io OpenAI user agents](https://www.xseek.io/docs/openai-crawlers-and-user-agents); [anagram.ai GPTBot explained, 2026](https://www.anagram.ai/blog/gptbot-explained-how-chatgpt-crawls-sees-and-cites-your-site-in-2026)).

**Which index?** This is where the sources disagree most sharply.
- **[CONTESTED]** One camp says Bing is still the backend: "ChatGPT uses Bing, not Google… relies on Microsoft's Bing index rather than maintaining its own" ([aiplusautomation.com, 2026](https://aiplusautomation.com/blog/chatgpt-bing-or-google)); a local-SEO source claims "ChatGPT uses the Bing Search API for 92% of its real-time web searches" ([surfacelocal.com, 2026](https://www.surfacelocal.com/blog/how-chatgpt-finds-local-businesses)).
- **[VENDOR/STUDY — more recent and better evidenced]** Between 21 May and 21 July 2026, observers captured ChatGPT surfacing source provenance labels reading **"Labrador"** (OpenAI's own index), **"Bright"**, **"Oxylabs"** and **"SERP"** — i.e. one in-house index plus three external scraping/SERP providers ([peec.ai](https://peec.ai/blog/chatgpt-built-its-own-search-index); [Search Engine Land, Aug 2026](https://searchengineland.com/chatgpt-retrieval-stack-index-cache-pages-485036)). Labrador is reported to be a *family* of vertical indexes — general web, PDFs, YouTube, news, arXiv, Wikipedia, **local listings**, finance, legal, medical, **shopping**, images — storing full page content, crawl date and publication date. An A/B flag named `prefer-index-over-serp-v3` was reportedly live on ~8% of chats in mid-August 2026.
- **Practical reading:** ChatGPT in 2026 is a *hybrid* — own index growing, third-party SERP/scraper feeds shrinking. Being in Bing still helps; being crawlable by OAI-SearchBot increasingly matters more.

**[VENDOR/STUDY]** Search Engine Land describes a three-layer stack: a **discovery index** that finds pages, a **reading cache** holding full copies of previously fetched pages, and a small set of pages opened **live** ([Search Engine Land, Aug 2026](https://searchengineland.com/chatgpt-retrieval-stack-index-cache-pages-485036); [seroundtable on the ChatGPT web cache](https://www.seroundtable.com/openai-chatgpt-web-cache-41312.html)).

### Google (AI Overviews, AI Mode, Gemini app)
**[CONFIRMED]** `Google-Extended` is **not a user agent** — it never appears in server logs. It is a robots.txt control token evaluated against pages Googlebot already crawled, deciding whether that content may be used for Gemini training and grounding ([Menra AI Overviews crawler guide](https://www.menra.ai/guides/ai-overviews-crawler-guide); [Menra Gemini crawler guide](https://www.menra.ai/guides/gemini-crawler-guide); [amicited.com glossary](https://www.amicited.com/glossary/google-extended/)). Introduced September 2023.

**[CONFIRMED]** AI Overviews are built from Googlebot-crawled data. There is **no way to appear in Google Search but not in AI Overviews** short of blocking Googlebot entirely ([aicrawlercheck.com, 2026](https://aicrawlercheck.com/blog/google-extended-vs-googlebot)). Blocking `Google-Extended` has no effect on rankings or indexation.

**[CONFIRMED]** Google published official guidance, "Optimizing your website for generative AI features on Google Search," under a new *Generative AI fundamentals* section of Search Central — the first on-record statement of what works for AI Overviews/AI Mode ([Semrush coverage](https://www.semrush.com/blog/google-publishes-generative-ai-search-guide/); [Stackmatix summary](https://www.stackmatix.com/blog/google-search-central-ai-overviews-guidance)). Core requirement: **pages must be indexed and eligible for standard search snippets to be citable.**

### Anthropic / Claude
**[CONFIRMED]** Anthropic's crawler documentation was updated **20 February 2026** and now names three bots with independent robots.txt tokens ([seroundtable](https://www.seroundtable.com/anthropic-updates-its-crawler-docs-40978.html); [Search Engine Land](https://searchengineland.com/anthropic-claude-bots-470171); [Search Engine Journal](https://www.searchenginejournal.com/anthropics-claude-bots-make-robots-txt-decisions-more-granular/568253/)):
- **ClaudeBot** — training-data collection.
- **Claude-SearchBot** — "content crawled by Claude-SearchBot is used to build and improve Claude's search index."
- **Claude-User** — user-initiated fetches when a person asks Claude something.

Blocking the training bot does **not** block the search bot or the user fetcher — you must list all three to block Anthropic entirely.

**[VENDOR/STUDY, strongly corroborated]** Claude's *consumer* web search is powered by **Brave Search**: Anthropic listed Brave Search as a Web Search subprocessor on **19 March 2025**; the web-search tool contains a parameter literally named `BraveSearchParams`; testers report Claude's citations match Brave's top results ~87% of the time ([TechCrunch, 21 Mar 2025](https://techcrunch.com/2025/03/21/anthropic-appears-to-be-using-brave-to-power-web-searches-for-its-claude-chatbot/); [cloro.dev](https://cloro.dev/blog/brave-search-api-vs-serp-api/); [shahabpapoon.com](https://www.shahabpapoon.com/blog/claude-web-search-runs-on-brave); [Profound](https://www.tryprofound.com/blog/what-is-claude-web-search-explained)). Brave's index is independent — ~30bn+ pages, 100M+ updates/day. **Implication: being indexed by Brave is a direct, under-contested lever for Claude visibility.**

**[CONFIRMED]** Claude's web search tool runs *iterative* searches — it uses results from one search to refine the next, continuing until it has enough information or hits a preset limit; every web-sourced response carries citations ([platform.claude.com web search tool docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool); [claude.com/blog/web-search-api](https://claude.com/blog/web-search-api)).

### Perplexity
**[CONFIRMED]** `PerplexityBot` indexes for search results; `Perplexity-User` fetches on behalf of a live user. Perplexity states PerplexityBot respects robots.txt and does not use blocked content for pre-training — but **if a page is blocked it may still index the domain, headline and a brief factual summary** ([Perplexity help centre](https://www.perplexity.ai/help-center/en/articles/10354969-how-does-perplexity-follow-robots-txt); [docs.perplexity.ai crawlers](https://docs.perplexity.ai/docs/resources/perplexity-crawlers)). **[CONTESTED]** Third parties report `Perplexity-User` does not honour robots.txt the same way, so both strings must be listed to block fully ([51degrees research, 2026](https://51degrees.com/blog/perplexity-ai-2026)).

**[VENDOR/STUDY]** Perplexity brought search in-house in **April 2025**, rebuilding on **Vespa** (retrieval, ranking and ML inference in one serving layer), running its own crawler plus licensed third-party crawlers, over an index it describes as covering hundreds of billions of pages with tens of thousands of index updates per second, using hybrid retrieval and cross-encoder reranking ([theaiengineer.substack.com](https://theaiengineer.substack.com/p/how-perplexity-built-their-search); [eseospace.com](https://eseospace.com/blog/how-perplexity-indexing-works-2026/)). It is **not** a Bing or Google reseller.

### Microsoft Copilot / Bing
**[CONFIRMED-ish]** There is **no separate Copilot crawler or index** — bingbot does the indexing, Copilot retrieves candidates from Bing's index, and the model composes a cited answer ([rankmax](https://www.rankmax.com.au/articles/microsoft-copilot-seo); [Winston Digital](https://www.winstondigitalmarketing.com/playbooks/bing-copilot-optimization/)). **IndexNow** is the push protocol for instant recrawl. In **February 2026** Microsoft shipped an **AI Performance** report in Bing Webmaster Tools (public preview) showing when your site is cited in Copilot, Bing AI summaries and select partner integrations ([Bing Webmaster blog, Feb 2026](https://blogs.bing.com/webmaster/February-2026/Introducing-AI-Performance-in-Bing-Webmaster-Tools-Public-Preview)) — currently the **only first-party AI-citation reporting** any engine offers.
**[CONTESTED]** Third parties describe a 2026 grounding rebuild called **"Web IQ"**, an AI-native retrieval layer that reasons about *how* to search rather than keyword-matching ([Lawrence Hitches](https://www.lawrencehitches.com/copilot-search-optimization/)). Treat as unconfirmed.

### Others as of 2026
- **Apple** — Applebot's documentation was extended to state that fetched data "may be used to help train Apple foundation models powering generative AI features," explicitly including Apple Intelligence and Siri; `Applebot-Extended` is the training opt-out. Apple unveiled a conversational Siri with real-time world knowledge at WWDC 2026 ([cicero.studio](https://cicero.studio/en/blog/apple-applebot-siri-ai-content-visibility-2026/); [nohacks.co](https://nohacks.co/blog/apple-on-the-agentic-web)).
- **Grok (xAI)** — real-time web search combined with X/Twitter data ([maxaeo.ai](https://maxaeo.ai/blog/which-search-engines-power-ai-answers/)).
- **Meta AI** — embedded in WhatsApp/Instagram/Facebook/Messenger on Llama; no separable domain, so its share is not independently tracked.
- **Market share context [VENDOR]:** worldwide AI-chatbot web-visit share, May 2026 — ChatGPT 53.9%, Gemini 27.9%, Claude 9.2%, DeepSeek 4.1%, Grok 2.4%, Perplexity 1.3%, Copilot 1.3% ([Momentic](https://momenticmarketing.com/blog/top-ai-chatbots); [Presenc](https://presenc.ai/research/ai-search-engine-market-share-2026)). US: ChatGPT 58.3%, Gemini 19.3%, Claude 13.4%.

### The access layer above robots.txt
**[CONFIRMED]** Cloudflare made AI-crawler blocking **default-on for new customers from 1 July 2025** ("Content Independence Day") and launched **Pay Per Crawl**, gating access behind HTTP 402 ([Cloudflare blog: pay per crawl](https://blog.cloudflare.com/introducing-pay-per-crawl/); [Cloudflare: content independence day](https://blog.cloudflare.com/content-independence-day-ai-options/); [TechCrunch, 1 Jul 2026](https://techcrunch.com/2026/07/01/cloudflares-new-policy-pushes-ai-companies-to-pay-for-publishers-content/)). Its **Content Signals Policy** defines three signals — `search`, `ai-input` (live answer grounding) and `ai-train`. **From 15 September 2026, Cloudflare defaults block "mixed-use" crawlers from any page hosting ads.** Cloudflare is explicit that these are *stated preferences, not enforcement*.
**Action:** check your CDN's AI-bot settings. Many site owners are blocked from ChatGPT and Perplexity by a default they never chose.

---

## 2. How retrieval actually works

### Query fan-out
**[CONFIRMED via patent]** Google patent application **US20240289407A1** describes using an LLM to generate multiple alternate queries from one search, decomposing by "sub-themes." Patent **US11663201B2** documents eight synthetic query types: *equivalent, follow-up, generalization, specification, canonicalization, language translation, entailment, clarification* ([SEJ on query fan-out](https://www.searchenginejournal.com/query-fan-out-technique-in-ai-mode-new-details-from-google/552532/); [Semrush](https://www.semrush.com/blog/query-fan-out/); [iPullRank](https://ipullrank.com/expanding-queries-with-fanout)).
**[VENDOR/STUDY]** Practitioner measurement puts typical fan-out at **8–12 parallel sub-queries**; Google has publicly referenced firing "hundreds of searches" for complex AI Mode tasks ([usehall.com](https://usehall.com/guides/query-fan-out-ai-mode); [wordlift.io](https://wordlift.io/blog/en/query-fan-out-ai-search/)). ChatGPT runs its own fan-outs, which are observable and drifting over time ([Lily Ray](https://lilyraynyc.substack.com/p/what-we-can-learn-from-evolving-chatgpt); [RESONEO fan-out capture tool](https://think.resoneo.com/scrap-chatgpt-plugin/)).

### Chunking, passages and the "custom corpus"
**[VENDOR/STUDY]** Google's AI surfaces build a **custom corpus** — a set of *passages* related to the query and its fan-outs — then use specialised models to summarise, compare and extract from it. AI Overviews are grounded in results Google already retrieved and ranked; **AI Mode is grounding plus fan-out**, running parallel sub-queries across the index, the Knowledge Graph and real-time data ([Green Flag Digital](https://greenflagdigital.com/learning-ai/google-ai-overviews-vs-ai-mode-gemini/); [wislr.com](https://www.wislr.com/articles/gemini-vs-ai-overviews-vs-ai-mode/); [singularity.digital](https://singularity.digital/insights/how-gemini-and-google-ai-mode-rank-content/)).
The standard RAG pipeline underneath — input encoder → neural retriever over chunked, embedded passages → output generator — is well described by iPullRank ([How RAG is Redefining SEO](https://ipullrank.com/how-retrieval-augmented-generation-is-redefining-seo)). **The consequence is that the unit of competition is the passage, not the page.**

### How much of your page is actually read
This is the single most under-appreciated mechanic.
**[VENDOR/STUDY — RESONEO, August 2026]** Corpus of 1,200 ChatGPT answers, 88,000 search results, 26,900 distinct pages ([think.resoneo.com/chatgpt-retrieval/](https://think.resoneo.com/chatgpt-retrieval/)):
- ChatGPT typically does **not read your page**. It queries one or more engines and takes **title, URL and roughly 200 characters**.
- **Out of ~80 pages pulled, only one is opened.** Of 759 opens in the corpus, 757 were in paid *thinking* mode; only two free-tier conversations opened a page. With the Think button, a free account opens ~0.28 pages per conversation.
- **When ChatGPT does open a page, it cites it three times out of four.** Getting opened is the hard part; getting cited after being opened is nearly automatic.

**[VENDOR/STUDY — Ahrefs, 1.4M ChatGPT prompts]** ([ahrefs.com/blog/why-chatgpt-cites-pages/](https://ahrefs.com/blog/why-chatgpt-cites-pages/)):
- Only **49.98%** of retrieved URLs are cited.
- The dominant factor is **semantic similarity between page title and the internal fan-out query** (mean cosine similarity 0.656 for cited URLs).
- Pages from ChatGPT's **general search index** have an **88%** citation rate; **Reddit** pages are 67.8% of *non-cited* URLs and are cited only **1.93%** of the time — retrieved for context, almost never credited ([SEJ coverage](https://www.searchenginejournal.com/chatgpt-often-retrieves-but-rarely-cites-reddit-pages-data-shows/572243/)).
- **Natural-language URL slugs** correlate with an **89.78%** citation rate vs **81.11%** otherwise.

**Practical translation:** your `<title>` and URL slug do disproportionate work, because for ~79 of every 80 retrieved pages they are the *only* thing the model sees. Write titles that match the *sub-questions* a buyer's prompt decomposes into, not the head term.

### JavaScript rendering
**[VENDOR/STUDY — Vercel + MERJ]** Analysis of real crawler traffic found **no major AI crawler renders JavaScript**. GPTBot fetched JS files in ~11.5% of requests and ClaudeBot in ~23.84% — but never executed them; across ~500M GPTBot fetches, **zero JS execution** ([Vercel: The rise of the AI crawler](https://vercel.com/blog/the-rise-of-the-ai-crawler); [SearchOptimo 2026 retest](https://searchoptimo.com/blog/do-ai-crawlers-render-javascript)). GPTBot, ClaudeBot and PerplexityBot extract text from initial markup. The exception is Google's stack, which inherits Googlebot rendering.
**Action:** server-render or statically render anything that must be cited — especially price, stock, ratings, service areas and phone numbers. A retail audit found Target's price was simply *absent* to AI crawlers ([MarketerFirst](https://marketerfirst.com/hub/what-ai-crawlers-can-read-on-product-pages/)).

### Freshness
**[VENDOR/STUDY — Ahrefs, ~16.975M citations across ChatGPT, Perplexity, Gemini, Copilot, AIO]**: cited URLs averaged **1,064 days old** vs **1,432 days** for the same queries' organic Google results — AI cites content **~25.7% fresher**; ~**half** of AI citations trace to content updated within **13 weeks**. ChatGPT is the most recency-biased (in-text references 393 days newer than organic; end-of-answer citations 458 days newer); Perplexity ~250 days newer; **AI Overviews shows the weakest freshness bias**. Median citation half-life ≈ **4.5 weeks** ([salespeak.ai summary](https://salespeak.ai/aeo-news/content-freshness-ai-search/); [growganic.io](https://growganic.io/blog/content-freshness-and-ai-citations); [Seer: AI brand visibility and content recency](https://www.seerinteractive.com/insights/study-ai-brand-visibility-and-content-recency)).

---

## 3. What the large-scale studies say gets cited

### The founding academic result
**[PEER-REVIEWED]** *GEO: Generative Engine Optimization* — Aggarwal, Murahari, Rajpurohit, Kalyan, Narasimhan, Deshpande (Princeton + IIT Delhi/Georgia Tech), arXiv **2311.09735**, published at **ACM SIGKDD 2024** ([arxiv.org/abs/2311.09735](https://arxiv.org/abs/2311.09735); [Princeton record](https://collaborate.princeton.edu/en/publications/geo-generative-engine-optimization/)). GEO-Bench: ~10,000 queries, nine datasets; Google top-5 sources per query, GPT-3.5-turbo synthesising cited answers.
- Headline "**up to 40%**" visibility lift is a **maximum, not an average**; the three strongest methods gave **+30–40% relative** on Position-Adjusted Word Count.
- Winning tactics: **Statistics Addition** (replace vague claims with numbers) — +30–40% PAWC, +15–30% subjective impression; **Quotation Addition** (strongest in People & Society / Explanation / History); **Cite Sources** — measured **+115.1% relative visibility lift for a site ranked 5th** that added citations.
- **Critique to keep in mind:** the study used GPT-3.5 over a 5-source Google-grounded pipeline in 2023. Modern engines fan out, cache and rarely open pages, so the mechanism has changed even if the direction (specific, quotable, numeric, sourced passages win) has held up ([Blck Alpaca methodology critique](https://blckalpaca.at/en/knowledge-base/seo-geo/geo-generative-engine-optimization/the-princeton-geo-study-methodology-results-and-critique)).

### Ranking overlap — where sources disagree most
| Finding | Source | Date |
|---|---|---|
| 76.1% of AI Overview citations also rank top-10 | Ahrefs | 2025 |
| 38% of AIO-cited pages rank top-10 (863k keywords, 4M URLs) | Ahrefs / AirOps | Mar 2026 |
| ~17% top-10 overlap for AIO citations | BrightEdge | 2026 |
| Overlap collapsed from ~70% to under 20% | 5W Research | 2026 |
| AI Mode: 51% *domain* overlap, 32% *URL* overlap with top 10 (sidebar); 89%/80% for below-answer links | Semrush AI Mode study | 2026 |
| Only **12%** of AI-cited URLs rank in Google's top 10 for the original prompt | Ahrefs | 2026 |

Sources: [Ahrefs AI search overlap](https://ahrefs.com/blog/ai-search-overlap/); [SEJ: AIO citations from top-ranking pages drop sharply](https://www.searchenginejournal.com/google-ai-overview-citations-from-top-ranking-pages-drop-sharply/568637/); [ALM Corp on the 76%→38% shift](https://almcorp.com/blog/google-ai-overview-citations-drop-top-ranking-pages-2026/); [5W/PRNewswire](https://www.prnewswire.com/news-releases/new-5w-research-overlap-between-top-google-rankings-and-ai-cited-sources-has-collapsed-from-70-to-under-20-302760132.html); [Semrush AI Mode study](https://www.semrush.com/blog/ai-mode-comparison-study/).

**Reconciliation:** the numbers differ by more than 4x because of differing query mixes, dates and whether "citation" means the inline link or the sidebar. **All agree on direction: top-10 organic is now a minority of AI citations.** Note the striking Semrush detail — the *below-answer* links in AI Mode are 89% top-10 overlap while the *sidebar* is 51%, i.e. different slots in the same interface are drawn from different pools.

### Which domains dominate
**[VENDOR/STUDY — Profound, ~680M citations, Aug 2024 – Jun 2025]** ([tryprofound.com](https://www.tryprofound.com/blog/ai-platform-citation-patterns)), aggregated with five other studies into the **5W AI Platform Citation Source Index 2026** (680M+ citations across ChatGPT, AIO, Perplexity, Gemini, Claude, Aug 2024 – Apr 2026) ([5wpr.com](https://www.5wpr.com/research/state-of-ai-citations-2026/); [PRNewswire](https://www.prnewswire.com/news-releases/5w-releases-ai-platform-citation-source-index-2026-the-50-websites-that-now-decide-what-brands-are-visible-inside-chatgpt-claude-perplexity-gemini-and-google-ai-overviews-302759804.html)):
- **Reddit is #1 across every major engine**, ~40% frequency.
- **Wikipedia dominates ChatGPT** — figures range 26%–48% of top-10 citation share (Profound reports 47.9%).
- **Perplexity skews to review/comparison aggregators**: G2, Gartner, NerdWallet, PCMag, TripAdvisor, Yelp; Reddit ≈46.7% of its top-10 source share.
- Top 15 domains capture **68%** of consolidated AI citation share.
- **[CONTESTED]** Some analyses argue no single domain exceeds ~5% of *total* citations with the other 95% spread across thousands of domains ([everything-pr.com](https://everything-pr.com/ai-platform-citation-source-index-2026)). This conflicts with the "Wikipedia = 47.9% of ChatGPT" figure — the difference is almost certainly *top-10 share* vs *all-citation share*. **Read every citation-share statistic for its denominator.**
- **[VENDOR/STUDY — Kevin Indig]** Top 10 domains take ~46% of citations while **58% of URLs are cited only once**, and **91% of citations appear in only one** of ChatGPT / Perplexity / AI Overviews ([Growth Memo: the science of how AI picks its sources](https://www.growth-memo.com/p/the-science-of-how-ai-picks-its-sources); [State of AI Search Optimization 2026](https://www.growth-memo.com/p/state-of-ai-search-optimization-2026)). Cross-engine visibility is close to uncorrelated — you must optimise per engine.
- **[VENDOR/STUDY]** Indig's analysis of 815,000 query-page pairs found the "ultimate guide" strategy produces **worse** citation results than a focused, shorter page. Winning assets are definitive guides, comparison pages, original research, glossaries and category explainers — pages that answer a *cluster* of sub-questions with clean, quotable passages placed early.

### Brand mentions and reviews
**[VENDOR/STUDY — Seer Interactive]** ([What drives brand mentions in AI answers](https://www.seerinteractive.com/insights/what-drives-brand-mentions-in-ai-answers); [800k AI responses / reviews study](https://www.seerinteractive.com/insights/study-of-800k-ai-responses-how-reviews-shape-brand-presence-in-ai-search)):
- Across **541,213 LLM responses, 20 brands, six platforms**, Seer proposes that the model **generates the answer first**, choosing brands from trained memory, **then retrieves sources to support those choices**. *"The citations are the bibliography, not the brainstorm."* This is the single most important mental model in this report.
- Brand **search volume** correlates with AI mentions at only **r ≈ 0.18** — real but modest.
- **Google page-1 ranking** correlates with LLM mentions at **r ≈ 0.65**; Bing ranking **r ≈ 0.5–0.6**.
- **Trustpilot effect:** brands with no Trustpilot profile have a **1%** median AI citation rate; brands with even **1–13 reviews** jump to **53.5%**. ChatGPT led with a **57.9%** Trustpilot citation rate. *(Caveat: correlational, and having a Trustpilot profile proxies for being a real, established commercial entity.)*
- Corpus: ~410,000 public brand mentions, 240 brands, 90 days (19 Apr – 17 Jul 2026), cross-referenced against commercial-investigation prompts on AIO, Perplexity and ChatGPT.

### Structured data — the clearest disagreement in the field
- **[VENDOR/STUDY, positive]** Sites with complete schema see 2.5x higher AI citation rates; 2.7x more likely to be cited in Perplexity ([stackmatix](https://www.stackmatix.com/blog/structured-data-ai-search)); an SSRN cross-platform empirical study by Kurt Fischman tests the same question ([SSRN 6284518](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6284518)).
- **[VENDOR/STUDY, null — better design]** Ahrefs tracked **1,885 pages after JSON-LD was added against 4,000 matched controls**: AI Overviews citations **−4.6%**, AI Mode **+2.4%**, ChatGPT **+2.2%** — all **statistically indistinguishable from zero** ([roiandshine summary](https://roiandshine.com/blog-ai/geo-seo/schema-markup-for-llm-citation/); [Loonis](https://www.loonis.co/blog/schema-markup-for-ai-search-what-actually-drives-citations-in-2026)).
- **[CONTESTED]** A February 2026 controlled experiment reports ChatGPT and Perplexity **tokenize JSON-LD as raw text** — reading the script block as a plain string, not as parsed structured data.
- **Verdict:** treat schema as **hygiene and entity disambiguation**, not a growth lever. The cross-sectional "2.5x" results are almost certainly confounded — sites with complete schema are also the sites with budget, authority and editorial discipline. Schema is table stakes for Google Merchant Center and rich results, which is reason enough to do it.

### llms.txt
**[CONFIRMED negative]** Gary Illyes confirmed at Google Search Central Live that Google does not support llms.txt and has no plans to; John Mueller compared it to the keywords meta tag; Google documentation (June 2026) states it has no effect on Search or AI Overviews. Ahrefs found **97% of llms.txt files received zero traffic in May 2026**; SE Ranking found **10.13% adoption** across 300,000 domains; monitoring of 500M+ AI bot visits over 90 days found only **408** requests targeting llms.txt ([seroundtable](https://www.seroundtable.com/google-ai-llms-txt-39607.html); [geojacker.com: what the 2026 data shows](https://geojacker.com/llms-txt); [digitalapplied.com adoption data](https://www.digitalapplied.com/blog/llms-txt-in-practice-adoption-evidence-2026)).

---

## 4. Training data vs. inference-time retrieval

These are two separate channels with almost nothing in common operationally.

| | **In the training data** | **Retrieved at inference** |
|---|---|---|
| Controlled by | `GPTBot`, `ClaudeBot`, `Google-Extended`, `Applebot-Extended` | `OAI-SearchBot`, `Claude-SearchBot`, `PerplexityBot`, `Googlebot`, `bingbot` |
| Latency | Months to years; frozen at cutoff | Minutes to days |
| What you can do | Almost nothing directly — you can only *become the kind of entity that gets written about*, mostly by third parties | A great deal: crawlability, titles, passage structure, feeds, freshness, index presence |
| Failure mode | Model has never heard of your brand → never nominated | Brand is nominated but no citable source exists → competitor gets the link |

**[VENDOR/STUDY]** Seer's finding that the model **names brands from memory first and retrieves citations second** ([Seer](https://www.seerinteractive.com/insights/what-drives-brand-mentions-in-ai-answers)) means the two channels do different jobs: **training/parametric memory decides *whether you are considered*; retrieval decides *whether you are linked*.** A business with strong retrieval hygiene but no parametric presence gets cited as a supporting source in answers that recommend someone else.

The practical consequence: **you influence the parametric channel indirectly, through the corpora that get trained on** — Wikipedia, Reddit, YouTube transcripts, G2/Trustpilot/Yelp, industry press, LinkedIn, and comparison/listicle pages on high-authority domains. That is a digital-PR and community problem, not a technical SEO problem. The Ahrefs finding that Reddit is massively *retrieved* but almost never *cited* (1.93%) is consistent with this: Reddit shapes what the model believes, then the model links somewhere citable.

**[CONTESTED/SPECULATIVE]** Nobody outside the labs can measure a brand's parametric presence directly. All "share of model" and "AI visibility score" products measure *outputs* (mention rate across a prompt panel), not the underlying weights. Treat those scores as a KPI, not a diagnosis.

---

## 5. Shopping, product feeds and local services

### ChatGPT Shopping and the Agentic Commerce Protocol
**[CONFIRMED]** OpenAI runs a **push-based product feed** rather than crawling ([developers.openai.com/commerce/specs](https://developers.openai.com/commerce/specs); [agentic-commerce-protocol.com feed spec](https://agentic-commerce-protocol.com/docs/commerce/specs/feed); [chatgpt.com/merchants](https://chatgpt.com/merchants/)):
- Formats: **JSONL (gzip), CSV (gzip), TSV (gzip), Parquet (zstd)**. Delivery by **SFTP** to an OpenAI-provided endpoint — not a Merchant Center-style UI upload.
- Refresh accepted **as often as every 15 minutes**, so price and stock are near real-time.
- Spec defines **79 fields, 19 required** (including conditionals). Security: TLS 1.2+, HTTPS/443, documented privacy and compliance policies before production approval.
- **Instant Checkout** runs on the **Agentic Commerce Protocol (ACP)**, an open standard covering checkout, payments, fraud signals and fulfilment notifications, with PSP integration via **Stripe** ([Lengow spec guide](https://www.lengow.com/get-to-know-more/chatgpt-product-feed/); [iPullRank](https://ipullrank.com/ecommerce-chatgpt-product-feeds)).

**[CONFIRMED]** OpenAI states product results are **organic and unsponsored** — no paid placement, no bidding, no keyword matching — and that merchants are ranked on factors including **availability, price, quality, and whether they are the maker or primary seller** ([OpenAI Help Center: Shopping with ChatGPT Search](https://help.openai.com/en/articles/11128490-shopping-with-chatgpt-search); [Using shopping research in ChatGPT](https://help.openai.com/en/articles/12911370-using-shopping-research-in-chatgpt)).
**[VENDOR]** Practitioners describe a shopping answer as built from three inputs: ChatGPT's own reading of public retail pages, the merchant's ACP feed, and the model's synthesis, filtered by OpenAI product policies; the rich **product card** tends to go to the item that is accurately described in structured data, corroborated by trusted third parties, **and** ranks well for the *fan-out sub-queries* behind the question ([alhena.ai](https://alhena.ai/blog/chatgpt-shopping-optimization/); [Precis research](https://www.precis.com/resources/how-chatgpt-shops-what-we-found-why-it-matters-and-what-to-do-about-it)).
Note that "shopping" is one of the vertical indexes reportedly inside Labrador — consistent with feeds being a distinct retrieval path from general web search.

### Google: Merchant Center, Shopping Graph, UCP
**[CONFIRMED]** The **Shopping Graph** is the grounding layer for AI Mode, AI Overviews *and* Gemini product answers — over **50 billion product listings**, with **2 billion updated hourly**, populated primarily by your **Merchant Center** feed ([Google Shopping Graph explainers: FeedOps](https://feedops.com/google-shopping-graph-explained/); [Appear Online](https://www.appearonline.co.uk/blog/google-shopping-graph-explained); [eevy.ai](https://eevy.ai/blog/google-merchant-center-ai-shopping)).
**[VENDOR]** AI Mode is reported to prefer live data over batch feeds — "a nightly feed leaves you wrong all day" — with real-time price and inventory accuracy treated as a ranking input ([Channable](https://www.channable.com/blog/google-shopping-ai-mode); [paz.ai Merchant Center for AI Mode](https://www.paz.ai/guides/google-merchant-center-for-ai-mode)). **AI Max for Shopping** went to broader release **30 April 2026**.
**[CONFIRMED]** The **Universal Commerce Protocol (UCP)** launched at **NRF, January 2026** — Google's open agentic-commerce standard, co-developed with merchants including Target — providing the conversational and checkout layer so AI Mode can coordinate cart, payment, order and post-purchase with merchant systems ([blog.google: Universal Cart and agentic shopping](https://blog.google/products-and-platforms/products/shopping/google-shopping-cart/); [The Register, 12 Jan 2026](https://www.theregister.com/2026/01/12/google_gemini_agentic_ai_shopping_protocol/); [Target fact sheet, Jan 2026](https://corporate.target.com/press/fact-sheet/2026/01/google-gemini-2026)). Agentic checkout ("buy for me", triggered by price tracking, always with explicit shopper confirmation) is live with selected US merchants; **Business Agent** (Feb 2026) lets brands run an AI sales associate inside AI Mode and the Gemini app.
**Note:** ACP (OpenAI/Stripe) and UCP (Google) are **competing standards**. Mid-size merchants will likely need both.

### Perplexity Shopping
**[CONFIRMED]** **"Buy with Pro"** launched **November 2024**: one-click checkout inside Perplexity for Pro subscribers, **PayPal/Venmo** handling the transaction, **merchant remains merchant of record** and keeps fulfilment and the customer relationship ([stellagent.ai](https://stellagent.ai/insights/perplexity-shopping-buy-with-pro); [1digitalagency](https://www.1digitalagency.com/perplexity-shopping-optimization/)).
**[CONFIRMED]** The **Merchant Program** has **no application fee, no ongoing cost and no commission** — funded from Pro subscription revenue. Requirements: sell and ship to the US. Feed is submitted in **Google Shopping CSV format via SFTP**, which is explicitly preferable to letting the crawler guess from product pages. Open to **all Shopify merchants since January 2026** ([alhena.ai](https://alhena.ai/blog/perplexity-shopping-merchants-setup-guide/); [Structora Shopify guide](https://structora.co/blog/shopify-perplexity-shopping/); [verityscore.io](https://verityscore.io/en/kb/perplexity-shopping/)).
**Practical note:** because Perplexity accepts the *same Google Shopping CSV* you already generate for Merchant Center, this is the cheapest incremental AI commerce channel available.

### Local services — "best plumber near me"
**ChatGPT [VENDOR/STUDY, plausible but single-sourced on the percentages]:** local queries trigger a tool call to external place APIs rather than a general web search. **Foursquare Places** is an official OpenAI partner (100M+ POIs, 200+ countries) and is reported to supply **over 70%** of local business results in ChatGPT; **Yelp Fusion** supplies listings, reviews, ratings and categories and appears as a cited source in **~33%** of local AI results; Bing Places feeds the web-search layer ([surfacelocal.com](https://www.surfacelocal.com/blog/how-chatgpt-finds-local-businesses); [bizinabox](https://bizinabox.one/blog/how-chatgpt-decides-which-local-business-to-recommend-the-answer-will-surprise-y); [gmbapi.com](https://gmbapi.com/news/how-to-make-llms-recommend-your-business/)). The reported "70%" and "33%" figures come from SEO vendors, not OpenAI — treat as directional. The existence of a **local listings vertical index** inside Labrador is consistent with a dedicated local path.
**Google AI Mode [VENDOR/STUDY]:** Gemini and AI Overviews pull directly from **Google Business Profile** category tags, service listings, attributes, review text, photos and connected website content ([Whitespark's guide to AI Mode for local](https://whitespark.ca/guides/whitesparks-guide-to-googles-ai-mode-for-local-businesses/); [cheers.tech](https://www.cheers.tech/geo-academy/how-local-businesses-can-show-up-in-google-ai-search)). Reported patterns: AI-generated local results surface only **~32%** as many unique businesses as traditional local packs; AI Overviews appear for **68%** of local searches vs 39% for traditional local packs; reliable citation reportedly begins around **150+ reviews** ([mapranks.com](https://www.mapranks.com/2025/06/26/google-business-profile-ai-guide-2026/); [biziq](https://biziq.com/blog/google-business-profile-statistics/)). **[SPECULATIVE]** The 150-review threshold is a vendor heuristic with no published methodology.
**Cross-cutting:** ~73% of AI-recommended local businesses are reported to have complete, consistent **NAP** data across major directories. **The local playbook is: complete GBP + complete Bing Places + Foursquare/Yelp presence + volume and recency of reviews + a server-rendered site with explicit service-area and service-list text.**

---

## 6. A concrete priority list

1. **Audit bot access.** Log-check `OAI-SearchBot`, `Claude-SearchBot`, `PerplexityBot`, `Googlebot`, `bingbot`. Check your CDN's default AI-bot posture (Cloudflare now blocks by default for new accounts and, since 15 Sep 2026, blocks mixed-use crawlers on ad-bearing pages).
2. **Separate training from retrieval in robots.txt.** Block `GPTBot`/`ClaudeBot`/`Google-Extended` if you must; never block the *-SearchBot* variants.
3. **Get into Brave's index** — it is the direct path to Claude and is far less contested than Google or Bing.
4. **Server-render everything that must be cited.** No JS-dependent prices, stock, reviews, hours or service areas.
5. **Write titles and slugs against fan-out sub-queries**, not head terms. This is the highest-leverage on-page change given that ~79 of 80 retrieved pages are judged on title + URL + 200 chars.
6. **Structure for passage extraction:** answer early, one question per page, numbers over adjectives, quotable sentences, explicit sourcing (per GEO's statistics/quotation/cite-sources findings).
7. **Refresh on a ~13-week cadence** with genuine substantive updates.
8. **Build third-party corroboration** — Trustpilot/G2/Yelp profiles with real review volume, Wikipedia-eligible notability, Reddit and YouTube presence, comparison-page inclusion.
9. **Ship feeds:** OpenAI ACP product feed (SFTP, ≤15 min refresh), Google Merchant Center (near-live price/stock), Perplexity Merchant Program (same Google Shopping CSV, zero commission).
10. **Measure per engine.** 91% of citations appear on only one engine; there is no single "AI visibility." Use Bing Webmaster Tools' AI Performance report (the only first-party data available) plus a prompt-panel tracker.

---

## Sources

**Official / vendor documentation**
- OpenAI, Overview of OpenAI Crawlers — https://developers.openai.com/api/docs/bots
- OpenAI, Product feeds – Agentic Commerce — https://developers.openai.com/commerce/specs
- Agentic Commerce Protocol, Product Feed Specification — https://agentic-commerce-protocol.com/docs/commerce/specs/feed
- OpenAI Help Center, Shopping with ChatGPT Search — https://help.openai.com/en/articles/11128490-shopping-with-chatgpt-search
- OpenAI Help Center, Using shopping research in ChatGPT — https://help.openai.com/en/articles/12911370-using-shopping-research-in-chatgpt
- OpenAI, Power product discovery in ChatGPT (merchants) — https://chatgpt.com/merchants/
- Anthropic, Web search tool (Claude Platform Docs) — https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool
- Anthropic, Introducing web search on the Anthropic API — https://claude.com/blog/web-search-api
- Perplexity Help Center, How does Perplexity follow robots.txt? — https://www.perplexity.ai/help-center/en/articles/10354969-how-does-perplexity-follow-robots-txt
- Perplexity, Crawlers documentation — https://docs.perplexity.ai/docs/resources/perplexity-crawlers
- Bing Webmaster Blog (Feb 2026), Introducing AI Performance in Bing Webmaster Tools — https://blogs.bing.com/webmaster/February-2026/Introducing-AI-Performance-in-Bing-Webmaster-Tools-Public-Preview
- Google blog, Universal Cart & agentic shopping — https://blog.google/products-and-platforms/products/shopping/google-shopping-cart/
- Google blog, Agentic checkout and holiday AI shopping tools — https://blog.google/products-and-platforms/products/shopping/agentic-checkout-holiday-ai-shopping/
- Target corporate fact sheet (Jan 2026), Google Gemini checkout — https://corporate.target.com/press/fact-sheet/2026/01/google-gemini-2026
- Cloudflare, Introducing pay per crawl — https://blog.cloudflare.com/introducing-pay-per-crawl/
- Cloudflare, Your site, your rules: new AI traffic options — https://blog.cloudflare.com/content-independence-day-ai-options/
- Cloudflare, Content Independence Day — https://blog.cloudflare.com/content-independence-day-no-ai-crawl-without-compensation/

**Academic**
- Aggarwal et al., *GEO: Generative Engine Optimization*, arXiv 2311.09735 / KDD 2024 — https://arxiv.org/abs/2311.09735 · https://arxiv.org/pdf/2311.09735 · https://collaborate.princeton.edu/en/publications/geo-generative-engine-optimization/
- Fischman, *Does Schema Markup Predict AI Citation?* SSRN 6284518 — https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6284518
- Blck Alpaca, critique of the Princeton GEO methodology — https://blckalpaca.at/en/knowledge-base/seo-geo/geo-generative-engine-optimization/the-princeton-geo-study-methodology-results-and-critique

**Large-scale studies and measurement**
- Ahrefs, Why ChatGPT Cites One Page Over Another (1.4M prompts) — https://ahrefs.com/blog/why-chatgpt-cites-pages/
- Ahrefs, Only 12% of AI Cited URLs Rank in Google's Top 10 — https://ahrefs.com/blog/ai-search-overlap/
- RESONEO (Aug 2026), What ChatGPT pulls, what it shows, what it cites — https://think.resoneo.com/chatgpt-retrieval/
- RESONEO, ChatGPT experiments tracker — https://think.resoneo.com/chatgpt-experiments/
- Search Engine Land (Aug 2026), Inside ChatGPT's retrieval stack — https://searchengineland.com/chatgpt-retrieval-stack-index-cache-pages-485036
- Peec AI, ChatGPT built its own search index — https://peec.ai/blog/chatgpt-built-its-own-search-index
- Vercel + MERJ, The rise of the AI crawler — https://vercel.com/blog/the-rise-of-the-ai-crawler
- SearchOptimo (2026), Do AI crawlers render JavaScript? — https://searchoptimo.com/blog/do-ai-crawlers-render-javascript
- Profound, AI Platform Citation Patterns — https://www.tryprofound.com/blog/ai-platform-citation-patterns
- Profound, What is Claude web search, explained — https://www.tryprofound.com/blog/what-is-claude-web-search-explained
- 5WPR, The State of AI Citations 2026 — https://www.5wpr.com/research/state-of-ai-citations-2026/
- 5W / PRNewswire, AI Platform Citation Source Index 2026 — https://www.prnewswire.com/news-releases/5w-releases-ai-platform-citation-source-index-2026-the-50-websites-that-now-decide-what-brands-are-visible-inside-chatgpt-claude-perplexity-gemini-and-google-ai-overviews-302759804.html
- 5W / PRNewswire, Top-ranking vs AI-cited overlap collapse — https://www.prnewswire.com/news-releases/new-5w-research-overlap-between-top-google-rankings-and-ai-cited-sources-has-collapsed-from-70-to-under-20-302760132.html
- Semrush, How Google's AI Mode Compares to Traditional Search and Other LLMs — https://www.semrush.com/blog/ai-mode-comparison-study/
- Semrush, The Most-Cited Domains in AI: A 3-Month Study — https://www.semrush.com/blog/most-cited-domains-ai/
- Semrush, What is query fan-out? — https://www.semrush.com/blog/query-fan-out/
- Seer Interactive, What Drives Brand Mentions in AI Answers? — https://www.seerinteractive.com/insights/what-drives-brand-mentions-in-ai-answers
- Seer Interactive, Study of 800K AI Responses: How Reviews Shape Brand Presence — https://www.seerinteractive.com/insights/study-of-800k-ai-responses-how-reviews-shape-brand-presence-in-ai-search
- Seer Interactive, AI Brand Visibility and Content Recency — https://www.seerinteractive.com/insights/study-ai-brand-visibility-and-content-recency
- Kevin Indig, The science of how AI picks its sources — https://www.growth-memo.com/p/the-science-of-how-ai-picks-its-sources
- Kevin Indig, State of AI Search Optimization 2026 — https://www.growth-memo.com/p/state-of-ai-search-optimization-2026
- Kevin Indig, AI Halftime Report H1 2026 — https://www.growth-memo.com/p/ai-halftime-report-h1-2026
- iPullRank, How RAG is Redefining SEO — https://ipullrank.com/how-retrieval-augmented-generation-is-redefining-seo
- iPullRank, How AI search platforms expand queries with fan-out — https://ipullrank.com/expanding-queries-with-fanout
- iPullRank, Quick Tip: How OpenAI's Product Feed Redefines Commerce Data — https://ipullrank.com/ecommerce-chatgpt-product-feeds
- Search Engine Journal, Query Fan-Out Technique in AI Mode — https://www.searchenginejournal.com/query-fan-out-technique-in-ai-mode-new-details-from-google/552532/
- Search Engine Journal, ChatGPT Often Retrieves But Rarely Cites Reddit Pages — https://www.searchenginejournal.com/chatgpt-often-retrieves-but-rarely-cites-reddit-pages-data-shows/572243/
- Search Engine Journal, Google AI Overview Citations From Top-Ranking Pages Drop Sharply — https://www.searchenginejournal.com/google-ai-overview-citations-from-top-ranking-pages-drop-sharply/568637/
- Search Engine Roundtable, Google Says No AI System Currently Uses LLMs.txt — https://www.seroundtable.com/google-ai-llms-txt-39607.html
- Search Engine Roundtable, OpenAI's ChatGPT Has A Web Cache — https://www.seroundtable.com/openai-chatgpt-web-cache-41312.html
- Search Engine Roundtable, Anthropic Updates Its Crawler Documentation — https://www.seroundtable.com/anthropic-updates-its-crawler-docs-40978.html
- GeoJacker, llms.txt: What the 2026 data actually shows — https://geojacker.com/llms-txt
- Digital Applied, llms.txt in practice: adoption data and evidence — https://www.digitalapplied.com/blog/llms-txt-in-practice-adoption-evidence-2026
- Salespeak, 50% of AI citations are under 13 weeks old — https://salespeak.ai/aeo-news/content-freshness-ai-search/
- Growganic, Content freshness and AI citations — https://growganic.io/blog/content-freshness-and-ai-citations
- MarketerFirst, What AI crawlers can read on retail product pages — https://marketerfirst.com/hub/what-ai-crawlers-can-read-on-product-pages/
- Precis, How ChatGPT shops — https://www.precis.com/resources/how-chatgpt-shops-what-we-found-why-it-matters-and-what-to-do-about-it
- Lily Ray, What we can learn from evolving ChatGPT fan-out queries — https://lilyraynyc.substack.com/p/what-we-can-learn-from-evolving-chatgpt

**Reporting and secondary analysis**
- Search Engine Land, Anthropic clarifies how Claude bots crawl sites — https://searchengineland.com/anthropic-claude-bots-470171
- Search Engine Journal, Anthropic's Claude Bots Make Robots.txt Decisions More Granular — https://www.searchenginejournal.com/anthropics-claude-bots-make-robots-txt-decisions-more-granular/568253/
- TechCrunch (21 Mar 2025), Anthropic appears to be using Brave to power web searches — https://techcrunch.com/2025/03/21/anthropic-appears-to-be-using-brave-to-power-web-searches-for-its-claude-chatbot/
- TechCrunch (1 Jul 2026), Cloudflare's new policy pushes AI companies to pay for publishers' content — https://techcrunch.com/2026/07/01/cloudflares-new-policy-pushes-ai-companies-to-pay-for-publishers-content/
- The Register (12 Jan 2026), Google rolls out agentic commerce in Search and Gemini — https://www.theregister.com/2026/01/12/google_gemini_agentic_ai_shopping_protocol/
- 51Degrees (2026), Perplexity's robots and crawlers research — https://51degrees.com/blog/perplexity-ai-2026
- The AI Engineer, How Perplexity built their search engine — https://theaiengineer.substack.com/p/how-perplexity-built-their-search
- eSEOspace, How Perplexity indexing works (2026) — https://eseospace.com/blog/how-perplexity-indexing-works-2026/
- Menra, Google AI Overviews crawler guide — https://www.menra.ai/guides/ai-overviews-crawler-guide
- Menra, Gemini crawler guide — https://www.menra.ai/guides/gemini-crawler-guide
- AI Crawler Check, Google-Extended vs Googlebot (2026) — https://aicrawlercheck.com/blog/google-extended-vs-googlebot
- Anagram, GPTBot explained (2026) — https://www.anagram.ai/blog/gptbot-explained-how-chatgpt-crawls-sees-and-cites-your-site-in-2026
- Anagram, AI crawlers explained: GPTBot, ClaudeBot, PerplexityBot (2026) — https://www.anagram.ai/blog/ai-crawlers-explained-gptbot-claudebot-perplexitybot-and-how-to-let-them-in-2026
- xSeek Docs, OpenAI / Claude / Perplexity user agents — https://www.xseek.io/docs/openai-crawlers-and-user-agents · https://www.xseek.io/docs/claude-user-agents · https://www.xseek.io/docs/perplexity-user-agents
- Semrush, Google publishes guide to optimizing for generative AI search — https://www.semrush.com/blog/google-publishes-generative-ai-search-guide/
- Stackmatix, Google Search Central's AI Overviews guidance — https://www.stackmatix.com/blog/google-search-central-ai-overviews-guidance
- Whitespark, Guide to Google AI Mode for local businesses — https://whitespark.ca/guides/whitesparks-guide-to-googles-ai-mode-for-local-businesses/
- SurfaceLocal, How ChatGPT finds local businesses (2026) — https://www.surfacelocal.com/blog/how-chatgpt-finds-local-businesses
- GMBAPI, How to make LLMs recommend your business — https://gmbapi.com/news/how-to-make-llms-recommend-your-business/
- Channable, How Google Shopping AI Mode impacts product feeds — https://www.channable.com/blog/google-shopping-ai-mode
- Paz.ai, Google Merchant Center for AI Mode (2026) — https://www.paz.ai/guides/google-merchant-center-for-ai-mode
- eevy.ai, Google Merchant Center and AI shopping — https://eevy.ai/blog/google-merchant-center-ai-shopping
- FeedOps, Google Shopping Graph explained — https://feedops.com/google-shopping-graph-explained/
- Lengow, ChatGPT product feed specification (2026) — https://www.lengow.com/get-to-know-more/chatgpt-product-feed/
- Alhena, ChatGPT product feed setup / ChatGPT shopping optimization / Perplexity merchant setup — https://alhena.ai/blog/chatgpt-shopping-product-feed-guide/ · https://alhena.ai/blog/chatgpt-shopping-optimization/ · https://alhena.ai/blog/perplexity-shopping-merchants-setup-guide/
- Stellagent, Perplexity Shopping — Buy with Pro — https://stellagent.ai/insights/perplexity-shopping-buy-with-pro
- 1Digital, Perplexity shopping optimization — https://www.1digitalagency.com/perplexity-shopping-optimization/
- Structora, How to get Shopify products into Perplexity Shopping — https://structora.co/blog/shopify-perplexity-shopping/
- RankMax, Microsoft Copilot SEO (2026) — https://www.rankmax.com.au/articles/microsoft-copilot-seo
- Winston Digital, Bing Copilot optimization playbook — https://www.winstondigitalmarketing.com/playbooks/bing-copilot-optimization/
- Lawrence Hitches, How to rank in Microsoft Copilot Search — https://www.lawrencehitches.com/copilot-search-optimization/
- Cloro, Brave Search API vs SERP API: the engine behind Claude's search — https://cloro.dev/blog/brave-search-api-vs-serp-api/
- Cloro, AI grounding by engine — https://cloro.dev/blog/ai-grounding-by-engine/
- Shahab Papoon, Claude web search runs on Brave — https://www.shahabpapoon.com/blog/claude-web-search-runs-on-brave
- Green Flag Digital, AI Overviews vs AI Mode vs Gemini — https://greenflagdigital.com/learning-ai/google-ai-overviews-vs-ai-mode-gemini/
- Wislr, Gemini vs AI Overviews vs AI Mode — https://www.wislr.com/articles/gemini-vs-ai-overviews-vs-ai-mode/
- Singularity Digital, How Gemini and Google AI Mode rank your content — https://singularity.digital/insights/how-gemini-and-google-ai-mode-rank-content/
- Hall, Query fan-out in Google AI Mode — https://usehall.com/guides/query-fan-out-ai-mode
- WordLift, Query fan-out: a data-driven approach — https://wordlift.io/blog/en/query-fan-out-ai-search/
- ROI & Shine, Schema markup for LLM citation: 2026 evidence — https://roiandshine.com/blog-ai/geo-seo/schema-markup-for-llm-citation/
- Loonis, Schema markup for AI search: what actually works in 2026 — https://www.loonis.co/blog/schema-markup-for-ai-search-what-actually-drives-citations-in-2026
- Stackmatix, Structured data for AI search — https://www.stackmatix.com/blog/structured-data-ai-search
- ALM Corp, AI Overview citations drop from top-ranking pages — https://almcorp.com/blog/google-ai-overview-citations-drop-top-ranking-pages-2026/
- Everything-PR, AI Platform Citation Source Index 2026 — https://everything-pr.com/ai-platform-citation-source-index-2026
- Momentic, Top generative AI chatbots & LLMs by market share — https://momenticmarketing.com/blog/top-ai-chatbots
- Presenc, AI search engine market share 2026 — https://presenc.ai/research/ai-search-engine-market-share-2026
- Cicero, Applebot, Siri and AI content visibility (2026) — https://cicero.studio/en/blog/apple-applebot-siri-ai-content-visibility-2026/
- No Hacks, Apple on the agentic web — https://nohacks.co/blog/apple-on-the-agentic-web
- No Hacks, The AI user-agent landscape in 2026 — https://nohacks.co/blog/ai-user-agents-landscape-2026
- MaxAEO, Which search engines power AI answers? — https://maxaeo.ai/blog/which-search-engines-power-ai-answers/
- AI+Automation, Does ChatGPT use Bing or Google? (2026) — https://aiplusautomation.com/blog/chatgpt-bing-or-google
- Am I Cited, Google-Extended glossary — https://www.amicited.com/glossary/google-extended/
