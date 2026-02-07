# Task Management

This document describes the task management system for home-brain.

---

## Task System Overview

**IMPORTANT**: At the start of each session, read `/tasks.md` for active tasks.

### Core Principles
- **Single source**: All tasks live in `/tasks.md` at repository root
- **Ownership tags**:
  - `@claude` - You own and execute these tasks
  - `@user` - User owns; you provide reminders and tracking
- **Status markers**: `[ ]` todo, `[~]` in progress, `[x]` done
- **Context**: Tasks include dates, notes, links as needed

---

## Your Responsibilities

- **Session start**: Read tasks.md to see active `@claude` tasks
- **During work**: Update status as you complete tasks
- **When asked**: Parse and summarize tasks by domain or ownership
- **Proactive**: Flag upcoming date-sensitive `@user` tasks when relevant
- **Cleanup**: Archive completed tasks to keep active list manageable
- **Task boundaries**: After finishing a task, check `inbox/` and propose triage (see communication.md "When Finishing a Task")

---

## Task Updates

- Mark `@claude` tasks in progress when you start: `- [~]`
- Mark complete when done: `- [x]`
- Add new tasks as they emerge from conversations
- Don't delete completed tasks immediately - archive them

---

## Task Structure in tasks.md

Tasks are organized by domain and include:
- Clear description of what needs to be done
- Ownership tag (`@claude` or `@user`)
- Status marker
- Relevant dates, links, or context as needed

---

*Check tasks.md at session start. Update it throughout the session.*
