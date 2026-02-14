---
name: ai-pm-craft-source-processing
description: Process source material for the AI PM Craft learning project. Use when the user mentions "ai-pm-craft", "pm project", or "pm craft" alongside source actions (add, read, process, extract, queue, status). Also use when inbox triage encounters items tagged project:ai-pm-craft. This skill is SPECIFIC to the ai-pm-craft project in domains/professional-development/ai-pm-craft/ — do not use for generic source handling.
---

# AI PM Craft — Source Processing

Handling the lifecycle of external source material — from capture through reading to knowledge extraction — for the AI PM Craft learning project.

**Project location**: `domains/professional-development/ai-pm-craft/`

---

## When This Applies

- User shares a link, article, video, or content to capture for ai-pm-craft
- User wants to mark something as read or add reading notes for ai-pm-craft
- User asks to process/extract ideas from an ai-pm-craft source
- User wants to capture an organic technique (from their own experience, no external source)
- User wants to mark a knowledge entry as featured
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

1. **Determine source metadata**: title, author, host (if interview), source_url, source_type, published date
   - **Attribution rule**: `author` = the primary voice whose ideas the source captures. For interviews/podcasts, this is the **guest**, not the host. The host goes in the `host` field. For multi-guest interviews, use the most relevant guest or comma-separate.
2. **Fetch content**: Use the source URL to retrieve the full text/transcript. If `WebFetch` fails (403, paywall, etc.), fall back to Chrome MCP tools (`tabs_context_mcp` → `navigate` → `wait` → `get_page_text`) which use the user's authenticated browser session. If Chrome MCP also fails, try `WebSearch` for cached/alternative versions. Only flag for manual capture after exhausting all three approaches.
3. **Generate triage summary**: Read the raw content and write a succinct summary for the frontmatter `summary` field. This is for quick triage — helping decide what's worth reading — not deep analysis.
   - **Tone**: Dense, concise, no fluff. Acronyms OK. Think "what would I need to know to decide whether to read this?"
   - **Focus**: Key claims, frameworks, or takeaways. What's novel or contrarian? What's the author's main argument?
   - **Length**: 1-3 sentences. Err short.
   - **Not**: Theme extraction, knowledge-base integration, or reading notes. That's for the processing step after reading.
4. **Create source file** from `templates/source.md` in `domains/professional-development/ai-pm-craft/sources/`
   - Filename: `YYYY-MM-DD-slug.md` (date = discovered date, slug = descriptive)
   - Set `status: unread`
   - Fill in all known frontmatter, including `summary` from step 3
   - Place raw content under `## Raw Content`
   - `archive_url` = the GitHub path to this file (relative to repo root)
5. **Update `sources/README.md`**: Add row to the All Sources table and the Unread section
6. **Update the ai-pm-craft README.md reading queue section**: Add to "Up Next (Unread)"

### Mark Source as Read

Trigger: User says "I read [source]" or "mark X as read"

1. **Update source frontmatter**: `status: read`, update `updated` date
2. **Add summary**: If user provides thoughts, fill the Summary section. If not, generate a brief summary from the raw content and ask user to validate.
3. **Add notes**: Capture any user annotations, reactions, questions
4. **Update `sources/README.md`**: Move from Unread to Read section
5. **Update ai-pm-craft README.md reading queue**: Move from "Up Next" to appropriate spot
6. **Flag for processing**: Mention that ideas can now be extracted when ready

### Process Source → Extract Ideas

Trigger: User says "process this", "extract ideas from X", or batch processing

This is the judgment-heavy step. Think carefully. **Before processing, read `meta/taxonomy.md`** for the full classification reference.

1. **Read the source thoroughly** — understand the full argument, not just highlights
2. **Identify discrete ideas**: Each source contains multiple ideas. An "idea" is:
   - A named technique or method (→ `technique`)
   - A framework for thinking about problems (→ `mental-model`)
   - An observation, principle, or non-obvious pattern (→ `insight`)
