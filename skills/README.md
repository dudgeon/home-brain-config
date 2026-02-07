# Skills

Skills encode **judgment patterns**, not mechanical checklists. A good skill captures:
- **When** to apply it (triggers and recognition)
- **Why** certain approaches work better (reasoning)
- **How** to think through edge cases (judgment)
- **What** good output looks like (examples)

Skills are for workflows that benefit from consistent judgment, not for simple procedures that don't need guidance.

---

## Current Skills

| Skill | Purpose |
|-------|---------|
| dictation-processing.md | Transform unstructured input into structured knowledge |
| person-intelligence.md | Handle multi-person interactions and entity updates |
| inbox-triage.md | Route captured items to appropriate homes |
| template-propagation.md | Update derived documents when templates change |
| entity-verification.md | Clarify vague entities by researching primary sources |
| newsworthy-events.md | Link to trusted sources for newsworthy events affecting family |

---

## When to Create a New Skill

A workflow deserves a skill when:
1. It requires **judgment calls** that benefit from documented reasoning
2. It has **edge cases** that are easy to forget
3. It spans **multiple steps** that should happen together
4. It would benefit from **examples** of good vs. poor execution

Don't create skills for:
- Simple, obvious procedures
- One-off tasks
- Things that vary too much to standardize

---

## Skill Structure

Skills should read like guidance from an experienced colleague, not a software spec:

```
# [Skill Name]

## When This Applies
[Recognition patterns — how to know this skill is relevant]

## The Goal
[What good looks like, in plain language]

## How to Think About It
[Judgment guidance, reasoning, principles]

## Watch Out For
[Common mistakes, edge cases, things that seem right but aren't]

## Examples
[Concrete examples of good execution]
```

---

## Skill-Worthy Pattern Detection

Watch for these patterns that might become skills:

**High Priority (suggest after 2-3 uses):**
- `/process-inbox` - Systematic inbox processing with filing decisions
- `/weekly-review` - Generate summaries by domain with action items
- `/project-start` - Create new project with all standard setup
- `/extract-actions` - Find all pending tasks across domains
- `/meeting-prep` - Pull relevant context before a meeting

**Medium Priority (suggest after 5+ uses):**
- `/connect` - Find cross-domain connections and patterns
- `/capture` - Guided quick capture with smart domain routing
- `/monthly-review` - Deep analysis with insights and trends
- `/archive-project` - Move completed projects to archive with cleanup

---

## Don't Over-Skill

Avoid suggesting skills for:
- One-off requests
- Simple single-step tasks
- Things that vary significantly each time
- Workflows still being refined
- Tasks better handled by natural conversation
