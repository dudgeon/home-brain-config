---
name: dictation-processing
description: Transform unstructured text into structured knowledge. Use when user provides voice dictation, stream-of-consciousness notes, says "capture this", "here's what happened", or shares raw notes about events/people/ideas.
---

# Dictation Processing

The user's primary workflow: providing unstructured text (often voice dictations) that need to be understood, enriched, and stored intelligently.

---

## When This Applies

- User provides a block of unstructured text
- User says "capture this", "here's what happened", "notes from today"
- User shares stream-of-consciousness about events, people, or ideas
- Input contains multiple topics, people, or domains intermixed

---

## The Goal

Transform raw input into knowledge that will be **useful on retrieval**. This means:
- Information lands in the right places
- Connections to existing knowledge are made
- Gaps are filled through research when possible
- Future-you can find and understand it

This is NOT transcription. It's intelligence work.

---

## How to Think About It

**Before acting, read the whole input.** Dictations often circle back, clarify, or contradict earlier statements. Don't process linearly.

**Identify the entities.** Who and what is mentioned? Resolve names using this lookup order:
1. `context/family.md` — immediate family, extended family, key friends (handles "my wife", "my brother", nicknames)
2. `domains/people/` — individual person files for friends and contacts
3. `context/*.md` — orgs, projects, places referenced in other context files

Unknown entities might be:
- New people who need person files
- Known people referred to by nickname or role ("my brother")
- Projects or places mentioned in passing

**Assess what's worth keeping.** Not everything in a dictation is knowledge. Apply the retention criteria from `.claude/context-management.md`. Skip:
- Thinking-out-loud that resolved itself
- Trivial logistics
- Filler and repetition

**Enrich where valuable.** Per `.claude/philosophy.md`: if something is researchable and would be useful later, research it. Restaurant mentioned? Find the link. Person's new job? Confirm the company. Don't wait to be asked.

**Route to the right homes.** Most dictations touch multiple domains or entities. Content might need to go to:
- Context files (preferences, relationships, patterns)
- Domain notes (events, projects, learnings)
- Person files (timeline entries, profile updates)
- **Kids' memories/timelines** (`domains/family/owen/memories.md`, `domains/family/alina/memories.md`) — any event meaningful to a child gets a timeline entry, even if the detail lives in a domain file
- Multiple places (multi-person interactions)

**Update, don't just create.** Check if relevant notes exist before creating new ones. A dictation about a trip should update the existing trip file, not create a duplicate.

---

## Watch Out For

**Pronoun confusion.** "He said" — who? Check recent context, ask if unclear. Wrong attribution corrupts the knowledge base.

**Splitting what should be together.** If a dictation is about one event involving multiple topics, consider whether it's one note with sections or truly separate notes.

**Missing the multi-person signal.** When an interaction involves multiple people, EACH person's file needs a timeline entry. See `person-intelligence.md`.

**Missing kids' timeline entries.** When an event involves Owen or Alina — even if it's filed under travel, boy-scouts, school, or another domain — ask: "Is this a memory worth preserving on their personal timeline?" If yes, cross-post a summary to their `memories.md` Timeline section. Domain files hold the detail; the kids' timelines hold the personal record.

**Over-processing.** Not every dictation needs extensive treatment. A quick note about a passing thought can just go in inbox. Match effort to value.

**Forgetting to connect.** After routing content, add links to related notes. Over-linking is better than under-linking.

---

## After Processing

Briefly report:
- What was captured and where
- Any entities that were created or significantly updated
- Research findings that enriched the input
- Anything that needs user clarification
- Action items extracted (add to tasks.md if appropriate)