3. **Classify each idea** using the taxonomy (see `meta/taxonomy.md`):
   - **Domain**: Does it augment a specific lifecycle phase? → `product-lifecycle`. Cross-cutting skill, methodology, or knowledge area? → `horizontal`. About org change? → `ai-adoption`.
   - **Phase** (product-lifecycle, required): Which of the six phases — discover, frame, shape, build, release, measure?
   - **Component** (product-lifecycle, optional): What specific component within the phase? Use the component slug from the taxonomy. Omit if the entry fits the phase generally but not a specific component.
   - **Horizontal domain** (horizontal, required): Which sub-domain?
     - `practices` — atomic portable technique (prompting pattern, workflow step, collaboration pattern)
     - `software-delivery` — delivery methodology (compound engineering, spec-driven dev, vibe coding)
     - `agent-lifecycle` — managing agents as participants (selection, onboarding, feedback, performance)
     - `knowledge-management` — shared human-agent context systems (curation, access, knowledge-as-product)
   - **Lifecycle phase** (horizontal, optional): If the horizontal entry has a primary phase connection, record it for dual attribution.
4. **For each idea, decide**: Is this genuinely new to the knowledge base, or a restatement of something already captured?
   - **New idea**: Create knowledge entry from `templates/knowledge-entry.md` in the appropriate domain folder:
     - Product lifecycle: `knowledge-base/product-lifecycle/{phase}/`
     - Horizontal: `knowledge-base/horizontal/{horizontal_domain}/`
     - AI adoption: `knowledge-base/ai-adoption/`
   - **Existing idea with new perspective**: Update existing knowledge entry — add new source citation to the Sources section, enrich Summary or How to Apply if this source adds something genuinely new
   - **Restatement with nothing new**: Just add a citation to the existing entry's Sources section
5. **Write the citation block** for each idea×source pairing:
   - Extract the best quote that demonstrates the idea
   - **Attribution**: Name the person who stated the idea. For interview sources, this is usually the guest (the `author` field), not the host. If the host contributes an idea worth extracting, attribute it to them explicitly.
   - Write "What this source adds" — how this specific source's framing is unique or useful
   - Include both original URL and archive relative markdown link
6. **Update the source file**: Fill in `## Key Ideas Extracted` with links to each knowledge entry. Set `status: processed`.
7. **Update `sources/README.md`**: Move to Processed section, fill in "Ideas Extracted" column
8. **Update ai-pm-craft README.md Knowledge Map**: Add new entries under appropriate phase/domain headings
9. **Update ai-pm-craft README.md reading queue**: Move to "Recently Processed"

### Review Reading Queue

Trigger: "What's in my queue?", "what should I read?", "reading status"

1. Query `sources/README.md` for status counts
2. Report: total sources, unread count, reading count, read-but-unprocessed count, processed count
3. List unread sources with their frontmatter `summary` fields — these triage summaries give quick context without reading the full source. Read summaries from source file frontmatter directly (no separate index needed).
4. Highlight any read-but-unprocessed sources (these represent unrealized value)

### Knowledge Gap Analysis

Trigger: "What haven't I covered?", "where are the gaps?", "what topics need more sources?"

1. Review the Knowledge Map headings in ai-pm-craft README.md
2. For each section, assess: How many entries? How well-sourced (single source vs. multi-source)?
3. Identify thin areas: sections with few entries or entries backed by only one source
4. Report gaps and suggest what types of sources might fill them

### Add Organic Technique

Trigger: User describes a technique from their own experience, not from an external source. Typically arrives via inbox (tagged `project: ai-pm-craft` with no source URL) or directly ("add this organic technique to ai-pm-craft").

This **skips the source file step entirely** — there's no external source to archive.

1. **Understand the technique**: Ask clarifying questions if the description is vague. You need enough to write the Summary, How to Apply, and Origin sections.
2. **Check for existing entries**: Search the knowledge base for entries that describe the same or substantially similar idea.
   - **Match found** → Add the user's organic perspective to the existing entry:
     - Add an Origin section alongside the existing Sources section
     - Update `origin` to `both`
     - Enrich Summary or How to Apply if the user's framing adds something new
     - Do NOT create a duplicate entry
   - **No match** → Create new knowledge entry (step 3)
3. **Create knowledge entry** from `templates/knowledge-entry.md` in appropriate `knowledge-base/` subfolder
   - Set `origin: organic`
   - Use the **Origin** section (not Sources): Context, How you use it, Why it works
   - Status starts at `draft` — but organic entries backed by extensive personal experience may start at `solid`
