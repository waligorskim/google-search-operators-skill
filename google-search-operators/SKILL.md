---
name: google-search-operators
description: >-
  Transforms web search queries into precise, operator-rich searches that
  actually find what you need instead of generic noise. Use this skill ANY time
  you are about to perform a web search, use WebSearch, brave_web_search, or any
  search tool — especially when researching topics, comparing products/tools,
  finding recent news, debugging errors, or gathering competitive intelligence.
  Also trigger when the user says "search for", "look up", "find info on",
  "research", "google", or when you need web information to answer a question.
  Even if the search seems simple, use this skill — simple searches are where
  the worst queries happen. If you're about to search the web, read this first.
---

# Google Search Operators — Query Construction Skill

You already know how to search. What you don't do well — and this is the whole reason this skill exists — is construct queries that cover the actual search space instead of firing off the first lazy string that comes to mind.

For the full operator reference table, see `references/operators.md`. This file focuses on the thinking process, the gotchas, and the patterns that turn bad searches into great ones.

## Gotchas — Where You Keep Failing

These are the real failure modes, observed repeatedly. Read them before every search session. Add to this list as you discover new ones.

### Gotcha 1: Single-synonym tunnel vision
You search `claude API features` when the user asked about AI tools. The internet doesn't only use the word "claude". You need `(claude|chatgpt|gemini|copilot|grok) API (features|capabilities)`. The OR operator exists — use it aggressively. One broad query with OR groups beats five narrow queries every time.

### Gotcha 2: No date filtering on tech topics
Tech moves fast. A search without `2025..2026` or `after:2025-01-01` returns results from 2019. For any tech/AI/software topic, assume freshness matters and add date filtering unless the user explicitly wants historical info.

### Gotcha 3: Accepting SEO garbage
Your first result will be "Top 10 Best X in 2025" listicle garbage. Exclude it: `-"top 10" -"top 5" -"best of" -listicle`. If you want real human opinions, target `site:reddit.com OR site:news.ycombinator.com`.

### Gotcha 4: Not quoting error messages
User pastes an error. You search `python connection error`. No. Search `"ConnectionResetError: [Errno 104] Connection reset by peer" python (fix|solution|resolved)`. The exact error string in quotes is the most powerful tool you have for debugging searches.

### Gotcha 5: Sequential single-keyword searches
You do `react hooks`, then `react hooks tutorial`, then `react hooks best practices`. Stop. One query: `"react hooks" (tutorial|guide|"best practices"|"deep dive") 2025..2026 -"top 10"`. Plan your query upfront instead of iterating on bad ones.

### Gotcha 6: Ignoring the user's actual need
User asks "how does X compare to Y" and you search `X features`. Read the intent. Comparison needs `(X|Y) (vs|versus|comparison|benchmark|"compared to")`. Troubleshooting needs exact quotes + solution terms. News needs `after:` dates. Match the query shape to the intent.

### Gotcha 7: Forgetting authoritative source targeting
For programming questions, `site:stackoverflow.com OR site:github.com` cuts through noise instantly. For AI/ML, target `site:arxiv.org OR site:huggingface.co`. For official docs, `inurl:docs OR inurl:documentation`. You know which sources are authoritative for each domain — use `site:` to get there directly.

### Gotcha 8: Not expanding beyond the literal words
User says "database" — you should think `(postgres|mysql|sqlite|mongodb|redis|"data store")`. User says "AI" — you should think `(AI|LLM|"machine learning"|"generative AI"|GenAI)`. User says "coding assistant" — you should think `(copilot|cursor|cody|windsurf|aider|"code completion"|"AI IDE")`. The internet uses many words for the same concept. Bridge that gap.

## Instead of This, Do This

Concrete before/after examples. Study these — they demonstrate the exact transformation this skill exists to produce.

### Dates and Freshness

**BAD:** `react server components tutorial`
**GOOD:** `"react server components" (tutorial|guide|"deep dive") after:2025-01-01 -"top 10"`
Why: tech content from 2021 is dangerously outdated. always add date filtering.

**BAD:** `AI news latest`
**GOOD:** `(AI|LLM|"generative AI") (announced|launched|released) after:2026-01-01`
Why: "latest" means nothing to Google. use `after:` with a specific date, or `2025..2026` range.

**BAD:** `python 3.12 new features`
**GOOD:** `"python 3.12" (changelog|"what's new"|"release notes") site:python.org OR site:docs.python.org`
Why: go straight to the source. official docs > random blog posts.

