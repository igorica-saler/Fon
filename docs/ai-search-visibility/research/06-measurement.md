# Measuring, Monitoring, Testing and Iterating AI Search Visibility (GEO/AEO)

*Research compiled 2026-09-21. Evidence-grade notes: primary-source domains (developers.google.com, blogs.bing.com, blog.cloudflare.com, docs.perplexity.ai, openai.com, semrush.com, ahrefs.com, arxiv.org) were blocked by this session's network egress proxy, so several figures below are cited to the URL where the claim was found rather than fetched in full. Claims are tagged **[primary]**, **[secondary]** or **[vendor]** accordingly.*

---

## Recommended measurement stack

A pragmatic stack, in priority order. Layers 1–3 cost nothing but engineering time and should be built first; layers 4–5 are optional spend.

| # | Layer | What it answers | Tooling | Cost |
|---|-------|-----------------|---------|------|
| 1 | **Server logs / edge analytics** | Are AI crawlers fetching my pages at all? Is retrieval live (`ChatGPT-User`, `Perplexity-User`) or training-only (`GPTBot`, `ClaudeBot`)? | Cloudflare Logpush / AI Audit, nginx access logs + cron grep, Vercel/Netlify bot analytics | Free–low |
| 2 | **First-party analytics** | Does AI visibility convert? | GA4 custom channel group for AI referrers + the native `AI Assistant` channel (May 2026); BigQuery export for anything serious | Free |
| 3 | **Platform-owned reports** | What do the engines themselves admit? | Google Search Console *Generative AI performance* report (June 2026); Bing Webmaster Tools *AI Performance* + *Citation Share* (Feb/June 2026) | Free |
| 4 | **DIY prompt panel** | Share of voice, citation rate, competitor position — auditable, cheap, engine-diverse | Python + OpenAI Responses `web_search`, Perplexity Sonar, Gemini grounding; store raw JSON in Postgres/DuckDB; 5 runs × prompt × engine × locale, weekly | ~$20–150/mo |
| 5 | **Commercial platform** | Consumer-surface parity, prompt-volume estimates, exec reporting | Peec AI or Otterly at the low end; Profound or Scrunch at enterprise; Semrush/Ahrefs if already a customer | $29–$2,000+/mo |

**The single most important design decision** is that layers 1–3 measure *reality* (real crawls, real clicks, real impressions) while layer 4–5 measure a *sample of a stochastic system*. Never let a vendor's "AI Visibility Score" be the primary KPI when GSC impressions and server logs are available for free.

---

## 1. Metrics, and the sampling problem nobody wants to talk about

### The metric vocabulary that has converged

