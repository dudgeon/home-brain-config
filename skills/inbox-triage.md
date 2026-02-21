---
name: inbox-triage
description: Process and route inbox items to appropriate domains. Use when user says "process inbox", "triage inbox", "clear inbox", or after completing tasks to check for accumulated items.
---

# Inbox Triage

Processing accumulated items in `inbox/` — deciding what to keep, where it goes, and what to discard.

---

## When This Applies

- **After completing any task or interaction** (see communication.md → "When Finishing a Task")
- User asks to "process inbox"
- Inbox has accumulated items that need routing
- During weekly/monthly review

---

## The Goal

Inbox zero, with everything either:
- Filed in the right domain with proper metadata
- Merged into existing notes
- Discarded as no longer relevant

---

## How to Think About It

**Inbox is temporary storage, not a domain.** If something has been in inbox for weeks and still doesn't have a home, question whether it's worth keeping.

**Read before routing.** Understand what the note actually is before deciding where it goes. The filename might be misleading.

**Check for existing notes.** Before creating a new note in a domain, search for related content. Often the right action is to merge or append, not create.

**Add metadata as you file.** Every note leaving inbox should have proper frontmatter (created, updated, tags, status). This is the tax for leaving inbox.

**Some things should be discarded.** Not everything captured deserves permanent storage. If a note is:
- Outdated and no longer relevant
- Already captured elsewhere
- Too vague to be useful
- A fleeting thought that didn't develop

...then delete it. The system shouldn't accumulate cruft.

---

## Watch Out For

**The "just in case" trap.** Keeping vague notes because "maybe someday" leads to bloat. Be willing to discard.

**Creating orphans.** Notes filed without links are hard to rediscover. Connect to related content.

**Domain ambiguity.** If you genuinely can't decide where something belongs, it might be:
- Actually two separate notes
- Missing context (ask user)
- A sign the domain structure needs adjustment

---

## Triage Questions

For each inbox item, ask:
1. Is this still relevant? (If no → delete)
2. Does a note for this already exist? (If yes → merge)
3. Which domain owns this? (File there)
4. What metadata does it need? (Add frontmatter)
5. What should it link to? (Add links)

---

## Domain & Entity Detection

Every inbox item is a signal. Before routing, ask:

