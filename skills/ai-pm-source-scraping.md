---
name: ai-pm-source-scraping
description: Bulk scraping of source publications for the AI PM learning project. Use when the user asks to scrape a newsletter or publication (every.to, chatprd.ai, lennysnewsletter.com, etc.), catch up on new articles since the last run, or add a new publication to the scraping registry. This skill handles the discovery, filtering, logging, and batch capture workflow. For the source lifecycle after capture (reading, processing, extraction), use ai-pm-source-processing.
---

# AI PM — Source Scraping

Bulk discovery and capture of source material from publications for the AI PM learning project.

**Project location**: `domains/professional-development/ai-pm/`
**Scrape logs live in**: `domains/professional-development/ai-pm/sources/`

---

## When This Applies

- User asks to scrape a specific publication (every.to/newsletter, chatprd.ai, lennysnewsletter.com, etc.)
- User asks to "catch up" on a publication since the last run
- User asks to add a new publication to the scraping registry
- User wants to see what was considered and skipped from a publication

---

## Overview

Scraping a publication is a three-phase workflow:

1. **Browse** — paginate the publication to discover all articles
2. **Decide** — filter each article (scrape / skip) using publication-specific criteria
3. **Capture** — fetch content and create source files; update the scrape log

---

## Phase 1: Browse

### Check the Scrape Log First

Before browsing, check if a scrape log already exists for this publication:
- `sources/every-to-scrape-log.md`
- `sources/chatprd-scrape-log.md`
- `sources/lennys-scrape-log.md`

