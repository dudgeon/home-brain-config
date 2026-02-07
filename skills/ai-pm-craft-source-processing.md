---
name: ai-pm-craft-source-processing
description: Process source material for the AI PM Craft learning project. Use when user says "add this source", "I read this", "process this source", "extract ideas from this", "what's in my reading queue", or when an inbox item is tagged with project ai-pm-craft. This skill is specific to the ai-pm-craft project in domains/professional-development/ai-pm-craft/.
---

# AI PM Craft — Source Processing

Handling the lifecycle of external source material — from capture through reading to knowledge extraction — for the AI PM Craft learning project.

**Project location**: `domains/professional-development/ai-pm-craft/`

---

## When This Applies

- User shares a link, article, video, or content to capture for ai-pm-craft
- User wants to mark something as read or add reading notes for ai-pm-craft
- User asks to process/extract ideas from an ai-pm-craft source
- Inbox triage encounters an item tagged `project: ai-pm-craft`
- User asks about their ai-pm-craft reading queue or source status
- User asks to update the ai-pm-craft knowledge base from recent reading

---

## The Goal

Every source captured should eventually be **read, understood, and its discrete ideas extracted into the knowledge base** with full lineage. The knowledge base is the product; sources are the raw material.

---

## Workflows

### Add New Source

Trigger: User shares content (link, text, dictation about something they found)

1. **Determine source metadata**: title, author, source_url, source_type, published date
2. **Fetch content if possible**: Use the source URL to retrieve the full text/transcript. If blocked, note that raw content needs manual capture.
3. **Create source file** from `templates/source.md` in `domains/professional-development/ai-pm-craft/sources/`
   - Filename: `YYYY-MM-DD-slug.md` (date = discovered date, slug = descriptive)
   - Set `status: unread`
   - Fill in all known frontmatter
   - Place raw content under `## Raw Content`
   - `archive_url` = the GitHub path to this file (relative to repo root)
4. **Update `sources/_index.md`**: Add row to the All Sources table and the Unread section
5. **Update the ai-pm-craft README.md reading queue section**: Add to "Up Next (Unread)"

### Mark Source as Read

Trigger: User says "I read [source]" or "mark X as read"

1. **Update source frontmatter**: `status: read`, update `updated` date
2. **Add summary**: If user provides thoughts, fill the Summary section. If not, generate a brief summary from the raw content and ask user to validate.
3. **Add notes**: Capture any user annotations, reactions, questions
4. **Update `sources/_index.md`**: Move from Unread to Read section
5. **Update ai-pm-craft README.md reading queue**: Move from "Up Next" to appropriate spot
6. **Flag for processing**: Mention that ideas can now be extracted when ready

### Process Source → Extract Ideas

Trigger: User says "process this", "extract ideas from X", or batch processing

This is the judgment-heavy step. Think carefully.

1. **Read the source thoroughly** — understand the full argument, not just highlights
2. **Identify discrete ideas**: Each source contains multiple ideas. An "idea" is:
   - A named technique or method (→ `technique`)
   - A framework for thinking about problems (→ `mental-model`)
   - An observation, principle, or non-obvious pattern (→ `insight`)
3. **For each idea, decide**: Is this genuinely new to the knowledge base, or a restatement of something already captured?
   - **New idea**: Create knowledge entry from `templates/knowledge-entry.md` in appropriate `knowledge/` subfolder (`techniques/`, `mental-models/`, or `insights/`)
   - **Existing idea with new perspective**: Update existing knowledge entry — add new source citation to the Sources section, enrich Summary or How to Apply if this source adds something genuinely new
   - **Restatement with nothing new**: Just add a citation to the existing entry's Sources section
4. **Write the citation block** for each idea×source pairing:
   - Extract the best quote that demonstrates the idea
   - Write "What this source adds" — how this specific source's framing is unique or useful
   - Include both original URL and archive wikilink
5. **Update the source file**: Fill in `## Key Ideas Extracted` with links to each knowledge entry. Set `status: processed`.
6. **Update `sources/_index.md`**: Move to Processed section, fill in "Ideas Extracted" column
7. **Update ai-pm-craft README.md Knowledge Map**: Add new entries under appropriate headings. If no heading fits, place under Uncategorized and flag for the user.
8. **Update ai-pm-craft README.md reading queue**: Move to "Recently Processed"