4. **Update ai-pm-craft README.md Knowledge Map**: Add new entry under appropriate heading (or note enrichment of existing entry in progress log)
5. **Note for future**: If external sources are later found that discuss this idea, add a Sources section alongside Origin and update `origin` to `both`

### Mark as Featured

Trigger: User says "this one is important", "feature this", "mark X as featured in ai-pm-craft", "this deserves adoption"

Featured entries are techniques worth championing organizationally — candidates for collateral building and adoption storytelling.

1. **Update frontmatter**: Set `featured: true` on the knowledge entry
2. **Update ai-pm-craft README.md Knowledge Map**: Add **★** marker to the entry so it stands out visually

---

## How to Think About It

**Atomicity matters — err on the side of more entries, not fewer.** One idea per knowledge entry. If a source describes a three-step workflow, each step is likely its own technique with its own rationale and applicability. "This article has a three-step process" should produce three entries, not one entry describing all three steps. The test: does this step have its own *why*, its own situations where it applies or doesn't, its own nuances? If yes, it's a separate entry.

**Decompose workflows into their constituent techniques.** A workflow like "PRD → Task List → Execution" contains three discrete techniques: (1) interactive PRD writing (templatization + follow-up questions), (2) task list generation (observability, stakeholder engagement), (3) stepwise task execution (human-in-the-loop checkpoints). Each has different value, different contexts where it applies, and different lessons. Extract them separately. Cross-reference them via the Related section.

**Decompose frameworks and classification schemes the same way.** When a source presents "three types of X" or "N categories of Y," don't default to one entry describing the whole framework. Each category may have independent properties — its own disruption profile, its own implications, its own actionable guidance. The test: can each category stand alone as something you'd teach someone? If each has its own *characteristics*, its own *so-what*, and its own *what to do about it*, they're separate entries. Example: "Shape/Ship/Sync" is one framework with three categories, but each has a different AI disruption rating, different applicable techniques, and different investment implications — that's three entries, not one. Cross-reference them via Related sections.

**Name entries by what they teach, not where they came from.** "Ryan Carson's three-file system" is a source reference. "Interactive PRD Writing with AI" is a teachable technique. The entry should be recognizable and useful even if you've never read the source.

**Capture the unique mechanism, not just the outcome.** For a technique, the *how* matters as much as the *what*. "Use AI to write PRDs" is too vague. "Templatize PRD generation with rule files that instruct AI to ask clarifying questions before writing" captures the actual insight.

**Watch for portable prompting patterns.** Some techniques are embedded inside larger workflows but are independently valuable — specific enough to use verbatim, broad enough to apply across many contexts. Examples: "Be 100 times more specific" (forces AI past platitudes in *any* domain), "MY job is X, YOUR job is Y" (delineates human/AI responsibility in *any* prompt). These are easy to overlook because they appear as single steps in a bigger workflow. Extract them as their own entries whenever the mechanism is specific and the applicability is broad. The test: could someone use this exact pattern tomorrow in a completely different context and get value from it?

