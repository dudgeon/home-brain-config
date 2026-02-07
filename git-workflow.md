# Git and Version Control Procedures

This document describes the git workflow for home-brain.

---

## Repository Structure

**Persistent Branch**: `claude/setup-second-brain-Fgt1a`
- This is your **single source of truth**
- All completed work lives here
- Always the most current, consolidated state

**Session Branches**: `claude/<task-description>-<sessionID>`
- Each Claude Code session works on a feature branch
- Session ID restriction means Claude can only push to its own branch
- After session completes, you manually merge the PR to consolidate

---

## Simplified Workflow

**For most sessions (quick edits, note capture, simple tasks):**
1. Work on session's branch
2. Commit changes with clear message
3. Push to session branch: `git push -u origin claude/<task>-<sessionID>`
4. **USER ACTION REQUIRED**: Manually merge PR on GitHub to consolidate into `claude/setup-second-brain-Fgt1a`

**For querying/reading (no changes):**
- Just work on the persistent branch - no push needed

**Key principle**: Accept that sessions create branches, but keep ONE persistent branch updated through manual PR merges. This is a limitation of Claude Code's git integration, not how personal repos should ideally work.

---

## End of Session Protocol

**Claude MUST do this at the end of every session with changes:**

1. Verify all work is committed and pushed to the session branch
2. **EXPLICITLY REMIND USER**:
   - "There's a PR for `claude/<task>-<sessionID>` that needs to be merged"
   - "Please merge this PR on GitHub to consolidate work into `claude/setup-second-brain-Fgt1a`"
   - Include the PR link if available from the push output
3. Don't end the session until this reminder is given

**Why this matters:**
- Without merging PRs, work fragments across branches
- The persistent branch becomes stale
- Future sessions may start from outdated state
- Knowledge base becomes inconsistent

---

## Commit and Push Discipline

**ALWAYS: Commit and push immediately after completing significant work**

This prevents loss of work and keeps the repository current across sessions.

**When to commit:**
- After completing a major task (creating trip patterns, decision docs, etc.)
- After significant updates to multiple files
- Before ending a work session
- When stop hook indicates uncommitted changes

**Don't commit:**
- While actively working through a multi-step process
- Incomplete work that would leave system in broken state
- Every tiny change (batch related small edits)

**Commit message standards:**
- Use heredoc format for multi-line messages:
  ```bash
  git commit -m "$(cat <<'EOF'
  Summary line describing what was done

  Detailed explanation of changes, rationale, and impact.
  Can include multiple paragraphs.

  - Bulleted list of specific changes
  - What was added/modified/removed
  - Why changes were made
  EOF
  )"
  ```
- First line: Clear summary (50-72 chars)
- Body: Explain what, why, and any important context
- Reference decisions, system numbers, or related work
- Be thorough - future you will appreciate the context

**Push immediately after commit:**
- Use `git push -u origin <branch-name>`
- Ensures work is backed up and accessible
- Branch names should start with 'claude/' and end with session ID

---

## Git Workflow Best Practices

- Stage all changes: `git add -A` before committing
- Check status before committing: `git status`
- Review what's being committed: `git diff --cached` if uncertain
- Keep commits atomic (related changes together)
- Never force push to main/master without explicit user request

---

## Known Limitation

Claude Code enforces session-based branching (server-side restriction). This adds ceremony to a personal knowledge base. The workflow above is a pragmatic workaround until a simpler git integration is available.
