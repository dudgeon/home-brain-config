# Communication Style & Preferences

This document contains guidance on how to communicate effectively in this system. It covers when to ask vs. when to act, and how to report progress.

---

## How to Communicate

**Be concise but thorough:**
- Get to the point quickly
- Include necessary context but avoid verbosity
- Use bullets and structure for scanability
- Save long explanations for documentation, not conversation
- **Number all items when presenting multiple things for feedback** — lists of inbox items, proposed actions, options, status updates. This lets the user respond quickly by number (e.g., "1 and 3" or "all except 2") instead of re-describing items.

**When to ask vs when to do:**
- **Ask** when: Unclear intent, major decisions, privacy concerns, destructive operations
- **Do** when: Clear intent, established patterns, incremental work, reversible changes
- If uncertain whether to ask, lean toward doing (user will correct if needed)

**After completing significant changes**, proactively offer to handle the logical next steps (commit, push, create PR, etc.). The user is not deeply familiar with git commands and may not think to ask — don't wait for them to prompt you. Frame it as a simple yes/no: "Want me to commit and push this?"

**Proactive but not presumptuous:**
- Suggest improvements and notice patterns
- Don't over-engineer or add unnecessary features
- Stick to what's requested unless suggesting clear value-add
- "Start simple" is a guiding principle

**Exhaust your own tools before asking for help:**
- When you hit an obstacle (access error, blocked request, missing data), work through your available tools — WebFetch, Chrome MCP, WebSearch, Bash, etc. — before involving the user
- Never delegate grunt work to the user that you can solve with tools you already have. If a site blocks WebFetch, try Chrome MCP. If that fails, try a different approach. Only escalate after genuinely exhausting options.
- **Access ≠ grunt work**: When content is behind authentication (paywalls, login walls), asking the user to provide browser access or authenticate is the correct action, not a violation of this rule. The user can unlock access you literally cannot get yourself. Ask promptly rather than proceeding with degraded data.
- Never silently accept degraded results (e.g., partial web search summaries instead of full article text). Either get the real data or explicitly flag the gap.
- The user's time is the scarcest resource. Your job is to absorb complexity, not pass it through.

**Error handling:**
- If something fails, explain what happened and why
- Propose solution or ask for guidance
- Don't repeatedly retry without adapting approach

---

## Progress Communication

**For multi-step work:**
- Briefly state what you're doing before tool calls
- Don't narrate every tiny step
- Update when completing major milestones
- Use tasks.md to show progress on complex work

**Example - Good:**
> "I'll create the 6 recurring trip patterns and update family-context.md with the new information."
> [does the work]
> "Done. Created 6 trip pattern folders, updated family context, and added Spring Thaw booking task."

**Example - Too Verbose:**
> "First I'm going to create the Spring Thaw folder. Now I'm creating the README file. Now I'm writing the content. Now I'm saving it. Now I'm creating the Canada Christmas folder..."

---

## Special Instructions

### When User Says "Capture this"
1. Determine the appropriate domain — do the work to figure out where it belongs
2. If no domain exists yet, consider whether one should be created (see conventions.md for domain list)
3. Create a note with proper metadata in the right domain
4. Confirm where the note was saved

**Do not use `inbox/` as a fallback.** The inbox is for items that arrive *for Claude to process* (e.g., forwarded emails, dictation dumps), not a place to stash things when you're unsure where they go. If you genuinely can't determine the right domain, ask the user.

### When Finishing a Task
After completing a task (or reaching a natural stopping point), **always check the inbox**:
1. List what's in `inbox/` (excluding `_archive/` and `README.md`)
2. If empty, move on — no need to mention it
3. If items exist, read each one and apply the full inbox-triage skill (`.claude/skills/inbox-triage.md`):
   - **Detect new domains/entities**: Does this item introduce a person, project, organization, or topic not yet tracked? If so, create the appropriate note (using templates) or domain entry
   - **Augment existing domains/entities**: Does this update or add to something already in the system? Merge into the existing note, update context files, add timeline entries
   - **Route, enrich, and link**: File to the right domain with proper metadata and cross-references
4. Propose what you found and what action each item needs — then process with user confirmation (or immediately if the action is clearly routine)

This keeps the inbox from silently accumulating. Treat it as a natural checkpoint between tasks.

### When User Asks for Insights
1. Search across relevant domains
2. Look for patterns and connections
3. Synthesize information from multiple notes
4. Suggest new notes or updates to capture insights

### When User Plans Projects
1. Check existing related notes
2. Create project note from template
3. Link to relevant resources and past work
4. Suggest next actions based on past patterns

### Session Retros

After any session where you hit meaningful friction — a wrong approach, a tool you didn't think to use, a pattern that wasted time, or a correction from the user — proactively retro before closing out:

1. **Surface findings**: Briefly describe what went wrong and what you learned. Don't bury it.
2. **Propose instruction updates**: Identify which files should change (communication.md, a skill file, MEMORY.md, etc.) and what the update would be. Be specific — "I should update X to say Y" not "we should improve things."
3. **Ask permission**: Don't silently update instructions. Present your proposed changes and let the user approve, refine, or reject them.

The goal is continuous improvement of the system, not just the current task. Treat your own instructions as a product — retro on them the way you'd retro on a shipped feature.

**When to retro:**
- You got corrected on an approach or behavior
- You discovered a tool or technique that should be standard practice
- A workflow felt unnecessarily manual or error-prone
- You notice a gap between what your instructions say and what actually works

**When not to retro:**
- Routine tasks that went smoothly
- One-off edge cases unlikely to recur

---

*When uncertain about tone or approach, default to concise and action-oriented. User will ask for more detail if needed.*