1. **New entity?** Does this introduce a person, project, organization, recurring event, or topic not yet in the system?
   - Person → create from `templates/person.md` in `domains/people/` (or `domains/family/[child]/friends.md` for kids' friends)
   - Project → create from `templates/project.md` in the relevant domain
   - Organization → create a note in the relevant domain
   - New life area → consider whether a new domain folder is warranted (rare — discuss with user)

2. **Existing entity update?** Does this add information to something already tracked?
   - Update the existing note (add timeline entry, merge details, update status)
   - Update context files if cross-domain facts are involved (`/context/*.md`)
   - Cross-post to related entity files (e.g., a person interaction → update all involved person files)

3. **Connections?** What existing notes should link to/from this content?

---

## Entity Timeline Updates — Hub and Spoke

When a note references people or projects, update their entity files using a **hub-and-spoke** pattern:

- **Hub**: The archived inbox note in `inbox/_archive/` — this is the authoritative record
- **Spokes**: Person files and project notes each get a lightweight timeline entry pointing back to the hub

### Person File Updates

For each person mentioned, add a reverse-chronological timeline entry in their file (`domains/people/[name]/README.md` or `domains/family/[name]/`):

```markdown
### YYYY-MM-DD — Brief description of interaction

One to three sentence summary of what was discussed relevant to this person.
Not a transcript — a useful index entry.

Source: [inbox note](relative/path/to/inbox/_archive/YYYY-Www/note.md)
```

### Project Updates

For each project discussed, add a similar timeline entry:

```markdown
### YYYY-MM-DD — Brief topic [progress]

One-liner on what was discussed or decided. Detail lives in the inbox note.

Source: [inbox note](relative/path/to/inbox/_archive/YYYY-Www/note.md)
```

### Cross-Links Between Spokes

Don't link person ↔ project for every interaction. The inbox note is the natural join point. Only add direct person ↔ project links when capturing a **durable relationship** — that's an entity fact, not a meeting artifact.

---

## Domain Emergence and Corpus Backfill

During triage, you may notice items clustering around a topic that doesn't have a domain. This is a **domain emergence signal**.

### Detection

- Multiple inbox items routing to the same parent domain but sharing a distinct sub-theme
- A single item introducing a clearly distinct life area with no existing home
- Repeated entity references that don't fit neatly into current domain structure

### Response

1. **Propose the new domain** — explain the clustering pattern, suggest a name and location. Number the evidence. Check [conventions.md](../conventions.md) for the current domain list. Get user approval before creating.
2. **Create the domain** — README.md (with `## Timeline` section), any initial notes from the triggering items.
3. **Bounded backfill scan** — search existing notes for mentions of the new domain's key concepts. Present findings as a numbered list:
   - Notes that should **backlink** to the new domain (add a link)
   - Notes that might **belong** in the new domain (propose relocation)
   - Context files that should **reference** the new domain
4. **Execute approved changes** — only after user confirms which items to act on.

Backfill runs **once at domain creation**, not continuously.

---

## Source-Synthesis Domain Detection

Some inbox items are destined for domains that use the [domain-source-synthesis](domain-source-synthesis.md) pattern. When triaging:

1. **Check for domain/project tags** — if an item is tagged for a specific domain (e.g., `project: ai-pm`), check whether that domain has `pattern: domain-source-synthesis` in its README
2. **Hand off to the pattern** — route to domain-source-synthesis (or a domain-specific extension like [ai-pm-source-processing](ai-pm-source-processing.md)) for capture rather than filing generically
3. **Source vs non-source** — items with a source URL route to the capture workflow; items describing personal experience route to the organic idea workflow

When in doubt about whether an item is for a source-synthesis domain, ask rather than processing generically.

---

## Email Forward Handling

Forwarded emails have a lower signal-to-noise ratio than user-authored notes. Don't treat them like curated content — most of the text is filler.

### Step 1: Classify the item

For each inbox item, determine its type:

- **User-authored note** (no forwarded email headers) → process normally using the workflow below
- **Annotated email forward** (user wrote instructions above the forwarded content) → follow the user's instructions to process. The annotation is the signal; the email is supporting context.
- **Unannotated email forward** (forwarded email with no user guidance) → **stop and ask before processing** (see Step 2)

### Step 2: Ask before processing unannotated forwards

Summarize the email in 2-3 bullets, then ask:

> Here's what's in this email: [summary bullets]
>
> What do you want captured?
> 1. Specific facts or contact info → file in the relevant domain
> 2. Upcoming dates/events → add to relevant calendar/tracking note
> 3. Store as reference → file the email as-is in the relevant domain
> 4. Nothing useful → discard

Wait for a response before processing.

### Step 3: Default events/dates as uncommitted

Any dates or events extracted from third-party emails (newsletters, eblasts, group communications) should be marked as **uncommitted/available** by default — not as the user's or family member's plans.

**Exception**: If entity search during triage finds evidence of a prior commitment (e.g., a note saying "Owen signed up for Sea Base"), mark as committed.

---

## Enrichment Pass — CriticMarkup Annotations

Before decomposing and routing, scan the raw note for opportunities to enrich it from **existing home-brain content**. This turns the archived note into a self-contained record — what the user said *plus* what the system already knew.

### When to Annotate

- The note contains a question that existing domain files, context files, or decision records can answer
- A vague reference ("the vendor", "that project") can be resolved to a specific entity with a link
- A stated fact conflicts with or confirms something already captured — worth noting the connection

### When NOT to Annotate

- The question requires web research or speculation — leave it unanswered (or flag as a task)
- The annotation would just restate what the note already says
- The note is already well-structured and complete

### CriticMarkup Convention

Use CriticMarkup comment syntax, dated and attributed:

```
{>>YYYY-MM-DD @claude: Annotation text. See [entity](relative/path/to/file.md)<<}
```

Place annotations **inline, immediately after** the relevant text. Do not move or modify the original text.

**Example:**

```markdown
I need to check on the swim team registration deadline
{>>2026-02-15 @claude: NCC Sharx registration opens in March. See [ncc-sharx](domains/family/shared-activities/ncc-sharx/README.md)<<}
```

### Enrichment Boundary

Only annotate from **existing home-brain content** — domain files, context files, decision records, person files. Never from web searches, speculation, or general knowledge. If the system can't answer it, leave it alone or create a task to research it.

---

## Processing Inbox Workflow

When processing inbox (whether prompted or as post-task check):
1. Read each note
2. **Classify the item** — user note, annotated forward, or unannotated forward (see Email Forward Handling above)
3. **Enrichment pass** — annotate answerable questions and resolvable references with CriticMarkup (see above)
4. **Check for source-synthesis domains** — if tagged for a domain with `pattern: domain-source-synthesis`, hand off to that domain's processing skill (see Source-Synthesis Domain Detection above). **After the domain skill completes, return here and continue from step 5** — the handoff does not skip the archive step (step 12)
5. Detect new or existing entities (see above)
6. **Update entity timelines** — add hub-and-spoke entries for people and projects referenced in the note (see above)
7. Add proper metadata
8. Create links to related notes
9. Move to appropriate domain folder (or merge into existing note)
10. **Check for domain emergence** — if routing reveals clustering around an untracked topic, propose new domain + backfill (see above)
11. Annotate the original with triage metadata
12. Archive processed item to `_archive/YYYY-Www/`

---

## Post-Triage Item Annotation

When filing an inbox item, annotate it with triage metadata before archiving:

```yaml
---
# Original frontmatter preserved above
triage_status: filed|merged|discarded
triage_date: YYYY-MM-DD
triage_action: "Brief description of what was done"
triage_destination: "path/to/destination.md"
---
```

This creates an audit trail: what came in, what happened to it, where it went.
