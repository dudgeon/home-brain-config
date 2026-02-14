# Fact Retention & Context Management

This document contains the logic for detecting, routing, and storing important facts. This is working logic—preserve it exactly.

---

## Core Principle

**ALWAYS retain important facts mentioned in conversation and store them in appropriate context locations.**

When the user mentions important information (names, dates, preferences, relationships, etc.), you MUST assess importance and retention needs.

---

## Retention Process

### 1. Detect Important Facts

When user mentions information, assess if it's important using these criteria:
- **Identity information**: Names, ages, relationships, roles
- **Dates & milestones**: Birthdays, anniversaries, memorial dates, deadlines
- **Preferences & patterns**: Likes, dislikes, routines, habits
- **Locations & addresses**: Home, work, frequent destinations, contacts
- **Goals & priorities**: Stated objectives, values, aspirations
- **Constraints**: Limitations, requirements, boundaries
- **Relationships**: Connections between people, places, concepts
- **Changes**: New information that updates previous understanding
- **Decisions**: Outcomes of discussions, rationale, who decided, what was considered

### 2. Determine Retention Location

Store facts in the appropriate context file (all in `/context/`):
- `family.md` - Family members, relationships, dates (note: detailed friend profiles go in `domains/people/`, not here)
- `travel.md` - Recurring trips, addresses, travel preferences
- `workshop.md` - Equipment, capabilities, techniques, active projects
- `professional.md` - Learning goals, projects, interests
- `food-drink.md` - Preferences, favorites, explorations
- `etsy.md` - Shop details, products, workflow
- `board.md` - Organization, role, initiatives
- `core.md` - Identity, work style, communication preferences
- `questions.md` - Areas needing more information

### 3. Update Context Immediately

- Don't wait to batch updates - update context files as facts emerge
- Mark updates with date: `*Updated: YYYY-MM-DD (brief description of change)*`
- If creating new sections, maintain consistent format
- Cross-reference between files when appropriate

### 4. Handle Conflicting Information

If new information conflicts with existing context:
- **STOP and ask clarification question** before proceeding
- Example: "You mentioned having a son earlier, but now said you have no kids. Can you clarify?"
- Never silently overwrite potentially correct information
- Document resolution: `*Corrected: 2026-01-19 (was X, now confirmed as Y)*`

---

## Examples of Important Facts to Retain

**DO Retain**:
- "My brother joins us for [holiday]" → family-context.md
- "[Family member]'s birthday is [date]" → family-context.md (Important Dates)
- "We stay at [hotel] for [trip]" → travel-context.md (trip patterns)
- "Kids love [activity] now" → family-context.md (interests)
- "[Equipment] has [issue]" → relevant domain context file
- "Friend [name] hosts annual party" → family-context.md (for event reference); person file in domains/people/

**DON'T Retain**:
- Trivial ephemeral details ("I'm drinking coffee right now")
- One-time logistical minutiae ("the package arrived at 3pm")
- General conversation filler
- Obvious facts already well-documented

---

## Context Enrichment During Conversations

When user naturally mentions information that fills gaps:
1. Recognize it relates to existing context area
2. Update appropriate context file
3. Mark in context-gathering-questions.md as answered
4. Add new follow-up questions that emerge

---

## Context Gathering - When New Areas Open

**ALWAYS: When conversation opens a new context area, add follow-up questions to context-gathering-questions.md**

Indicators that a new area opened:
- User mentions a domain/topic with insufficient current context
- You have questions but don't want to interrupt flow
- You identify gaps in understanding that would improve future assistance

Process:
1. Recognize the gap (e.g., "Etsy shop mentioned but no product details")
2. Add section to context-gathering-questions.md with 3-5 specific questions
3. Categorize priority: High (immediate usefulness), Medium (enriches context), Low (nice to have)
4. Reference during idle time or when user asks "what else do you need?"

---

## "Always" Rules Enforcement

When user says "you should always do X":
1. Document the rule in claude.md (this file)
2. Create mechanism to enforce it (checklist, reminder, process)
3. Test that it will consistently happen
4. Add to relevant workflows or best practices

---

## Context Gathering Questions

See `context/questions.md` for:
- Areas where more context would be helpful
- Questions to ask during idle time
- Priority levels for different contexts
- Pattern recognition opportunities

**Usage**: When between tasks or user asks "what else do you need?", reference this file for productive context-gathering questions.