### Documents and File Types

**BAD:** `machine learning salary report`
**GOOD:** `"machine learning" (salary|compensation|"total comp") (report|survey|data) filetype:pdf 2025..2026`
Why: salary data lives in PDF reports from Levels.fyi, Glassdoor, etc. — not in blog posts. `filetype:pdf` digs them out.

**BAD:** `startup pitch deck examples`
**GOOD:** `"pitch deck" (template|example|"series A"|"seed round") filetype:pptx OR filetype:pdf -site:slideshare.com`
Why: actual pitch decks are PDFs and PPTXs. exclude SlideShare (mostly garbage previews).

**BAD:** `company financial data`
**GOOD:** `"company-name" (10-K|"annual report"|financials) filetype:pdf site:sec.gov OR filetype:xlsx`
Why: real financial data is in SEC filings (PDFs) or spreadsheets, not blog summaries.

**BAD:** `research paper on transformer architecture`
**GOOD:** `"transformer" (architecture|"attention mechanism") (paper|arxiv) filetype:pdf site:arxiv.org 2024..2026`
Why: research papers are PDFs on arxiv. searching without `filetype:pdf site:arxiv.org` gives you blog explainers instead of the actual paper.

### Long-Tail and Niche Searches

**BAD:** `kubernetes best practices`
**GOOD:** `(kubernetes|k8s) ("best practices"|"production checklist"|"lessons learned") (site:engineering.*.com|inurl:blog) 2025..2026 -"top 10" -listicle`
Why: "best practices" returns generic content. target engineering blogs, add freshness, exclude listicles.

**BAD:** `how to optimize postgres queries`
**GOOD:** `(postgres|postgresql) "query optimization" (EXPLAIN|"execution plan"|"index scan"|"seq scan") (site:stackoverflow.com|site:github.com|inurl:blog) -"top 10"`
Why: add the actual technical terms (EXPLAIN, execution plan) to get deep content instead of introductory fluff.

**BAD:** `EU AI Act compliance`
**GOOD:** `"EU AI Act" (compliance|requirements|obligations) (startup|SME|"small business") 2025..2026 site:ec.europa.eu OR site:euractiv.com OR filetype:pdf`
Why: target official EU sources and PDF guidance documents. add `startup|SME` to narrow to the relevant audience.

**BAD:** `docker vs podman`
**GOOD:** `(docker|podman|containerd) (vs|versus|comparison|benchmark|migration) 2025..2026 -"top 10" -listicle (site:reddit.com|inurl:blog|site:news.ycombinator.com)`
Why: expand beyond just two tools, target real opinions on reddit/HN, exclude SEO filler.

**BAD:** `GDPR data processing agreement template`
**GOOD:** `"data processing agreement" (template|sample|model|"standard contractual clauses") GDPR filetype:pdf OR filetype:docx -site:pinterest.com`
Why: DPA templates are actual document files. use `filetype:` to find downloadable ones. exclude Pinterest (random pins).

### Error Messages and Debugging

**BAD:** `python memory error`
**GOOD:** `"MemoryError" python (fix|cause|solution|"out of memory") (site:stackoverflow.com|site:github.com) -closed`
Why: always quote the EXACT error, use OR for solution terms, target where developers actually post answers.

**BAD:** `nginx 502 bad gateway`
**GOOD:** `"502 Bad Gateway" nginx (cause|fix|debug|upstream) (site:stackoverflow.com|site:serverfault.com|inurl:docs) -"top 10"`
Why: target sysadmin sources (serverfault), add debugging terms, exclude listicles.

## The Query Construction Process

This isn't a rigid step-by-step — adapt it to the situation. The core idea is: think in dimensions, then combine them with operators.

**Identify the dimensions** of your search. Each dimension is an independent concept with multiple possible terms. For "latest AI coding assistants compared":

| Dimension | Terms |
|---|---|
| Domain | `AI\|LLM\|"generative AI"` |
| Product | `"coding assistant"\|copilot\|cursor\|cody\|windsurf\|aider` |
| Intent | `comparison\|benchmark\|review\|"pros and cons"` |
| Freshness | `2025..2026` |

**Build OR groups** for each dimension, then combine dimensions with AND (implicit in Google — just put them next to each other). This is the single most important technique.

**Add precision filters** based on need: `site:` for authoritative sources, `-term` to exclude junk, `filetype:` for specific formats, `after:/before:` for dates, `"quotes"` for exact phrases.

