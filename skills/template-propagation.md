---
name: template-propagation
description: Update documents when templates change. Use when a template has structural changes, user asks to "propagate template changes", or documents have outdated template_version.
---

# Template Propagation

Updating derived documents when a template's structure changes.

---

## When This Applies

- A template has been updated with structural changes
- User asks to "propagate template changes" or "update docs from template"
- You notice documents with outdated `template_version`

---

## The Goal

Systematically update documents derived from a template to match the new structure, while preserving their content.

---

## How to Think About It

**Template changes come in different flavors:**

1. **Additive** (new sections): Add the new sections to derived docs, usually empty or with sensible defaults
2. **Structural** (reorganization): Move content from old structure to new locations
3. **Clarifying** (better prompts/labels): Update labels but keep content
4. **Subtractive** (removed sections): Decide whether to delete or archive removed content

**Not all docs need the same treatment:**

- Some docs may have already evolved beyond the template
- Reference documents (`type: reference`) are exempt from template updates
- User may want to review changes before applying

---

## The Process

### 1. Identify Affected Documents

Find all documents derived from the changed template:

```
# Search for template reference in frontmatter
grep -r "template: templates/[template-name].md" --include="*.md"
```

Filter to those with `template_version` < current version.

### 2. Understand the Changes

Read the template's changelog to understand what changed between versions.

Categorize each change:
- What sections were added?
- What sections were reorganized?
- What sections were removed?
- What labels/prompts changed?

### 3. Plan the Update

For each affected document, determine:
- What needs to be added (new sections)
- What content needs to move (reorganization)
- What can be updated in place (labels)
- What needs user decision (removed sections with content)

### 4. Apply Updates

For each document:
1. Read the current content
2. Apply structural changes, preserving existing content
3. Update `template_version` to current version
4. Update `updated` date

### 5. Report Results

Summarize:
- How many documents were updated
- Any documents that need user review (e.g., had content in removed sections)
- Any documents that were skipped (already evolved beyond template)

---

## Watch Out For

**Content loss.** Never delete user content without asking. If a section is removed from a template but a document has content there, flag it for review.

**Over-automation.** Some documents may have intentionally diverged from the template. Check if content seems to follow template structure before bulk-updating.

**Version mismatch.** A document at version 1 being updated to version 5 may need incremental changes, not a jump. Read all changelog entries.

**Reference documents.** Documents with `type: reference` are not template-derived and should be skipped.

---

## Example

Template `person.md` updated from v1 to v2:
- Added: "### Communication Preferences" under Profile
- Renamed: "Current Life" → "Current Situation"
- Removed: Nothing

Propagation steps:
1. Find all docs with `template: templates/person.md` and `template_version: 1`
2. For each doc:
   - Add empty "### Communication Preferences" section after Basic Info
   - Rename "Current Life" header to "Current Situation" (content unchanged)
   - Update `template_version: 2`
   - Update `updated: [today]`
3. Report: "Updated 7 person files to template v2"
