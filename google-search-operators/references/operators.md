# Google Search Operators — Full Reference

## Essential Operators (use these constantly)

| Operator | Syntax | What it does | Example |
|---|---|---|---|
| **OR / pipe** | `term1 OR term2` or `term1\|term2` | Matches either term — use to cover synonyms, competitors, alternative names | `(typescript\|javascript) runtime (bun\|deno\|node)` |
| **Exact match** | `"exact phrase"` | Forces Google to match the exact phrase, not reorder or substitute words | `"connection reset by peer" python` |
| **Exclude** | `-term` | Removes results containing this term — use to filter junk, ads, irrelevant domains | `python web framework -django -flask -"top 10"` |
| **Grouping** | `(a OR b)` | Groups OR terms — essential for building complex queries with multiple dimensions | `(aws\|gcp\|azure) (kubernetes\|k8s) "cost optimization"` |
| **Site restrict** | `site:domain.com` | Only results from this domain | `site:github.com "machine learning" python` |
| **Wildcard** | `*` | Placeholder for unknown words within exact phrases | `"the * of software engineering"` |
| **Number range** | `num1..num2` | Matches number ranges — great for years, prices, versions | `react tutorial 2025..2026` |

## Power Operators (use when you need precision)

| Operator | Syntax | What it does | Example |
|---|---|---|---|
| **Title search** | `intitle:term` | Term must appear in page title — filters for focused content | `intitle:"migration guide" postgres` |
| **All in title** | `allintitle:words` | ALL terms must be in the title | `allintitle:react hooks testing` |
| **URL search** | `inurl:term` | Term must appear in the URL — good for finding docs, API refs | `inurl:api-reference openai` |
| **All in URL** | `allinurl:words` | ALL terms must be in URL | `allinurl:amazon field-keywords nikon` |
| **Body search** | `intext:term` | Term must appear in the page body text — bypasses title/meta gaming | `intext:"orbi vs eero vs google wifi"` |
| **All in body** | `allintext:words` | ALL terms must be in the body text | `allintext:react hooks performance optimization` |
| **Filetype** | `filetype:ext` or `ext:ext` | Restricts to specific file type (pdf, doc, xls, ppt, csv, etc.) | `filetype:pdf "system design" interview` |
| **Date restrict** | `after:YYYY-MM-DD` / `before:YYYY-MM-DD` | Filters by publication date — combine both for a date range | `"gpt-5" after:2025-01-01 before:2026-01-01` |
| **Proximity** | `AROUND(X)` | Terms must appear within X words of each other — precision for related concepts | `tesla AROUND(3) edison` |
| **Related** | `related:domain.com` | Finds sites similar to the given one — great for competitor discovery | `related:vercel.com` |
| **News source** | `source:name` | Restricts Google News results to a specific publication | `"AI regulation" source:reuters` |
| **Define** | `define:term` | Fetches definitions — useful for jargon or disambiguation | `define:idempotent` |
| **Price search** | `$amount` or `€amount` | Searches for specific prices | `tesla model 3 $35000..50000` |

## Common Noise Exclusion Patterns

```
-"top 10" -"top 5" -"best of" -listicle -pinterest -quora
-site:medium.com        # optional, when Medium quality is insufficient
-site:w3schools.com     # for programming searches, prefer MDN/official docs
-"sponsored" -"affiliate"
```

## Domain-Specific Authoritative Sources

### Programming / Tech
`site:stackoverflow.com OR site:github.com OR site:dev.to OR site:docs.* OR inurl:documentation`

### AI / ML
`site:arxiv.org OR site:huggingface.co OR site:openai.com OR site:anthropic.com OR site:deepmind.google`

### Business / Startups
`site:techcrunch.com OR site:bloomberg.com OR site:reuters.com OR site:crunchbase.com`

### Design / UX
`site:nngroup.com OR site:smashingmagazine.com OR site:designsystems.com OR site:figma.com`

### Legal / Compliance
`site:law.cornell.edu OR site:eur-lex.europa.eu OR filetype:pdf (regulation|directive|statute)`

## Deprecated Operators (don't use these)

These operators no longer work in Google and will waste your query budget:
- `~term` (synonym search) — deprecated 2013, Google does this automatically now
- `+term` (force exact) — deprecated 2011, use `"term"` instead
- `link:domain.com` (find backlinks) — deprecated, use backlink tools instead
- `info:url` (page info) — deprecated 2017
- `cache:url` (cached page) — discontinued 2024

## Tool-Specific Syntax Support

### WebSearch / Brave Search
- Supports: `"exact phrases"`, `site:`, `OR`, `-exclude`, `filetype:`
- May not support: `intitle:`, `inurl:`, `after:/before:`, number ranges
- Workaround for dates: append year like `2025` or `2025 2026` as regular terms

### Google (via browser)
- Full operator support
- Combine freely: `site: + intitle: + "phrase" + OR + -exclude + after:`

### When operators aren't supported
Fall back to smart keyword selection:
- Still use diverse synonyms (even without OR, search engines do some fuzzy matching)
- Put the most important/specific terms first
- Use quotes for exact phrases (most engines support this)
- Add year numbers for freshness
- Add domain names as regular keywords: `stackoverflow react hooks 2025`
