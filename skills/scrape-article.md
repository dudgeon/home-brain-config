---
name: scrape-article
description: Capture a single article, podcast, or newsletter as an ai-pm source. Use when the user provides a URL and asks to add, capture, or scrape a specific piece of content. This skill handles the full capture workflow for one item — duplicate check, fetch, triage summary, source file creation, registry updates, and scrape log update (for known publications). Does NOT perform source processing (idea extraction); use ai-pm-source-processing for that. Trigger with `/scrape-article [URL]`.
user-invocable: true
---

# scrape-article

Capture a single piece of content as an ai-pm source file.

**Project location**: `domains/professional-development/ai-pm/`

---

## When to Use

- User provides a URL and asks to "add", "capture", or "scrape" it
- User invokes `/scrape-article [URL]` directly
- Single-article capture during inline triage (contrast with bulk scraping, which uses `ai-pm-source-scraping`)

---

## Workflow

### Step 0 — Extract the URL

If the user invoked `/scrape-article [URL]`, the URL is the argument. If a URL wasn't provided, ask for it before proceeding.

---

### Step 1 — Duplicate check

Before doing anything else, check whether this article is already in the knowledge base.

1. Search `sources/README.md` for the URL (or a close slug match)
2. Grep for the URL string across `sources/*.md`
3. If a match is found: report it to the user, link to the existing file, and stop. Do not re-capture.

---

### Step 2 — Determine metadata

From the URL alone, infer:
- **Publication**: every.to, lennysnewsletter.com, chatprd.ai, etc.
- **Source type**: article, podcast, newsletter, video
- **Author/host**: may be inferable from URL path or publication convention
- **Published date**: attempt to infer from URL slug if date-stamped; confirm after fetch

---

### Step 3 — Fetch content

Try in order until one succeeds:

1. **WebFetch** — fast, no auth. Try first.
2. **Chrome MCP** — use when WebFetch returns 403, paywalled content, or clearly truncated output:
   ```
   tabs_context_mcp (createIfEmpty: true)
   → navigate to URL
   → wait (2 seconds)
   → get_page_text
   ```
   If `get_page_text` returns minimal content (e.g., only a comment or stub), follow up with:
   ```
   javascript_tool: document.querySelectorAll('p, h1, h2, h3, h4, li, blockquote')
     → map to textContent → join('\n')
   ```
3. **WebSearch** — search for cached or alternative versions of the article
4. **Flag for user** — only after exhausting all three. Never silently accept partial content.

---

### Step 4 — Generate triage summary

Read the fetched content and write a 1-3 sentence dense summary for the frontmatter `summary` field.

**Tone**: Dense, no fluff. Acronyms OK. What would you need to know to decide whether to read this?
**Focus**: Key claims, frameworks, novel/contrarian arguments.
**Not**: Theme extraction or knowledge-base analysis — that's processing, not capture.

If the content is paywalled and only partially available, note the approximate word count accessible and reflect that in the summary.

---

### Step 5 — Create source file

**Filename**: `YYYY-MM-DD-descriptive-slug.md` where the date is today (the discovered/captured date).

Place in: `domains/professional-development/ai-pm/sources/`

**Frontmatter template**:

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
template: templates/source.md
template_version: 3
tags: [source, ai-pm]
status: unread
source_type: [article | podcast | newsletter | video | note | organic]
source_url: "[original URL]"
archive_url: "domains/professional-development/ai-pm/sources/FILENAME.md"
author: "[primary voice — for podcasts: the GUEST, not the host]"
host: "[host name — podcasts only; omit field for articles]"
published: YYYY-MM-DD
discovered: YYYY-MM-DD
summary: "[triage summary from Step 4]"
domain: professional-development
project: ai-pm
---
```

**Attribution rule**: `author` = the primary voice whose ideas the source captures. For interviews/podcasts, this is the **guest**, not the host. Host goes in the `host` field.

**Body**:

```markdown
# [Title]

**By**: [Author] ([role/affiliation if known])
**Host**: [Host] ([show name]) — *podcasts only*
**Source**: [Publication name](original_url)
**Type**: [source_type]

## Summary

*Fill after reading.*

## Key Ideas Extracted

*Fill during processing.*

## Notes

- [Any capture notes: partial paywall, content gaps, cross-references]

## Raw Content

[Paste fetched content here]
```

If content was partially paywalled, note this explicitly in both the frontmatter `summary` and the `## Notes` section.

---

### Step 6 — Update sources/README.md

Open `sources/README.md` and make two edits:

1. **All Sources table** — add a row:
   ```
   | YYYY-MM-DD | [Title] | [Author] | [source_type] | [link](filename.md) | unread | — |
   ```
   Insert in date order (newest first).

2. **Unread section** — add a row to the Unread Sources table:
   ```
   | [Title] | [Author] | [source_type] | [link](filename.md) | [summary 1-liner] |
   ```

3. **Update Stats** — increment "Total sources" and "Unread" counts.

4. Update the `updated` date in frontmatter.

---

### Step 7 — Update scrape log (if applicable)

Check whether the URL belongs to a known publication with an existing scrape log:

| Publication | URL pattern | Scrape log |
|---|---|---|
| every.to | `every.to/` | `sources/every-to-scrape-log.md` |
| Lenny's Newsletter | `lennysnewsletter.com/` | `sources/lennys-scrape-log.md` |
| ChatPRD (How I AI) | `chatprd.ai/` | `sources/chatprd-scrape-log.md` |

If the URL matches a known publication:

1. Read the scrape log
2. Add a row to the Articles table with `decision: scrape` and a link to the new source file:
   ```
   | YYYY-MM-DD | [Title] | [Author] | [link](url) | scrape | [filename.md](filename.md) |
   ```
3. Update the frontmatter `updated` date and `scrape_range` end date if this article is newer than the current range end

If the URL does not match any known publication, skip this step.

---

### Step 8 — Report

Confirm what was done:

- Source file created: `[filename.md](path/to/file)`
- Status: unread
- Scrape log updated: yes/no
- Any caveats (partial paywall, content gaps, etc.)

Ask if the user wants to process it now (→ `ai-pm-source-processing` skill) or leave it in the reading queue.

---

## What This Skill Does NOT Do

- **Source processing** (idea extraction into knowledge entries) — use `ai-pm-source-processing` for that
- **Bulk scraping** — use `ai-pm-source-scraping` for paginating a publication and capturing many articles at once. That skill delegates its per-article capture to this one (Phase 3).
- **Updating the ai-pm README Knowledge Map** — that happens during processing, not capture

---

## Quick Reference

| Action | Where |
|---|---|
| Duplicate check | `sources/README.md` + grep `sources/*.md` |
| Source files | `sources/YYYY-MM-DD-slug.md` |
| Registry to update | `sources/README.md` |
| Scrape logs (optional) | `sources/*-scrape-log.md` |
| Fetch fallback | WebFetch → Chrome MCP → WebSearch |
| Paywall partial content | Note in `summary` + `## Notes`; do NOT silently proceed |