### Review Reading Queue

Trigger: "What's in my queue?", "what should I read?", "reading status"

1. Query `sources/_index.md` for status counts
2. Report: total sources, unread count, reading count, read-but-unprocessed count, processed count
3. List unread sources with brief descriptions and source types
4. Highlight any read-but-unprocessed sources (these represent unrealized value)

### Knowledge Gap Analysis

Trigger: "What haven't I covered?", "where are the gaps?", "what topics need more sources?"

1. Review the Knowledge Map headings in ai-pm-craft README.md
2. For each section, assess: How many entries? How well-sourced (single source vs. multi-source)?
3. Identify thin areas: sections with few entries or entries backed by only one source
4. Report gaps and suggest what types of sources might fill them

---

## How to Think About It

**Atomicity matters — err on the side of more entries, not fewer.** One idea per knowledge entry. If a source describes a three-step workflow, each step is likely its own technique with its own rationale and applicability. "This article has a three-step process" should produce three entries, not one entry describing all three steps. The test: does this step have its own *why*, its own situations where it applies or doesn't, its own nuances? If yes, it's a separate entry.

**Decompose workflows into their constituent techniques.** A workflow like "PRD → Task List → Execution" contains three discrete techniques: (1) interactive PRD writing (templatization + follow-up questions), (2) task list generation (observability, stakeholder engagement), (3) stepwise task execution (human-in-the-loop checkpoints). Each has different value, different contexts where it applies, and different lessons. Extract them separately. Cross-reference them via the Related section.

**Name entries by what they teach, not where they came from.** "Ryan Carson's three-file system" is a source reference. "Interactive PRD Writing with AI" is a teachable technique. The entry should be recognizable and useful even if you've never read the source.

**Capture the unique mechanism, not just the outcome.** For a technique, the *how* matters as much as the *what*. "Use AI to write PRDs" is too vague. "Templatize PRD generation with rule files that instruct AI to ask clarifying questions before writing" captures the actual insight.

**Watch for portable prompting patterns.** Some techniques are embedded inside larger workflows but are independently valuable — specific enough to use verbatim, broad enough to apply across many contexts. Examples: "Be 100 times more specific" (forces AI past platitudes in *any* domain), "MY job is X, YOUR job is Y" (delineates human/AI responsibility in *any* prompt). These are easy to overlook because they appear as single steps in a bigger workflow. Extract them as their own entries whenever the mechanism is specific and the applicability is broad. The test: could someone use this exact pattern tomorrow in a completely different context and get value from it?

