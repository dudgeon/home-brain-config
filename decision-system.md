# Decision Documentation - Proactive Detection

This document describes the ADR (Architectural Decision Record) workflow and how to detect decision-worthy moments.

---

## Core Principle

**Architectural and strategic decisions should be documented as they're made, not after the fact.**

This system follows the Architectural Decision Record (ADR) pattern. When significant decisions are being made, Claude should proactively identify them and offer to document them.

---

## What Qualifies as a Decision?

A decision is worth documenting if it meets **2 or more** of these criteria:

1. **Affects system structure** - Changes how things are organized or architected
2. **Has alternatives** - Multiple viable approaches were considered
3. **Future impact** - Will affect future work or constrain future options
4. **Non-obvious** - The reasoning isn't immediately apparent
5. **Precedent-setting** - Could apply to similar situations later
6. **Reversibility cost** - Would take significant effort to undo
7. **Learning value** - Could benefit another instance or future review

---

## Decision Categories

Decisions span multiple contexts - system, life, and domain:

All decisions go in `/decisions/` with appropriate category metadata.

**Templates**:
- `decision.md` - Standard format for most decisions
- `decision-quick.md` - Lightweight format for smaller choices

---

## Decision Levels

**Major Decisions** (must document):
- System: Architecture changes, major tool adoption
- Life: Career transitions, housing, education philosophy
- Domain: High-stakes choices with long-term impact
- Cross-domain: Decisions affecting multiple life areas

**Medium Decisions** (should document):
- System: Templates, conventions, process definitions
- Life: Medium-term commitments, significant purchases
- Domain: Project approaches, resource allocation

**Minor Decisions** (optional documentation):
- Use quick template if beneficial
- Otherwise skip documentation (avoid decision fatigue)

---

## User's Decision-Making Preference

**IMPORTANT: "Work it out conversationally, then document the thinking after"**

User prefers to:
1. Discuss options and trade-offs conversationally first
2. Make the decision through natural dialogue
3. THEN create documentation once the decision is made

**Don't**:
- Create big decision briefs upfront that need approval
- Present formal decision documents before discussing
- Over-formalize the exploration phase

**Do**:
- Discuss options naturally in conversation
- Once decided, THEN document comprehensively
- Make documentation thorough but only AFTER decision is made

---

## Claude's Detection Process

**During Conversations:**

1. **Listen for decision indicators**:
   - "Should we..." or "How should we..."
   - Multiple options being discussed
   - Trade-offs being evaluated
   - "Let me think about..." or "I'm not sure if..."
   - Rejecting one approach for another

2. **Recognize the decision point**:
   - Pause and identify: "This is a decision moment"
   - Assess: Does it meet 2+ of the 7 criteria above?
   - Determine level: Major, Medium, or Minor

3. **Surface the decision** (conversationally):
   - For **Major**: "This seems like an important decision. Let's talk through the options."
   - For **Medium**: "I see a few different ways to approach this..."
   - For **Minor**: Don't interrupt flow
   - **Avoid**: Jumping straight to formal decision documentation

4. **Document AFTER decision is made**:
   - Once user decides: "That makes sense. Let me document this decision."
   - Choose the appropriate template based on scope
   - Save to `/decisions/NNN-name.md`
   - Include enough context for future review
   - Document comprehensively, but only after the conversation settles on an approach

5. **Offer timeline entry**:
   - After documenting the decision, offer to add a timeline entry linking to the decision record
   - Decisions that shape project direction are always changelog-worthy (per [communication.md](communication.md) Changelog Awareness)
   - Brief prompt: "Should I add this decision to [project]'s timeline?"

---

## Choosing the Right Template

**decision.md** (1-2 pages):
- Standard format for most decisions
- Works for system, life, or domain decisions
- Options, rationale, success criteria

**decision-quick.md** (< 1 page):
- Smaller decisions with light structure
- Ultra-lightweight, minimal overhead
- Just enough to preserve reasoning

---

## Examples of Decision Moments

**DO Document - System**:
- "Should we use skills now or wait?" ✓ (System-001)
- "Should we integrate with Obsidian or stay editor-agnostic?"
- "Weekly review: manual or automated?"
- "How should we structure domains for family?"

**DO Document - Life**:
- "Should we renovate or move to a new house?"
- "Is this the right time for a career transition?"
- "What education philosophy should guide our family?"
- "How should we allocate time between work, family, and personal growth?"

**DO Document - Domain**:
- "Should I DIY the deck or hire a contractor?" (DIY)
- "Should we expand product line A or B?" (Etsy)
- "Should we switch to monthly or quarterly board meetings?" (Board)
- "Should [child] do competitive or recreational soccer?" (Family)

**DON'T Document**:
- "Should this note go in DIY or Family?" (Too small)
- "Should we use dashes or underscores?" (Unless setting a convention)
- "Should we add an emoji here?" (Trivial)
- "What should we have for dinner?" (Unless it's a dietary philosophy decision)

---

## Keeping Decisions Visible

- Link decision briefs in relevant domain READMEs
- Reference decisions in claude.md when they affect instructions
- Update `decisions/README.md` with a log of all decisions
- Tag relevant notes with decision references: `See ADR-001`

---

## Review and Revision

- Decision briefs are living documents - update as we learn
- Mark status: Proposed → Decided → Implemented → Validated or Superseded
- When a decision is proven wrong, document why and what replaced it
- Encourage periodic review (monthly or quarterly)
