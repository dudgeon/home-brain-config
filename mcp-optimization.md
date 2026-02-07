# Writing for MCP Discoverability

This repo is accessed two ways:
1. **Claude Code** — direct file system access
2. **brain-stem MCP** — semantic search via `search_brain`, `get_document`, `list_folders`, `list_recent`

Content should work well for both. This guide focuses on what helps MCP consumers.

---

## How Search Works

`search_brain(query)` does semantic search across all markdown content. It returns relevant passages with source paths. The searcher might be:
- Claude on mobile with no file system access
- Claude in claude.ai with limited context
- Any MCP client looking for information

---

## Writing for Discoverability

**Front-load important concepts.** The first paragraph matters most. State what the document is about in plain language.

**Use natural language, not just structure.** Bullet points and tables are scannable but don't search well. Include prose that contains the words someone might search for.

**Include synonyms.** Someone might search "kid" or "child" or "son". If all three could be relevant, use all three somewhere in the document.

**Make documents self-contained.** When retrieved via MCP, the document might appear without its parent README context. Include enough background that it makes sense standalone.

**Use descriptive headers.** Headers often match search queries. "Workshop Equipment" is better than "Equipment" if someone might search "workshop equipment."

---

## What Hurts Discoverability

- **Cryptic filenames** — The path appears in search results
- **Deep nesting** — Harder to browse via `list_folders`
- **Structured-only content** — Tables and lists without prose context
- **Jargon without explanation** — Inside terms that searchers won't use
- **Context-dependent references** — "See above" or "As mentioned" without restating

---

## README Files

Domain READMEs serve as entry points for `list_folders` browsing. They should:
- Explain what the domain contains
- List key subfolders and their purposes
- Include example queries that work with `search_brain`
- Stay updated as the domain evolves

---

## Testing Discoverability

When creating important content, imagine searching for it:
- What would you search?
- Would this document surface?
- Does it contain those words?