If a log exists, note the last scraped date and the range covered. Only collect articles newer than the last run (or articles marked `pending` that weren't yet captured).

### Browser Navigation

Use Chrome MCP tools to paginate through the publication:

```
tabs_context_mcp (createIfEmpty: true)
→ navigate to the publication's article index
→ wait (2 seconds)
→ javascript_tool to extract article links
```

**JavaScript to extract article links** (adapt selector to publication):
```javascript
// Generic — adjust selectors to the publication
Array.from(document.querySelectorAll('main a'))
  .map(a => ({ href: a.href, text: a.textContent.trim() }))
  .filter(a => a.href.includes('publication-domain.com')
            && !a.href.includes('/tag/')
            && !a.href.includes('/author/')
            && a.text.length > 5)
  .map(a => `${a.text} → ${a.href}`)
  .join('\n')
```

Paginate through all pages (pagination links, "Load more", or URL param `?page=N`) until the last-scraped date is reached. Collect all articles into a working list before deciding.

### Publication-Specific Navigation

**every.to/newsletter**: Paginate with `/newsletter?page=N`. 20 articles per page. Stop when you reach the date of the last scrape run.

**chatprd.ai/how-i-ai**: Paginate the podcast archive. Episodes published weekly (approx). Stop at last scraped date.

**lennysnewsletter.com**: Use the archive or search for AI-tagged content. The full catalog is very large; use topic filtering rather than exhaustive pagination.

---

## Phase 2: Decide

### Decision Categories

| Decision | Meaning |
|---|---|
| `scrape` | Captured as ai-pm source (already done) |
| `skip` | Not captured — reason documented |
| `pending` | Queued for capture in this run |

### Universal Skip Criteria (all publications)

- Company/product announcements for the publication itself
- Pure promotional posts (subscribe CTAs, event registrations, merch)
- Newsletter digests/roundups (curations of other content, not original ideas)
- Tool model vibe checks / benchmark reviews (unless the workflow insight justifies it)
- Content not primarily about AI practices, AI-PM, or AI-native work patterns

### Publication-Specific Filter Heuristics

#### every.to/newsletter

URL prefix predicts article type:

| Prefix | Type | Default |
|---|---|---|
| `source-code/` | Technical how-tos | SCRAPE |
| `chain-of-thought/` | Dan Shipper essays | case-by-case |
| `podcast/` | Interviews | SCRAPE |
| `context-window/` | Newsletter digests | SKIP |
| `vibe-check/` | Model/tool reviews | SKIP |
| `on-every/` | Company announcements | SKIP |
| `playtesting/` | Game AI (Good Start Labs) | SKIP |
| `learning-curve/` | Learning with AI | SCRAPE |
| `also-true-for-humans/` | AI & human behavior | SCRAPE |
| `thesis/` | Strategic essays | SCRAPE |
| `working-overtime/` | Personal productivity | case-by-case |
| `p/` | General articles | case-by-case |

**Chain-of-thought / working-overtime judgment**: Keep if the essay surfaces a transferable model for how to think about AI at work. Skip if it's primarily philosophical, literary, or personal without a usable insight.

#### chatprd.ai (How I AI podcast)

Nearly all podcast episodes are relevant — the show's format guarantees workflow demonstrations. Scrape everything except:
- ChatPRD platform/feature announcements
- Blog posts that are purely promotional (no workflow content)
- Recaps of events with no substantive technique transfer

#### lennysnewsletter.com

Lenny's covers the full PM surface; only the AI-focused slice is relevant. **Do not exhaustively paginate** — use search or topic filters. Scrape:
- Episodes / posts explicitly about AI workflows for PMs
- Guest posts demonstrating AI tooling or AI-native work patterns
- Lenny's own essays on AI's impact on PM roles

Skip (even if AI is mentioned):
- General PM strategy, discovery, roadmapping, OKRs (unless AI angle is primary)
- GTM, growth, marketing without AI focus
- PM career advice without AI context
- Startup fundraising, pricing, hiring (unless AI-native hiring patterns)

### Handling Duplicates

A single piece of content sometimes appears under multiple URLs or dates (e.g., reposted, updated slug, crossposted). When duplicates are found:
- Mark earlier/lower-quality URL as `skip` with note `⚠️ Duplicate of [other URL]`
- Scrape only the canonical/later version

---

## Phase 3: Capture

### For Small Batches (< 10 articles)

Scrape articles inline. For each `pending` article:

1. Fetch content — WebFetch first, Chrome MCP as fallback (for paywalled content)
2. Create source file using the **ai-pm-source-processing** skill workflow ("Add New Source" step)
3. Update the scrape log: change `pending` to `scrape` and add the source file link

### For Large Batches (10+ articles)

Launch a background general-purpose agent. Provide it with:
- The full list of pending articles (title, URL, author, date)
- The source file template format (from `templates/source.md`)
- Fetch instructions (WebFetch → Chrome MCP fallback)
- Output directory: `domains/professional-development/ai-pm/sources/`
- Instruction to update the scrape log as each file is created

After the agent completes, verify source files exist and update the scrape log accordingly.

### Source File Creation (per article)

Follow the **ai-pm-source-processing** skill for the full "Add New Source" procedure. Key fields:

```yaml
status: unread
source_type: [article | podcast | newsletter | video]
source_url: "[original URL]"
author: "[primary voice — for podcasts: the GUEST, not the host]"
host: "[host name — podcasts only, omit for articles]"
published: YYYY-MM-DD
discovered: [today's date — the date claude captured it]
summary: "[1-3 dense sentences for triage]"
```

After each source file is created:
- Add it to `sources/README.md` (All Sources table + Unread section)
- Update the scrape log: change decision to `scrape`, link to source file

---

## Scrape Log Format

Every publication gets a scrape log at `sources/{publication-slug}-scrape-log.md`.

### Frontmatter

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [scrape-log, {publication-slug}, ai-pm]
status: active
domain: professional-development
project: ai-pm
scrape_range: "YYYY-MM-DD to YYYY-MM-DD"
scrape_sections: "{base URL scraped}"
scraped_by: claude
---
```

### Required Sections

1. **Publication Overview** — what this publication is, format, cadence, why it's relevant
2. **Decision Key** — the scrape / skip / pending legend
3. **Skip Criteria** — what types of content from this publication get skipped and why
4. **Articles table** — one row per article considered (see below)
5. **Stats** — scraped count, skipped count, catalog coverage notes

### Articles Table Format

```markdown
| Date | Title | Author | URL | Decision | Reason / Source File |
|---|---|---|---|---|---|
| YYYY-MM-DD | Title | Author | [link](url) | scrape | [source-file-slug.md](source-file-slug.md) |
| YYYY-MM-DD | Title | Author | [link](url) | skip | Newsletter digest |
| YYYY-MM-DD | Title | Author | [link](url) | pending | — |
```

For publications with URL column prefixes (like every.to), include a **Column Guide** section that maps prefix → type → default decision.

### Catch-up Runs

When running a catch-up on an existing publication:
1. Add a new date-stamped section to the existing log (e.g., `### Catch-up Run: 2026-03-XX`)
2. List only new articles (since last run)
3. Update the frontmatter `scrape_range` end date and `updated` date
4. Update the **Next run** note at the top of the log

### Adding a New Publication

When adding a publication that was previously scraped without a log:
1. Create the scrape log from scratch using the format above
2. Back-fill all known scraped articles (look in `sources/README.md` for existing sources from that publication)
3. Use `discovered` dates in existing source files to infer when the scrape was done
4. Mark known-scraped articles as `scrape` with link to source file
5. Infer and document typical skip categories (don't need to enumerate every skipped article if the catalog is large — document patterns instead)

---

## Existing Scrape Logs

| Publication | Log file | Last scraped | Coverage |
|---|---|---|---|
| every.to/newsletter | [every-to-scrape-log.md](every-to-scrape-log.md) | 2026-02-17 | Pages 1–6 (Sep 2025 – Feb 2026) |
| chatprd.ai (How I AI) | [chatprd-scrape-log.md](chatprd-scrape-log.md) | 2026-02-15 | Apr 2025 – Feb 2026 |
| lennysnewsletter.com | [lennys-scrape-log.md](lennys-scrape-log.md) | 2026-02-15 | Selective; AI content only |

---

## Paywall Handling

Many publications have paywalled content. Fetch chain:

1. **WebFetch** — try first (fast, no auth)
2. **Chrome MCP** — use when WebFetch returns 403 or partial content
   ```
   tabs_context_mcp → navigate → wait 2s → get_page_text
   ```
   Works because Chrome uses the user's authenticated session.
3. **WebSearch** — try for cached versions of specific articles
4. **Flag for user** — only after exhausting all three

Never silently accept partial content. If the article is paywalled and all fetch methods fail, record it in the scrape log with decision `skip` and reason `Paywall — content unavailable`.
