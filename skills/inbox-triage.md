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
   - Organization → add to `.claude/entity-index.md` and create a note in the relevant domain
   - New life area → consider whether a new domain folder is warranted (rare — discuss with user)

2. **Existing entity update?** Does this add information to something already tracked?
   - Update the existing note (add timeline entry, merge details, update status)
   - Update context files if cross-domain facts are involved (`/context/*.md`)
   - Cross-post to related entity files (e.g., a person interaction → update all involved person files)

3. **Connections?** What existing notes should link to/from this content?

## Project-Tagged Items

Some inbox items are pre-tagged for specific learning projects. These bypass generic triage and route to specialized processing.

### Detecting Project Tags

Check frontmatter for `project:` field. Currently recognized projects:

- **`project: ai-pm-craft`** → Route to the `ai-pm-craft-source-processing` skill. This is source material (article, video, tweet thread, etc.) for the AI PM Craft learning project. Do NOT process generically — instead, follow the "Add New Source" workflow in `.claude/skills/ai-pm-craft-source-processing.md`, which handles source file creation, index updates, and reading queue management.

### How Items Get Tagged

Users may tag inbox items themselves, or mention the project when dropping something in:
- Frontmatter `project: ai-pm-craft`
- Tags include `ai-pm-craft` or `ai-pm-craft-source`
- Note content mentions "for the pm project", "ai pm craft", or similar

When in doubt about whether an item is a learning source for a known project, ask rather than processing generically.

---

## Processing Inbox Workflow

When processing inbox (whether prompted or as post-task check):
1. Read each note
2. **Check for project tags** — if tagged for a specific project, hand off to that project's processing skill (see above)
3. Detect new or existing entities (see above)
4. Add proper metadata
5. Create links to related notes
6. Move to appropriate domain folder (or merge into existing note)
7. Update INDEX.md if significant
8. Update `.claude/entity-index.md` if new entities were created
9. Archive processed item to `_archive/YYYY-Www/`