**Screen for change management and adoption.** Sources often embed insights about how leaders drive adoption of AI-native skills alongside the craft techniques themselves. These are easy to overlook because they feel like "context" rather than "content" — but they're independently valuable. Examples: framing AI skills as mandatory for one's next job (urgency creation), running live demos to shift skeptics (show-don't-tell adoption), pairing resisters with early adopters (social proof). When you find these, extract them as knowledge entries under the Change Management & Adoption section. The test: is this about *how to bring others along* rather than *how to do the craft yourself*?

**Screen for horizontal domain insights.** Sources about delivery workflows often embed insights about agent management, knowledge systems, or delivery methodologies that deserve their own entries. A source about "how we built X with AI agents" may contain: (1) lifecycle-phase techniques (Build), (2) a delivery methodology insight (Software Delivery), (3) an agent onboarding pattern (Agent Lifecycle), and (4) a context engineering strategy (Knowledge Management). Extract each into the appropriate horizontal domain. The test: is this about a *portable technique* (→ Practices), a *delivery methodology* (→ Software Delivery), *managing agents over time* (→ Agent Lifecycle), or *knowledge systems* (→ Knowledge Management)?

**Distinguish atomic techniques from domain knowledge.** A single prompting pattern you could use tomorrow in any context → Practices. A named delivery methodology with proponents, tradeoffs, and organizational implications → Software Delivery. A strategy for agent management across a project lifecycle → Agent Lifecycle. A system for curating and accessing shared context → Knowledge Management. When in doubt, start with the more specific horizontal domain — it's easier to generalize later than to untangle a technique from its domain context.

**Lineage is the whole point.** The knowledge entry is the synthesis, but its value comes from being able to trace back to specific quotes and demonstrations in specific sources. Don't skimp on citations.

**Don't duplicate, enrich.** When a new source discusses an idea that already has a knowledge entry, resist creating a new entry. Instead, add the new source's perspective to the existing entry. The entry gets richer over time.

**Status reflects reality.** If you extracted ideas but didn't create proper knowledge entries, the source isn't "processed" — it's "read." Be honest about status.

**The Knowledge Map is the consumption layer.** Individual entries are powerful but hard to browse. The ai-pm-craft README's Knowledge Map is what makes the knowledge base navigable. Every new or moved entry must be reflected there.

**Respect the taxonomy.** Place entries in the most specific applicable phase/component. Use `meta/taxonomy.md` as the canonical reference. If it doesn't fit, flag it for review.

---

## Watch Out For

**Overly broad entries.** "AI Product Management Best Practices" is too broad. "Pre-mortem for AI Feature Launches" is right. If you can't explain the idea in 2 sentences, it's probably multiple ideas. Similarly, "PRD-Driven AI Development" that covers three distinct techniques is too broad — split into "Interactive PRD Writing," "Task List Generation for Observability," and "Stepwise Task Execution." The same applies to classification frameworks: "Shape/Ship/Sync PM Work Model" bundles three categories with different disruption profiles and different actionable guidance — split into "Shape the Product," "Ship the Product," and "Sync the People."

**Under-extracting from rich sources.** A source with three workflows likely contains 5-8 discrete ideas, not 3. Look for: the overarching insight *and* each individual technique *and* any broadly applicable sub-techniques (e.g., a prompting pattern that works beyond the specific use case described).

**Missing lineage.** Every claim in a sourced knowledge entry should trace to a source. For organic entries, the Origin section is the lineage — make sure it explains where the idea comes from in practice.

**Attribution precision.** In interview sources, ideas belong to the person who stated them. Don't attribute a guest's insight to the host's show. When the host does contribute a genuine idea, call that out explicitly.

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
- **Categorization**: Notes appear in `sources/README.md` like any other source, making them visible during knowledge gap analysis and future processing.
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

## Taxonomy Reference

**Full reference**: `domains/professional-development/ai-pm-craft/meta/taxonomy.md`
**Lifecycle framework**: `domains/professional-development/ai-pm-craft/meta/lifecycle-framework-v2.md`

Three domains, six lifecycle phases, four horizontal domains:

| Domain | Folder | Classification test |
|---|---|---|
| **Product Lifecycle** | `knowledge-base/product-lifecycle/{phase}/` | Augments a specific lifecycle phase |
| **Horizontal** | `knowledge-base/horizontal/{horizontal_domain}/` | Cross-cutting skill, methodology, or knowledge area |
| **AI Adoption** | `knowledge-base/ai-adoption/` | About bringing others along or changing how teams/orgs work |

**Lifecycle phases**: Discover → Frame → Shape → Build → Release → Measure

**Horizontal domains**:

| Horizontal Domain | Slug | Classification test |
|---|---|---|
| Practices | `practices` | Atomic, portable technique applicable across 3+ phases |
| Software Delivery | `software-delivery` | Delivery methodology or how software gets built |
| Agent Lifecycle | `agent-lifecycle` | Managing agents as ongoing participants |
| Knowledge Management | `knowledge-management` | Shared human-agent context systems |

Each lifecycle phase has named components (see `meta/taxonomy.md` for the full list with slugs). Use the most specific component that fits. If the entry fits the phase but not a specific component, omit `component` from frontmatter.

**Placement rules**:
1. Most specific component wins (lifecycle) or most specific horizontal domain wins (horizontal)
2. Phase-level is OK when no component fits
3. Cross-phase entries (3+ phases) → horizontal (then determine which horizontal domain)
4. Horizontal entries can optionally reference a `lifecycle_phase` for dual attribution
5. Screen every source for adoption insights alongside craft techniques
6. Screen every source for horizontal domain insights (delivery methodologies, agent management, knowledge patterns)