**Screen for change management and adoption.** Sources often embed insights about how leaders drive adoption of AI-native skills alongside the craft techniques themselves. These are easy to overlook because they feel like "context" rather than "content" — but they're independently valuable. Examples: framing AI skills as mandatory for one's next job (urgency creation), running live demos to shift skeptics (show-don't-tell adoption), pairing resisters with early adopters (social proof). When you find these, extract them as knowledge entries under the Change Management & Adoption section. The test: is this about *how to bring others along* rather than *how to do the craft yourself*?

**Lineage is the whole point.** The knowledge entry is the synthesis, but its value comes from being able to trace back to specific quotes and demonstrations in specific sources. Don't skimp on citations.

**Don't duplicate, enrich.** When a new source discusses an idea that already has a knowledge entry, resist creating a new entry. Instead, add the new source's perspective to the existing entry. The entry gets richer over time.

**Status reflects reality.** If you extracted ideas but didn't create proper knowledge entries, the source isn't "processed" — it's "read." Be honest about status.

**The Knowledge Map is the consumption layer.** Individual entries are powerful but hard to browse. The ai-pm-craft README's Knowledge Map is what makes the knowledge base navigable. Every new or moved entry must be reflected there.

**Respect the hierarchy.** Place entries in the most specific applicable section of the Knowledge Map. If it doesn't fit, put it in Uncategorized and flag it — the hierarchy may need to evolve.

---

## Watch Out For

**Overly broad entries.** "AI Product Management Best Practices" is too broad. "Pre-mortem for AI Feature Launches" is right. If you can't explain the idea in 2 sentences, it's probably multiple ideas. Similarly, "PRD-Driven AI Development" that covers three distinct techniques is too broad — split into "Interactive PRD Writing," "Task List Generation for Observability," and "Stepwise Task Execution."

**Under-extracting from rich sources.** A source with three workflows likely contains 5-8 discrete ideas, not 3. Look for: the overarching insight *and* each individual technique *and* any broadly applicable sub-techniques (e.g., a prompting pattern that works beyond the specific use case described).

**Missing lineage.** Every claim in a knowledge entry should trace to a source. If you're synthesizing from your own reasoning (not from sources), mark it clearly as editorial.

**Stale reading queue.** Sources sitting at "unread" for weeks are a signal. Surface them proactively.

**Knowledge Map drift.** After processing sources, always check that the Knowledge Map reflects the current state. It's the primary navigation tool.

**Premature hierarchy.** Don't create deeply nested sub-sections in the Knowledge Map until there are enough entries to warrant it. Start flat, add structure as volume demands.

---

## Organic Sources (Draft)

Some sources are written by the user (blog posts, reflections, case studies) rather than captured from external authors. These are "organic sources" and follow the same lifecycle as external sources:

- **During inbox triage**: Register them as sources with `status: unread` and `source_type: organic`. Do NOT process or extract ideas — they are drafts that need the user's review first.
- **The user decides when to process**: Organic drafts may still be evolving. Only extract knowledge entries when the user explicitly asks.
- **Frontmatter signal**: Look for `context: new organic source` or `status: draft` in inbox items. These indicate organic content.
- **Filing**: Create the source file the same way as external sources, but set `source_url` to the repo path (or leave blank if unpublished) and note `source_type: organic` in frontmatter.

---

## Note Sources

Quick notes — ideas, techniques, learning goals — that are nascent and not yet backed by external sources. These arrive via inbox save and represent the kernel of an idea, not a fully formed argument.

- **During inbox triage**: Create a source file with `source_type: note`. The note text goes in **Summary** and **Notes** sections — no Raw Content section needed. Set `status: unread`. Author is the user.
- **Processing**: Notes can be processed immediately since there's nothing to "read" that you haven't already captured. During processing, extract the idea into a `draft` knowledge entry — but **do not elaborate beyond what the note states**. If the note says "pre-mortems might work well for AI feature launches," the entry should capture exactly that level of specificity, not manufacture a full technique description.
- **Weight**: A note alone keeps a knowledge entry at `draft`. Promotion to `solid` requires corroboration from an external source or the user fleshing out the idea substantially.
- **Categorization**: Notes appear in `sources/_index.md` like any other source, making them visible during knowledge gap analysis and future processing.
- **When a real source corroborates a note**: Enrich the existing `draft` knowledge entry with the source's citation, quotes, and framing. Promote to `solid` if the source provides substantial evidence. This is the "don't duplicate, enrich" rule applied to notes.

---

## Source Types Expected

- X/Twitter threads
- YouTube videos/transcripts
- Newsletters (Lenny's, etc.)
- Blog posts and articles
- Podcast transcripts
- Book chapters
- Organic (user-authored drafts, case studies, reflections)
- Notes (quick ideas, nascent techniques, learning goals — user-originated, pre-corroboration)

## Knowledge Map Sections

Two axes:

**Product Lifecycle** (vertical — @user to provide definitive framework):
Strategy & Vision, Shipping & Execution, Evaluation & Measurement, UX & Design

**Horizontal Skills** (cut across lifecycle stages):
Prompt Engineering, Working with Agentic Systems, Change Management & Adoption

When placing an entry, first ask: is this about a *stage* of building a product (→ Product Lifecycle), or is it a *skill* that applies regardless of stage (→ Horizontal)? Prompt engineering = specific patterns for getting better AI outputs. Working with agentic systems = the meta-process of designing, evaluating, and refining human-AI collaboration.
