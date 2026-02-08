# Home Brain Configuration

This is the `.claude/` configuration folder from a private personal knowledge management system (PKM) called **home-brain** — a markdown-based second brain managed through [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

The actual knowledge base (notes, context files, domain content) lives in a private repo. This repo contains only the **configuration, skills, templates, and behavioral rules** that tell the AI agent how to operate on that knowledge base. It's published separately in case it's useful to anyone building their own AI-assisted PKM.

## How the System Works

The knowledge base is a git repo of markdown files organized into **domains** — life areas like family, travel, workshop, professional development, etc. Each domain has its own folder, README, and internal structure. Cross-domain context (preferences, relationships, patterns) lives in a separate `context/` folder.

An AI agent (Claude) operates on the repo using these configuration files to:

- **Process unstructured input** (voice dictations, raw notes) into structured, linked knowledge
- **Route information** to the right domains and files automatically
- **Enrich content** by researching gaps (looking up restaurants, verifying facts, adding links)
- **Maintain connections** between people, places, events, and ideas across domains
- **Manage tasks** and surface relevant context during work

The core philosophy: a second brain should *think*, not just record. The agent adds value at capture time rather than storing raw notes.

## What's in This Repo

### Always-loaded guidance

Files that define how the agent thinks and acts, loaded every session:

| File | Purpose |
|------|---------|
| `philosophy.md` | Core principles — enrich, connect, validate, think about retrieval |
| `communication.md` | When to ask vs. act, progress reporting, error handling |
| `context-management.md` | Detecting, routing, and storing important facts from conversations |

### On-demand reference

Loaded when the task requires them:

| File | Purpose |
|------|---------|
| `conventions.md` | File naming, metadata standards, linking rules, template system |
| `mcp-optimization.md` | Writing for search discoverability (semantic search via MCP) |
| `decision-system.md` | Structured decision documentation framework |
| `task-system.md` | Task management conventions |
| `git-workflow.md` | Branching and commit procedures |

### Skills (`skills/`)

Procedural workflows triggered by matching user intent to frontmatter descriptions. Each skill defines when it applies, how to think about the task, and what to watch out for.

Examples: processing voice dictations, triaging an inbox, verifying entities via web research, handling multi-person interactions, propagating template changes across existing notes.

### Settings

`settings.local.json` — Tool permission allowlists for the Claude Code CLI.

## Key Concepts

**Domains**: Top-level life areas. Each is self-contained with its own README documenting structure and conventions. Examples: workshop, travel, family, professional development.

**Context files**: Cross-cutting information (preferences, relationships, recurring patterns) stored centrally so the agent can reference them across domains.

**Templates**: Standardized structures for common note types (people, projects, decisions, sources). Notes track which template and version they were created from, enabling bulk updates when templates evolve.

**Skills**: Reusable procedures the agent follows for specific workflows. They separate *what to do* from *how to decide what to do* — the guidance files handle judgment, skills handle execution.

**Inbox**: A processing queue, not a storage location. Items arrive (forwarded content, dictation dumps) and get triaged into the right domains with enrichment and linking.

## Adapting This for Your Own Use

These files encode one person's workflow. To adapt them:

1. Start with `philosophy.md` and `communication.md` — adjust the principles to match how you want your agent to behave
2. Define your own domains based on your life areas
3. Build skills incrementally as you notice repeating workflows
4. Keep context files focused on information the agent genuinely needs across sessions

The system works best when it grows organically from real use rather than being designed upfront.