**Split if over ~32 words.** Google caps around 32 words. If your query is too complex, split into 2-3 targeted queries covering different angles — not 5 lazy ones covering the same ground with slight variations.

## Synonym Expansion — Think Like the Web

This is where you fail hardest. When a user mentions a topic, think about what terms the RESULTS would use, not just what the user said:

| User says | Expand to |
|---|---|
| "AI" | `AI\|LLM\|"machine learning"\|ML\|"generative AI"\|GenAI` |
| "Claude" | `claude\|anthropic\|chatgpt\|openai\|gemini\|copilot\|grok\|mistral` |
| "coding assistant" | `copilot\|cursor\|cody\|windsurf\|aider\|"code completion"\|"AI IDE"` |
| "database" | `postgres\|postgresql\|mysql\|sqlite\|mongodb\|redis\|"data store"` |
| "container" | `docker\|kubernetes\|k8s\|podman\|"container runtime"` |
| "cloud" | `(aws\|"amazon web services")\|(gcp\|"google cloud")\|(azure\|"microsoft azure")` |
| "frontend" | `react\|vue\|svelte\|angular\|nextjs\|nuxt` |
| "CI/CD" | `"CI/CD"\|"github actions"\|jenkins\|"gitlab ci"\|circleci` |
| "monitoring" | `monitoring\|observability\|datadog\|grafana\|prometheus\|"new relic"` |
| "auth" | `authentication\|OAuth\|SSO\|JWT\|SAML\|OIDC` |
| "serverless" | `serverless\|lambda\|"cloud functions"\|"edge functions"\|vercel\|cloudflare` |
| "testing" | `"unit test"\|"integration test"\|e2e\|jest\|vitest\|pytest\|playwright` |

This table is illustrative, not exhaustive. Apply the same thinking to any domain. The habit of expanding one word into 4-8 alternatives via OR groups is what separates a useful search from a useless one.

## Search Patterns — Starter Templates

Adapt these to your situation. They're starting points, not rigid formulas.

```
# Technology comparison
(tech1|tech2|tech3) (comparison|benchmark|"pros and cons"|review) 2025..2026 -"top 10"

# Error debugging
"exact error message" (fix|solution|resolved|workaround) (site:stackoverflow.com|site:github.com)

# API / library docs
"library-name" (documentation|API|reference|guide) (site:official-docs.com|inurl:docs)

# News / announcements
(company1|company2) (announced|launched|released) "product" after:2025-01-01

# Competitive intelligence
(brand1|brand2|brand3) (pricing|features|review) -site:brand1.com 2025..2026

# Real human opinions (not SEO content)
"topic" (site:reddit.com|site:news.ycombinator.com|site:lobste.rs) -removed -deleted

# Academic / research papers
"topic" (paper|study|research) (filetype:pdf|site:arxiv.org|site:scholar.google.com)

# Buried data (information gain — PDFs, presentations, spreadsheets)
"topic" (filetype:pdf|filetype:pptx|filetype:xlsx) (report|whitepaper|survey|data)

# Finding discussions and threads
"topic" (site:reddit.com|site:quora.com|inurl:forum|inurl:community) intitle:"topic"

# Proximity-based (concepts discussed close together)
"concept1" AROUND(5) "concept2" site:authoritative-domain.com
```

## Multi-Query Strategy

For complex research, plan queries as a funnel — don't just fire and hope:

1. **Broad sweep** (1-2 queries): Cover the space with OR groups, get the lay of the land
2. **Targeted drill-down** (1-2 queries): Based on what the broad sweep reveals, go deep on specific angles
3. **Verification** (optional): Cross-check specific claims from authoritative sources using `site:` restricts

Two or three well-constructed queries with operators will cover more ground than ten vague ones. If you find yourself doing more than 3 searches on the same topic, stop and rethink your query construction.

## Before You Search — Quick Gut Check

- Did I use OR groups for synonyms and alternatives?
- Did I add date filtering? (for tech topics, the answer is almost always yes)
- Did I exclude noise patterns? (`-"top 10"`, `-listicle`, etc.)
- Did I use exact quotes for specific phrases or error messages?
- Is the query under ~32 words? If not, split it.
- Am I doing 1-3 smart queries instead of 5+ dumb ones?
- Did I consider `site:` restricting to authoritative sources?

For the full operator reference (syntax, examples, deprecated operators, tool-specific notes), see `references/operators.md`.

