# File Conventions

This document consolidates all file naming, metadata, and linking conventions for home-brain.

---

## Folder Structure

- `tasks.md` - All tasks across domains (check at session start!)
- `domains/` - Life areas (Workshop, Etsy, Board, Family, People, Travel, Food & Drink, House, Brain, Interests, Professional Development)
- `context/` - Cross-domain context files for Claude
- `decisions/` - Decision records
- `inbox/` - Items arriving for Claude to process (not a general dumping ground)
- `templates/` - Note templates for consistency

---

## File Naming Conventions

- Use lowercase with hyphens: `my-project-name.md`
- Include dates for time-based notes: `YYYY-MM-DD-description.md`
- Keep names descriptive but concise
- Use prefixes for series: `meeting-2026-01-19-board.md`

---

## Metadata Standards

All notes should include frontmatter:

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2, tag3]
status: active|planned|completed|archived
---
```

### Template-Derived Documents

Documents created from templates should track their lineage:

```yaml
---
created: 2026-01-25
updated: 2026-01-25
template: templates/person.md
template_version: 1
tags: [person]
status: active
---
```

- `template`: Path to the source template (relative to repo root)
- `template_version`: Version number of the template when document was created

**Why this matters**: When a template is updated, an agent can find all derived documents and apply structural updates. The version number identifies which docs need attention.

### Reference Documents

Documents uploaded or imported (not created from a template) should be marked as reference material:

```yaml
---
created: 2026-01-25
updated: 2026-01-25
type: reference
source: uploaded
tags: [reference, topic]
status: active
---
```

- `type: reference` — Distinguishes from template-derived docs
- `source`: Where it came from
  - `uploaded` — User uploaded directly
  - `external` — Copied from external source
  - `imported` — Migrated from another system

Reference documents may or may not follow template structure. They exist as source material, not as structured notes that need template updates.

### Source Documents (Learning Projects)

Source material captured for learning projects (articles, videos, podcasts, etc.) uses the `templates/source.md` template and tracks dual URLs:

```yaml
---
created: 2026-02-05
updated: 2026-02-05
template: templates/source.md
template_version: 3
tags: [source, ai-pm-craft]
status: unread
source_type: article
source_url: "https://example.com/original-article"
archive_url: "domains/professional-development/ai-pm-craft/sources/2026-02-05-slug.md"
author: "Guest Name"
host: "Interviewer Name"
published: 2026-02-05
discovered: 2026-02-05
summary: "Dense triage summary generated at ingestion. Key claims, frameworks, takeaways."
domain: professional-development
project: ai-pm-craft
---
```

- `source_url` — Canonical URL of the original content (may go offline)
- `archive_url` — Path to the local copy in this repo (permanent)
- `source_type` — One of: `article`, `video`, `podcast`, `newsletter`, `tweet-thread`, `book-chapter`, `organic`, `note`
- `status` — Reading lifecycle: `unread` → `reading` → `read` → `processed`
- `author` — Primary voice whose ideas this captures. For interviews: the **guest**. For articles: the writer.
- `host` — Interviewer or show host. Omit for single-author formats.
- `summary` — Triage summary auto-generated at ingestion. Dense, concise, focused on key insights. Enables quick queue review without reading source body. Distinct from the `## Summary` body section, which is filled post-reading with the user's own take.
- `project` — Which learning project this source belongs to

### Knowledge Entries (Learning Projects)

Synthesized ideas extracted from sources use `templates/knowledge-entry.md`:

```yaml
---
created: 2026-02-05
updated: 2026-02-05
template: templates/knowledge-entry.md
template_version: 2
tags: [knowledge, ai-pm-craft]
status: draft
entry_type: technique
origin: sourced
featured: false
domain: professional-development
project: ai-pm-craft
---
```

- `entry_type` — One of: `technique`, `mental-model`, `insight`
- `status` — Maturity: `draft` → `solid` → `canonical`
- `origin` — `sourced` (from external sources), `organic` (from personal experience), `both` (organic + later found in sources)
- `featured` — Techniques worth championing organizationally — candidates for collateral building and adoption storytelling

### Template Files

Templates themselves use this frontmatter:

```yaml
---
type: template
version: 1
changelog: |
  v1 (2026-01-25): Initial template
---
```

Increment version when making structural changes. Add changelog entry describing what changed.

---

## Linking Conventions

- **Use relative markdown links** for all internal links: `[project-name](path/to/project-name.md)`
- This is a GitHub repo, not Obsidian — `[[wikilinks]]` don't render and should not be used
- Always link related concepts - over-linking is better than under-linking
- When creating a note, check for and add relevant backlinks

---

## Note Creation Guidelines

When creating new notes:

1. **Check if a note exists first** - Search before creating
2. **Use templates** when available (see `templates/`)
3. **Add frontmatter** with metadata (date, tags, status)
4. **Create in the right domain** - Don't put everything in inbox
5. **Link to related notes** using relative markdown links

---

## Domain Structure

Each domain should have:
- `README.md` - Overview and domain-specific guidance
- Organized subfolders as needed
- Consistent naming within the domain

---

*When in doubt about where something goes or how to name it, check existing examples in the relevant domain.*