Across vendors and practitioners, eight metrics recur ([Data-Mania, 2026](https://www.data-mania.com/blog/ai-search-visibility-benchmarks-2026-citation-rates-share-of-voice-b2b-saas/); [MaximusLabs](https://www.maximuslabs.ai/ai-search-101/geo/measurement/geo-measurement-metrics)) **[secondary]**:

- **Presence / mention rate** — % of sampled answers that name the brand at all (no link required). This is the *brand-knowledge* metric; it can be non-zero even when the engine never retrieves your site.
- **Citation rate** — % of answers that link to *your domain* in the sources block. This is the *retrieval* metric and is the one that correlates with referral traffic.
- **Share of Voice (SoV)** — `(brand mentions ÷ total mentions of all tracked brands) × 100` over the prompt set. The near-universal formula ([Shadow](https://www.shadow.inc/resources/how-to-measure-ai-share-of-voice); [Digital Applied](https://www.digitalapplied.com/blog/ai-share-of-voice-tracking-brand-citations-framework-2026)) **[secondary]**. Note SoV is *relative*: it can rise while absolute mentions fall, because a competitor dropped out.
- **Answer position / rank-in-list** — for list-type answers ("best X for Y"), the ordinal position. Materially different from mention rate: being #7 of 8 is near-worthless.
- **Prompt coverage** — % of your prompt inventory where you appear at least once. The inverse, **zero-mention gaps**, is the actionable version and belongs on the weekly dashboard.
- **Sentiment / framing** — how the brand is characterised. Most tools do this with an LLM-as-judge pass over the answer text; treat as directional only.
- **Source mix / citation domain share** — which domains the engine cited instead of you (own site vs. G2/Reddit/listicles). This is the single richest diagnostic field and is why you must store raw answers.
- **AI referral traffic and conversions** — the lagging, cash-relevant metric (§3).

Tool-specific composites (Ahrefs "AI Share of Voice", Semrush "AI Visibility Index", Elmo "Visibility Score", the open-source "SRO Score 0–100") are all weighted blends of the above. They are not comparable across vendors and should never be trended across a tool migration.

### Non-determinism: how many runs are actually needed

Same prompt, same model, different answer — this is not a bug to engineer around, it is the measurement substrate. Causes: probabilistic sampling; and even at temperature 0, floating-point non-associativity under server-side batching ([Zartis](https://www.zartis.com/the-subtle-truth-about-determinism-and-variance-in-llms/)) **[secondary]**. For retrieval-augmented answers a further source dominates: the engine's own query fan-out differs run to run, so the candidate document set differs before the model even writes.

The most useful quantitative work is a 2026 variance-components decomposition of brand answers, *"Where Does the Noise Come From? A Variance-Components Decomposition of Non-Determinism in LLM Brand Answers"* (arXiv 2607.13304) **[primary, abstract-level]**. Reported findings:

- A brand score moves for four separable reasons: **within-prompt resampling, prompt paraphrase, model identity, query language**.
- On the resampled-stability subset, **resampling accounts for 34.8% of variance**; a brand-in-context interaction term accounts for **29.6%**.
- Industry practice has converged on **~5 repeats per prompt, averaged** — but the paper argues this is a poor use of budget: *per unit of query spend, adding languages and models reduces relative-error variance far more than adding repeats*, and the sixth repeat reduces it by only **0.0003**.

**Practical rule.** Budget a fixed number of engine calls per week, then spend them in this order: (1) more *prompts* (coverage), (2) more *engines*, (3) more *locales/languages*, (4) more *repeats* — stopping repeats at about 5. A common failure mode is 200 prompts × 1 run: that produces a metric whose week-over-week noise exceeds any realistic effect size. With p ≈ 0.3 mention rate, the standard error on a 100-observation sample is ≈ 4.6 points, so a "6-point improvement" on 100 observations is not a result.

**Report confidence intervals, not point estimates.** For a binomial mention rate, Wilson intervals on `n = prompts × runs`. Anything a dashboard shows without an interval invites the team to react to noise.

### Personalization and geolocation

Answers vary by: IP-derived locale, account country, logged-in state and persistent memory, A/B feature flags, and (on ChatGPT) plan tier. This is why API-based measurement and consumer-app measurement diverge — see §5. Practically: pin a locale explicitly (`user_location` in the OpenAI web_search tool; `gl`/`hl` equivalents in SERP-scraping vendors), measure each target market as its own panel, and never compare a US panel to a DE panel as though it were a time series.

---

## 2. Commercial tools (2025–2026)

Pricing below is as published/reported in 2026; all **[secondary]** unless noted, since vendor pricing pages were unreachable in this session.

**Entry tier (self-serve, credit-card):**
- **Otterly.ai** — Lite **$29**, Standard **$189**, Premium **$489**/mo, Enterprise from ~$1,000 ([criticnest](https://criticnest.com/ai-visibility-tools-pricing/)). Cheapest credible entry point; tracks prompts across ChatGPT/Perplexity/Google AI surfaces and publishes useful guides on the Bing AI Performance report.
- **Peec AI** — roughly **€89 / €199** (reported elsewhere as $95/$245/$495). Widely described as the best depth-to-price ratio below enterprise ([Surmado](https://www.surmado.com/blog/best-ai-visibility-tools-2026)).
- **Rankscale, Writesonic GEO, HubSpot AI Search Grader, Goodie, Xfunnel, Bluefish, Evertune, AirOps, Athena** — a crowded middle. Several offer free one-shot "grader" scans (HubSpot's grader is the best-known free entry). Treat free graders as lead magnets: they run a handful of prompts once and are useless as a time series.

**Mid/enterprise:**
- **Scrunch AI** — Core from ~**$250/mo**; acquired by **Sitecore in June 2026** ([get-ryze](https://www.get-ryze.ai/blog/ai-visibility-tools-pricing-compared-2026)). Strength is agent-experience auditing (what bots see) alongside visibility.
- **Profound** — Lite **$499/mo**, Enterprise custom, no self-serve above Lite ([Maintouch](https://maintouch.com/blogs/profound-ai-pricing)). The differentiator is **Prompt Volumes / Conversation Explorer**: Profound states it licenses conversations from **double-opt-in consumer panels** of real answer-engine users, anonymized and PII-scrubbed, then applies probabilistic modelling to estimate prompt frequency and intent ([tryprofound.com/features/prompt-volumes](https://www.tryprofound.com/features/prompt-volumes)) **[vendor]**. This is the closest thing to "AI keyword volume" that exists; it is panel-extrapolated, not census data, and should be read like Nielsen ratings, not like Search Console.
- **Conductor, BrightEdge, SE Ranking AI, Similarweb AI traffic** — incumbents that bolted AI visibility onto existing suites. Similarweb is distinctive because it sells *panel-based referral measurement* (what AI traffic competitors receive) rather than prompt sampling.

**Suite add-ons (best value if you already pay):**
- **Semrush AI Visibility Toolkit** — standalone **$99/mo per domain**, including **1 domain and 25 tracked prompts**; +50 prompts ≈ $60/mo; extra domain $99; extra seat $99. Bundled **Semrush One**: Starter $199 (50 prompts/day), Pro+ $299 (100), Advanced $549 (200, + API) ([semrush.com/pricing/ai](https://www.semrush.com/pricing/ai/); [criticnest](https://criticnest.com/semrush-one-pricing)). **25 prompts is far below the sample size §1 says you need** — budget the prompt add-ons or don't bother.
- **Ahrefs Brand Radar** — **$398/mo** (Select Platforms: AI Overviews, AI Mode, ChatGPT, Perplexity, Copilot, Gemini, Grok; **2,500 custom-prompt checks/mo**) or **$699/mo** (All Platforms + a ~414M organic prompt pool). **Requires an active Ahrefs base subscription on top** ($129–$1,499/mo) ([aeolabs](https://www.aeolabs.ai/blog/ahrefs-brand-radar-review); [get-ryze](https://www.get-ryze.ai/blog/ahrefs-brand-radar-pricing-2026)).

**Free / near-free:** Bing Webmaster Tools AI Performance (real citation data, free); GSC Generative AI report (free); Cloudflare AI Audit and the **AEO Visibility Dashboard shipped in early access 6 Aug 2026** monitoring Claude and GPT **[secondary]**; open-source self-hosting (§5).

**Methodology credibility checklist** — ask every vendor, in writing:
1. Do you query the **consumer surface** (scraped) or the **API**? Which models/versions?
2. **How many runs per prompt per period**, and is the reported number a mean or a single draw?
3. Do you store and expose **raw answer text and the full citation list**? (If not, you cannot audit or re-derive anything.)
4. What **locale/logged-in state** is simulated?
5. Is "visibility score" a published formula or a black box?

Vendors that cannot answer (2) and (3) are selling a number, not a measurement.

---

## 3. Analytics: GA4, Search Console, Bing Webmaster Tools

### GA4

On **13 May 2026** Google added a native **`AI Assistant`** channel to GA4's Default Channel Group ([Search Engine Journal](https://www.searchenginejournal.com/google-analytics-adds-ai-assistant-as-default-channel-group/574974/); [GA4 Optimizer](https://www.gaoptimizer.com/blog/ga4-ai-assistant-channel/)) **[secondary]**. Two caveats dominate: the channel is **not retroactive**, and its source list (reported as ChatGPT, Gemini, DeepSeek, Copilot, Grok) **excludes Perplexity**, which continues to land in generic *Referral* ([Terminus](https://www.terminusapp.com/blog/ai-traffic-channel-in-ga4/)) **[secondary]**. A custom channel group therefore remains mandatory.

Admin → Data display → Channel groups → new group → condition `Source matches regex` (GA4 uses **RE2**; escape every dot):

```
^(chatgpt\.com|chat\.openai\.com|openai\.com|perplexity\.ai|www\.perplexity\.ai|gemini\.google\.com|bard\.google\.com|aistudio\.google\.com|copilot\.microsoft\.com|bing\.com/chat|m365\.cloud\.microsoft|claude\.ai|meta\.ai|grok\.com|x\.ai|deepseek\.com|chat\.mistral\.ai|you\.com|poe\.com|phind\.com|kagi\.com|duckduckgo\.com/aichat|andisearch\.com|iask\.ai)$
```

Notes that save pain later:
- ChatGPT appends **`?utm_source=chatgpt.com`** to outbound links, so much ChatGPT traffic arrives as `source=chatgpt.com / medium=referral` even when the referrer header is stripped. Do **not** strip or rewrite it. Conversely, never add your own `utm_source=chatgpt` to links — it corrupts the only reliable signal you have.
- Apps (mobile ChatGPT, Copilot in Windows) frequently send **no referrer**, so a share of AI traffic is permanently misclassified as Direct. Expect your measured AI referral number to be an undercount.
- Build the equivalent in BigQuery for anything analytical:

```sql
SELECT
  CASE WHEN REGEXP_CONTAINS(source, r'(?i)(chatgpt|openai|perplexity|gemini\.google|copilot\.microsoft|claude\.ai|grok|deepseek|mistral|meta\.ai|you\.com|phind)')
       THEN 'AI Assistant' ELSE 'Other' END AS channel,
  COUNT(DISTINCT user_pseudo_id) AS users,
  COUNTIF(event_name='purchase')  AS conversions
FROM (
  SELECT user_pseudo_id, event_name,
    (SELECT value.string_value FROM UNNEST(event_params) WHERE key='source') AS source
  FROM `project.analytics_XXXXXX.events_*`
  WHERE _TABLE_SUFFIX BETWEEN '20260601' AND '20260921')
GROUP BY channel;
```

### Conversion-rate evidence (handle with care)

Reported 2026 findings **[secondary, and every one of them is confounded]**:
- **Semrush (2026)**: AI-referred visitors convert at **4.4×** standard organic ([runmarshal](https://www.runmarshal.com/field-notes/ai-search-traffic-is-4x-more-valuable-than-organic)).
- **Ahrefs**: AI search visitors convert at **23×** organic — **0.5% of traffic drove 12.1% of signups** ([averi](https://www.averi.ai/blog/ai-search-visitors-convert-23x-higher.-everyone-s-ignoring-it.)).
- Platform splits reported at **ChatGPT 15.9%, Perplexity 10.5%, Claude 5.0%** conversion ([kozec](https://kozec.ai/ai-sourced-traffic-conversion-rates-vs-organic-search/)).
- A Rocket Agency 18-month cross-industry study: ChatGPT visits convert at **5.1×** organic.

The mechanism offered ("the assistant pre-qualifies the visitor") is plausible, but these are **self-selection comparisons, not experiments**: AI referrals skew late-funnel and brand-aware, and the denominators differ wildly (0.5% of traffic). Use them to justify *measuring* AI traffic; do not use them to forecast revenue. Volume context: AI referral traffic is still roughly **~1% of total visits** for most sites ([authoritytech](https://authoritytech.io/curated/ai-referral-traffic-brand-citation-measurement-2026)), and in B2B during March–April 2026 the split of measurable AI referrals was **ChatGPT 62.6%, Claude 18.5%, Gemini 10.6%, Perplexity 7.3%** **[secondary]**.

A step change worth knowing: Similarweb attributes a sharp rise in outbound clicks to **ChatGPT's 7 May 2026 update**, which turned brand names in answers into prominent clickable links; homepage referrals from ChatGPT moved from ~26–29% of referral traffic to **~62–63% by late May 2026** and held ([Similarweb](https://www.similarweb.com/blog/insights/ai-news/chatgpt-referral-traffic-triples/)) **[vendor]**. Any year-over-year AI-traffic comparison that straddles May 2026 is comparing two different products.

### Google Search Console

**3 June 2026**: Google launched **Search Generative AI performance reports** in Search Console, covering generative AI features on Search (**AI Overviews and AI Mode**) and in Discover ([Google Search Central](https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports); [help doc](https://support.google.com/webmasters/answer/16984139)) **[primary]**. Rolled out to a subset first, then **to all sites worldwide by 31 August 2026** **[secondary]**.

What you get: **impressions**, broken down by **page, country, device and date**. What you do **not** get: **clicks, CTR, position, or query data** for the AI surfaces **[secondary, consistently reported]**. The corresponding clicks remain folded into the main Performance report's totals with everything else. So GSC tells you *you appeared*, never *which prompt* or *whether anyone clicked*. Before June 2026, AI Mode data was simply merged into the aggregate Web totals with no way to isolate it — which is why 2025-era "AI Overviews cost us X%" analyses were all inference.

### Bing Webmaster Tools

Bing is the most generous of the engines, and it matters disproportionately because ChatGPT's retrieval leans on the Bing index blended with OpenAI's own `OAI-SearchBot` crawl **[secondary]**.

- **9 Feb 2026** — **AI Performance** report enters public preview: citation tracking across **Microsoft Copilot, Bing AI-generated summaries, and select partner AI experiences** ([blogs.bing.com](https://blogs.bing.com/webmaster/February-2026/Introducing-AI-Performance-in-Bing-Webmaster-Tools-Public-Preview)) **[primary]**.
- **16 June 2026** — four additions, free, global preview: **Intents** (informational/commercial/research buckets), **Topics** (thematic clustering), **Citation Share** (your % of citations for a given grounding query) and **Compare** (change over time) ([blogs.bing.com](https://blogs.bing.com/search/June-2026/New-AI-Visibility-Insights-in-Bing-Webmaster-Tools-Intents-Topics-Citation-Share-Compare)) **[primary]**.

**Citation Share is the only first-party, census-level share-of-voice metric that exists for any AI surface.** It does not cover ChatGPT, Perplexity or Google AI Overviews — but as a *leading indicator for ChatGPT*, it is the best free signal available. Also use BWT's URL Inspection to confirm Bing indexation: a page absent from Bing has close to zero chance of a ChatGPT citation.

---

## 4. Server-log and edge analysis

Logs are the only layer that is a census rather than a sample, and the only one that distinguishes **training crawls** from **live retrieval**.

Three functional classes (metadata per the [ai.robots.txt](https://github.com/ai-robots-txt/ai.robots.txt) project, ~180 agents tracked, 4.1k stars, machine-readable `robots.json` + `table-of-bot-metrics.md`) **[primary]**:

| Class | Agents | What a hit means |
|---|---|---|
| **Training / corpus** | `GPTBot`, `ClaudeBot`, `anthropic-ai`, `Google-Extended`, `Applebot-Extended`, `Meta-ExternalAgent`, `Bytespider`, `CCBot`, `Amazonbot`, `MistralAI-Training`, `QwenBot`, `DeepSeekBot` | Your content may enter a future model's parametric memory. No near-term citation signal. |
| **Search index** | `OAI-SearchBot`, `Claude-SearchBot`, `PerplexityBot`, `Applebot`, `Bingbot`, `MistralAI-Index`, `meta-webindexer`, `ExaSearchBot`, `TavilyBot`, `Kimi-SearchBot` | You are eligible to be retrieved. This is the *prerequisite* for citation. |
| **Live user fetch** | `ChatGPT-User`, `ChatGPT Agent`, `Perplexity-User`, `Claude-User`, `Meta-ExternalFetcher`, `MistralAI-User`, `GoogleAgent-URLContext`, `Gemini-Deep-Research`, `Amzn-User`, `Diffbot-User` | **A real human's question caused this fetch, right now.** The highest-signal line in your logs. |

New in 2026 and worth adding to any existing regex: `OAI-AdsBot` (ad validation/targeting in ChatGPT), `ChatGPT Agent`, `Claude-Code`, `Gemini-Deep-Research`, `GoogleAgent-URLContext`, `meta-webindexer`, `MistralAI-Index`.

A `ChatGPT-User` or `Perplexity-User` hit on a specific URL is effectively a **zero-latency citation alert**: it tells you which page an assistant chose to open *for a live question*, minutes before any dashboard would show it. Spikes in `*-User` fetches on a page are the earliest experiment read-out you will get (§6).

```bash
# Daily AI-bot breakdown by class and top URL (nginx combined format)
awk -F'"' '{ua=$6; req=$2} 
  ua ~ /ChatGPT-User|ChatGPT Agent|Perplexity-User|Claude-User|MistralAI-User|Meta-ExternalFetcher|GoogleAgent-URLContext/ {print "LIVE\t" $0; next}
  ua ~ /OAI-SearchBot|Claude-SearchBot|PerplexityBot|MistralAI-Index|meta-webindexer|Applebot|ExaSearchBot|TavilyBot/  {print "INDEX\t" $0; next}
  ua ~ /GPTBot|ClaudeBot|anthropic-ai|Google-Extended|Bytespider|CCBot|Amazonbot|Meta-ExternalAgent|Applebot-Extended/ {print "TRAIN\t" $0}' \
  /var/log/nginx/access.log \
| awk -F'\t' '{split($2,a,"\""); print $1}' | sort | uniq -c | sort -rn
```

**Verify before you believe.** User-agent strings are trivially spoofed — anyone can `curl -A ClaudeBot`. Two accepted verification methods:
1. **Published IP ranges.** OpenAI (`openai.com/gptbot.json`, `searchbot.json`, `chatgpt-user.json`), Anthropic, Perplexity and Apple publish CIDR lists; refresh them on a schedule and match request IPs.
2. **Forward-confirmed reverse DNS.** `dig -x <ip>` should resolve into the operator's domain (e.g. OpenAI crawlers resolve under `openaibot.com`), and the hostname must resolve forward to the same IP.

```bash
ip=203.0.113.10
host=$(dig +short -x "$ip" | sed 's/\.$//')
[ -n "$host" ] && [ "$(dig +short "$host" | head -1)" = "$ip" ] && echo "verified: $host" || echo "SPOOFED"
```

**Edge platforms.** Cloudflare's AI Audit / Radar publishes **crawl-to-refer ratios** — HTML crawl requests per referred human visit, per platform. On a 28-day window ending **21 July 2026**: Mistral **3,389:1**, Anthropic **~2,237:1**, OpenAI **~217:1**, Google **~4.6:1**; Anthropic's ratio had been near **23,951:1** across Jan–Mar 2026, **10,300:1** on 31 May and **~4,580:1** in June ([Cloudflare Radar AI Insights](https://blog.cloudflare.com/ai-crawler-traffic-by-purpose-and-industry/); compiled by [SEOmator](https://seomator.com/blog/crawl-to-refer-ratio-ai-crawlers-llm-bots)) **[secondary]**. Compute your *own* ratio from logs + GA4 — it is the cleanest "am I being farmed or promoted?" metric, and its trend per platform is more meaningful than the absolute level. Vercel and Netlify both expose bot/AI-crawler breakdowns in their analytics; Cloudflare's **AEO Visibility Dashboard** (early access, 6 Aug 2026) adds Claude/GPT visibility monitoring at the edge.

---

## 5. DIY monitoring you can build in an afternoon

Three grounded APIs cover most of the market. Costs are per prompt-run, 2026:

| API | Grounding | Citation field | Indicative cost |
|---|---|---|---|
| **OpenAI Responses API**, `tools:[{type:"web_search"}]` | Live web | `output[].content[].annotations[]` of type `url_citation` with `url`, `title`, `start_index`, `end_index` | tokens + per-call web-search fee (varies by search-context size) |
| **Perplexity Sonar** | Always live | `search_results` / `citations` array | `sonar` $1/$1 per M tokens; `sonar-pro` $3/$15 per M; **plus a per-1,000-request search fee of ~$5–$14** depending on context size. In 2026 citation tokens are billed only for Deep Research ([cloudzero](https://www.cloudzero.com/blog/perplexity-api-pricing/)) |
| **Gemini API** with Google Search grounding | Live | `groundingMetadata.groundingChunks[].web.uri/title`, `webSearchQueries`, `groundingSupports` | per-request grounding fee above a free daily allowance |

At 5 runs × 3 engines × 100 prompts = 1,500 calls/week, a realistic all-in bill is **$30–$120/month** — one to two orders of magnitude below enterprise tooling.

```python
# minimal prompt-tracker skeleton — one row per (prompt, engine, run)
import os, json, re, datetime, sqlite3, httpx
from openai import OpenAI

BRAND   = {"acme", "acme.com", "acmecloud"}
RIVALS  = {"globex": {"globex","globex.io"}, "initech": {"initech"}}
PROMPTS = [l.strip() for l in open("prompts.txt") if l.strip()]
RUNS, LOCALE = 5, {"type": "approximate", "country": "US", "city": "Chicago"}

db = sqlite3.connect("geo.db")
db.execute("""CREATE TABLE IF NOT EXISTS obs(ts,engine,prompt,run,answer,citations,
              mentioned INT, cited INT, position INT)""")

def openai_run(prompt):
    r = OpenAI().responses.create(
        model="gpt-5.1",
        tools=[{"type": "web_search", "user_location": LOCALE}],
        input=prompt)
    text, cites = r.output_text, []
    for item in r.output:
        for c in getattr(item, "content", []) or []:
            for a in getattr(c, "annotations", []) or []:
                if a.type == "url_citation":
                    cites.append({"url": a.url, "title": a.title})
    return text, cites

def perplexity_run(prompt):
    r = httpx.post("https://api.perplexity.ai/chat/completions",
        headers={"Authorization": f"Bearer {os.environ['PPLX_KEY']}"},
        json={"model": "sonar-pro",
              "messages": [{"role": "user", "content": prompt}]},
        timeout=120).json()
    ch = r["choices"][0]["message"]
    return ch["content"], [{"url": s["url"], "title": s.get("title")}
                           for s in r.get("search_results", [])]

def score(text, cites, terms):
    low = text.lower()
    mentioned = any(t in low for t in terms)
    cited = any(any(t in c["url"].lower() for t in terms) for c in cites)
    # ordinal position in a list-style answer
    pos = next((i+1 for i, line in enumerate(re.findall(r'^\s*(?:\d+\.|[-*])\s*(.+)$',
                text, re.M)) if any(t in line.lower() for t in terms)), None)
    return mentioned, cited, pos

for engine, fn in (("openai", openai_run), ("perplexity", perplexity_run)):
    for p in PROMPTS:
        for run in range(RUNS):
            text, cites = fn(p)
            m, c, pos = score(text, cites, BRAND)
            db.execute("INSERT INTO obs VALUES (?,?,?,?,?,?,?,?,?)",
                (datetime.datetime.utcnow().isoformat(), engine, p, run, text,
                 json.dumps(cites), int(m), int(c), pos))
db.commit()
```

Share of voice then falls out of SQL:

```sql
SELECT engine,
       AVG(mentioned)*100                        AS mention_rate,
       AVG(cited)*100                            AS citation_rate,
       COUNT(DISTINCT CASE WHEN mentioned=0 THEN prompt END) AS zero_mention_prompts,
       AVG(CASE WHEN position IS NOT NULL THEN position END) AS avg_position
FROM obs WHERE ts >= date('now','-7 day') GROUP BY engine;
```

**Store the raw answer text and the full citation list, always.** Every derived metric can be recomputed later; an answer you did not save is gone. This is exactly the design choice Elmo makes — raw engine output in Postgres "so any metric can be re-derived and audited later" ([elmohq/elmo](https://github.com/elmohq/elmo)).

### Open-source projects worth reading or forking

- **[elmohq/elmo](https://github.com/elmohq/elmo)** — MIT, ~347★, TypeScript/Postgres/pg-boss/Docker Compose. Tracks ChatGPT, Claude, Perplexity, Gemini, Copilot, Grok, Mistral, Google AI Mode and AI Overviews; computes visibility score, SoV, citation-domain categorisation, and **query fan-out analysis** (the sub-searches the engine actually ran). Runs each prompt **multiple times daily**, uses both scraping providers (consumer surfaces) and direct APIs. The clearest reference implementation of the whole pipeline.
- **[Auriti-Labs/geo-optimizer-skill](https://github.com/Auriti-Labs/geo-optimizer-skill)** — ~870★, Python CLI + MCP server. `geo audit` (0–100 across robots.txt/llms.txt/schema/meta/content/entity/signals/discovery), `geo citations` (Perplexity Sonar ≈ $0.02/query; Google AI Overviews via Serpbase at $0.30/1,000 after 100 free), `geo drift`, `geo track`, plus CI gating with `--fail-on warning`.
- **[danishashko/geo-aeo-tracker](https://github.com/danishashko/geo-aeo-tracker)** — MIT, ~270★, Next.js, local-first (IndexedDB), six models via Bright Data's LLM scrapers, bring-your-own-keys.
- **[ai-search-guru/getcito…](https://github.com/ai-search-guru/getcito-worlds-first-open-source-aio-aeo-or-geo-tool)** — ~418★, Next.js visibility tracker.
- **[amplifying-ai/awesome-generative-engine-optimization](https://github.com/amplifying-ai/awesome-generative-engine-optimization)** — ~513★ curated index of GEO tools and research.
- **[ai-robots-txt/ai.robots.txt](https://github.com/ai-robots-txt/ai.robots.txt)** — the canonical machine-readable crawler registry; wire `robots.json` straight into your log parser so the UA list maintains itself.

### The API-vs-app caveat (state it on every dashboard)

API results are **not** what a consumer sees. Differences: model version and routing (consumer apps A/B new models first), system prompts, memory and personalization, tool-use policy (when the app decides to search at all), ad/commerce placements, and geo/logged-in state. Consumer-surface scraping (what Profound, Peec and Elmo's scraping providers do) is closer to truth but is fragile, sometimes against ToS, and rate-limited. **Recommended posture:** use APIs for the *high-frequency, cheap, statistically powered trend line*, and a small scraped or manual panel (20–30 prompts, monthly) as a calibration check on whether the API proxy still tracks the app.

---

## 6. Experimentation: making changes and reading the result

**A controlled GEO test needs a control group.** The workable design is a **matched-page holdout**: split a set of comparable pages into treatment and control, apply the change to treatment only, track citation rate for both on the same prompt set, and report the *difference-in-differences*. Without a control, seasonality and engine-side model updates will be attributed to your edit.

**Time-to-effect (all [secondary]; treat as order-of-magnitude, not precision):**
- Crawl/index of a newly published, well-structured page: **24–72 hours**.
- Perplexity: first citation changes **~4–7 days** after structured-data changes; citations appearing **7–14 days** after publication.
- ChatGPT Search: **~10–14 days** for structured-data effects; **14–30 days** for new pages.
- Google AI Overviews / AI Mode: slowest, **4–8 weeks**, because it inherits classic index and quality lag.
- One 13-week study (240 pages, 200 prompts, five categories) reported an **11% citation lift concentrated on Claude and Perplexity, with no measurable change on ChatGPT Search or Google AI Overviews** ([Security Boulevard, June 2026](https://securityboulevard.com/2026/06/the-geo-measurement-study-50000-ai-citations-in-90-days-what-actually-moves-citation-share/)).

That last finding is the strategically important one: **engines respond to GEO changes at very different rates and magnitudes**, so a single blended "AI visibility score" will systematically wash out real per-engine effects. Always report per engine.

**Fast diagnostic probes (run these before any long experiment):**
1. **Retrievability check** — `"Summarize https://example.com/page"` in each assistant. If it cannot fetch or summarises wrongly, nothing downstream matters. Confirm in logs that the matching `ChatGPT-User` / `Perplexity-User` / `Claude-User` hit arrived and returned 200 (not 403 from a bot-blocking WAF — a very common self-inflicted wound).
2. **Bing index check** — BWT URL Inspection, and `site:` / `url:` in Bing. No Bing index ⇒ effectively no ChatGPT Search citation.
3. **Google AI Mode presence** — manually check target queries in AI Mode, and cross-read GSC's Generative AI report for impressions on the page.
4. **Render check** — fetch your page with JS disabled. Most AI fetchers do not execute JavaScript reliably; if the content only exists after hydration, it does not exist.

**Instrument the test end to end:** log-level `*-User` fetch counts (hours) → Bing Citation Share (days) → your prompt-panel citation rate (1–2 weeks) → GSC AI impressions (2–6 weeks) → GA4 AI referral sessions and conversions (4–12 weeks). Each stage is a checkpoint; if stage 1 never moves, stop and fix access rather than waiting twelve weeks for a null result.

---

## 7. Competitive intelligence and prompt-inventory building

**Your prompt inventory is the measurement instrument.** If it is unrepresentative, every number downstream is unrepresentative. Build it from, roughly in order of value:

1. **Search Console queries** — export 3–12 months, keep question-shaped and commercial-intent queries, then *rewrite them as natural-language prompts* ("best CRM for nonprofits under 50 seats" rather than "nonprofit crm").
2. **Customer support / sales-call transcripts** — the highest-fidelity source of how buyers actually phrase problems, and the one competitors do not have.
3. **People Also Ask and related-questions scrapes** for your head terms.
4. **Reddit / Stack Overflow / niche forum question titles** — these also happen to be heavily cited *sources* in AI answers, so they double as a target list.
5. **Vendor prompt-volume estimates** — Profound Prompt Volumes, Ahrefs' organic prompt pool, Semrush. Panel-extrapolated, not census; use for *ranking* prompts by importance, not for absolute forecasts.
6. **The engines' own fan-out** — Elmo's query fan-out analysis and Bing's Topics/Intents reveal the sub-queries engines generate internally. Those sub-queries are often better tracking targets than the user-facing prompt.

Segment the inventory by funnel stage (problem-aware / category / comparison / brand / "alternatives to X") and keep the mix fixed over time, or your trend line will move whenever the mix does.

**Reverse-engineering competitor citations** is the highest-ROI analysis in the whole discipline, and it requires only your stored raw answers:

```sql
-- Which domains win the prompts where we are invisible?
SELECT json_extract(c.value,'$.url') AS url, COUNT(*) AS n
FROM obs, json_each(obs.citations) c
WHERE obs.mentioned = 0 AND obs.ts >= date('now','-30 day')
GROUP BY 1 ORDER BY n DESC LIMIT 50;
```

Cluster the result by domain type. The recurring pattern in 2026 is that a large share of citations for commercial prompts resolves to **third-party listicles, review platforms (G2/Capterra), Reddit threads and documentation**, not to vendor homepages. That reframes the work: much of GEO is *earning placement in the sources the engine already trusts* (get into the listicle, answer the Reddit thread, publish the comparison page competitors lack) rather than editing your own homepage. Also check *which specific URL* of a competitor is cited — usually a comparison or "alternatives" page, which is a directly copyable asset.

---

## 8. Reporting: a dashboard spec that survives contact with an exec

**Weekly (operator view), per engine, with confidence intervals:**
- Citation rate and mention rate, with n stated (`prompts × runs`) and a Wilson 95% CI.
- Share of Voice vs. the 3–5 tracked competitors.
- Zero-mention prompt list — *the work queue*, sorted by prompt volume estimate.
- Top 20 cited domains on prompts where you are absent — *the outreach queue*.
- AI bot hits split TRAIN / INDEX / LIVE, plus 4xx/5xx served to AI agents (an access regression should page someone).
- Bing Citation Share trend.

**Monthly (leadership view):**
- GSC Generative AI impressions trend (and share of total impressions).
- GA4 AI Assistant sessions, conversions and revenue; AI share of total conversions.
- Crawl-to-refer ratio by platform.
- Experiment ledger: change shipped → date → predicted metric → observed diff-in-diff.

**Leading vs lagging:**

| Leading (days) | Mid (weeks) | Lagging (months) |
|---|---|---|
| AI bot crawl coverage; `*-User` live fetches; Bing/Google index status; HTTP status served to agents | Bing Citation Share; prompt-panel citation rate and SoV; answer position | GSC AI impressions; GA4 AI sessions, conversions, revenue; brand-search lift |

**What "good" looks like.** Honest answer: **there are no trustworthy cross-industry benchmarks yet.** Figures circulating in 2026 — e.g. "17% of B2B SaaS discovery happens through AI answers, up from 4%", "top SaaS brands earn 8.4× more AI citations than competitors" ([Data-Mania](https://www.data-mania.com/blog/ai-search-visibility-benchmarks-2026-citation-rates-share-of-voice-b2b-saas/)) — come from vendor panels with undisclosed prompt sets and are not comparable to your own. The only benchmark with a defensible denominator is Similarweb's observation that **citation presence in US ChatGPT prompts rose from ~1.6% (June 2025) to ~6.8% (May 2026)** — a statement about the market, not about you.

Use **self-relative and competitor-relative targets instead**: baseline for four weeks, then target (a) zero-mention prompts down X%, (b) SoV vs. named competitor up Y points, (c) citation rate on your top-20 commercial prompts above the leading competitor's. Those are measurable, falsifiable, and immune to vendor benchmark inflation.

---

## Sources

- Google Search Central, "Introducing Search Generative AI performance reports in Search Console", 3 June 2026 — https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports
- Search Console Help, "Generative AI performance report (Search)" — https://support.google.com/webmasters/answer/16984139
- Bing Blogs, "Introducing AI Performance in Bing Webmaster Tools Public Preview", 9 Feb 2026 — https://blogs.bing.com/webmaster/February-2026/Introducing-AI-Performance-in-Bing-Webmaster-Tools-Public-Preview
- Bing Blogs, "New AI Visibility Insights in Bing Webmaster Tools: Intents, Topics, Citation Share, Compare", 16 June 2026 — https://blogs.bing.com/search/June-2026/New-AI-Visibility-Insights-in-Bing-Webmaster-Tools-Intents-Topics-Citation-Share-Compare
- Search Engine Journal, "Google Analytics Adds AI Assistant As Default Channel Group", May 2026 — https://www.searchenginejournal.com/google-analytics-adds-ai-assistant-as-default-channel-group/574974/
- GA4 Optimizer, "GA4 AI Assistant Channel: How to Track Chatbot Traffic", 2026 — https://www.gaoptimizer.com/blog/ga4-ai-assistant-channel/
- Terminus, "The 'AI Traffic' Channel in GA4: How to Define It Yourself in 2026" — https://www.terminusapp.com/blog/ai-traffic-channel-in-ga4/
- Swydo, "The Agency Guide to Tracking AI Traffic in GA4 — Setup, Regex Patterns" — https://www.swydo.com/blog/track-ai-traffic-in-ga4/
- arXiv 2607.13304, "Where Does the Noise Come From? A Variance-Components Decomposition of Non-Determinism in LLM Brand Answers", 2026 — https://arxiv.org/html/2607.13304
- Zartis, "The Subtle Truth About Determinism and Variance in LLMs" — https://www.zartis.com/the-subtle-truth-about-determinism-and-variance-in-llms/
- Cloudflare Blog, "A deeper look at AI crawlers: breaking down traffic by purpose and industry", 2026 — https://blog.cloudflare.com/ai-crawler-traffic-by-purpose-and-industry/
- SEOmator, "GEO Data Report 2026: crawl-to-refer ratios" (28-day window ending 21 July 2026) — https://seomator.com/blog/crawl-to-refer-ratio-ai-crawlers-llm-bots
- ai-robots-txt/ai.robots.txt — crawler registry, `robots.json`, `table-of-bot-metrics.md` — https://github.com/ai-robots-txt/ai.robots.txt
- Search Engine Journal, "Complete Crawler List For AI User-Agents", Dec 2025 — https://www.searchenginejournal.com/ai-crawler-user-agents-list/558130/
- elmohq/elmo — open-source AEO/GEO platform (MIT) — https://github.com/elmohq/elmo
- Auriti-Labs/geo-optimizer-skill — open-source GEO/AEO CLI + MCP — https://github.com/Auriti-Labs/geo-optimizer-skill
- danishashko/geo-aeo-tracker — local-first AI visibility dashboard (MIT) — https://github.com/danishashko/geo-aeo-tracker
- ai-search-guru/getcito — open-source AI visibility tracker — https://github.com/ai-search-guru/getcito-worlds-first-open-source-aio-aeo-or-geo-tool
- amplifying-ai/awesome-generative-engine-optimization — curated GEO resources — https://github.com/amplifying-ai/awesome-generative-engine-optimization
- OpenAI, "Web search" (Responses API, `url_citation` annotations) — https://developers.openai.com/api/docs/guides/tools-web-search
- OpenAI Cookbook, "Web Search and States with Responses API" — https://cookbook.openai.com/examples/responses_api/responses_example
- CloudZero, "Perplexity API Pricing In 2026: Models, Costs, And Optimization Tips" — https://www.cloudzero.com/blog/perplexity-api-pricing/
- Semrush, AI Visibility Toolkit pricing — https://www.semrush.com/pricing/ai/
- CriticNest, "Semrush One Pricing 2026" — https://criticnest.com/semrush-one-pricing
- CriticNest, "AI Visibility Tool Pricing 2026: Otterly, Semrush, Profound, Peec" — https://criticnest.com/ai-visibility-tools-pricing/
- Get-Ryze, "AI Visibility Tool Pricing Compared (2026): Every Published Price" — https://www.get-ryze.ai/blog/ai-visibility-tools-pricing-compared-2026
- AEO Labs, "Ahrefs Brand Radar Review (2026): Pricing, Features, and the Real Cost of Full Coverage" — https://www.aeolabs.ai/blog/ahrefs-brand-radar-review
- Get-Ryze, "Ahrefs Brand Radar Pricing in 2026" — https://www.get-ryze.ai/blog/ahrefs-brand-radar-pricing-2026
- Profound, "Track Prompt & Keyword Volume Across AI Conversations" — https://www.tryprofound.com/features/prompt-volumes
- Maintouch, "Profound Pricing Review September 2026" — https://maintouch.com/blogs/profound-ai-pricing
- Surmado, "Best AI Visibility Tools 2026: Profound vs Peec vs Otterly vs the Rest" — https://www.surmado.com/blog/best-ai-visibility-tools-2026
- Similarweb, "ChatGPT Referral Traffic Near Triples Overnight" (May 2026 link update) — https://www.similarweb.com/blog/insights/ai-news/chatgpt-referral-traffic-triples/
- Similarweb, "AI Referral Traffic by Industry: 2026 Data" — https://aisearch.similarweb.com/blog/ai-referral-traffic-by-industry/
- Similarweb, "AI Search Stats in 2026" — https://www.similarweb.com/blog/marketing/geo/gen-ai-stats/
- Averi, "AI Search Visitors Convert 23x Higher" (Ahrefs data) — https://www.averi.ai/blog/ai-search-visitors-convert-23x-higher.-everyone-s-ignoring-it.
- RunMarshal, "AI Search Traffic Is 4x More Valuable Than Organic" (Semrush 2026 data) — https://www.runmarshal.com/field-notes/ai-search-traffic-is-4x-more-valuable-than-organic
- Kozec, "AI Traffic vs Organic Search Conversion Rates: 2026 Data" — https://kozec.ai/ai-sourced-traffic-conversion-rates-vs-organic-search/
- AuthorityTech, "AI Referral Traffic Is 1 Percent of Total Visits" — https://authoritytech.io/curated/ai-referral-traffic-brand-citation-measurement-2026
- Security Boulevard, "The GEO Measurement Study: 50,000 AI Citations in 90 Days", June 2026 — https://securityboulevard.com/2026/06/the-geo-measurement-study-50000-ai-citations-in-90-days-what-actually-moves-citation-share/
- ZipTie.dev, "How Different AI Platforms Cite the Same Source Differently" — https://ziptie.dev/blog/how-different-ai-platforms-cite-the-same-source-differently/
- Data-Mania, "AI Search Visibility Benchmarks 2026: Citation Rates & Share of Voice for B2B SaaS" — https://www.data-mania.com/blog/ai-search-visibility-benchmarks-2026-citation-rates-share-of-voice-b2b-saas/
- Shadow, "How to Measure AI Share of Voice: Methods, Tools, and Benchmarks (2026)" — https://www.shadow.inc/resources/how-to-measure-ai-share-of-voice
- MaximusLabs, "GEO Measurement Metrics: 7 KPIs That Prove AI Visibility" — https://www.maximuslabs.ai/ai-search-101/geo/measurement/geo-measurement-metrics
- Stackmatix, "Bing Webmaster Tools for ChatGPT Optimization: Complete Guide (2026)" — https://www.stackmatix.com/blog/bing-webmaster-tools-chatgpt
- Otterly.ai, "Bing Webmaster Tools AI Performance Report" — https://otterly.ai/blog/bing-webmaster-tools-ai-performance-report/
