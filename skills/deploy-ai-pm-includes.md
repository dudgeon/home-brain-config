---
description: Deploy changes to ai-pm _includes/ files (head_custom.html, footer_custom.html, etc.) directly to the dudgeon/ai-pm public repo. Use this when editing Jekyll infrastructure files — the sync workflow does NOT cover _includes/.
---

# Skill: Deploy ai-pm `_includes/`

The sync workflow (`sync-public-repos.yml`) excludes `_includes/` by design. Changes to Jekyll infrastructure files must be pushed directly to `dudgeon/ai-pm` via the GitHub API.

## When to Use

Any time you edit files in `domains/professional-development/ai-pm/_includes/`.

## Procedure

### 1. Get the current SHA of the file in the public repo

```bash
gh api repos/dudgeon/ai-pm/contents/_includes/head_custom.html \
  --jq '.sha'
```

### 2. Push the updated file

```bash
gh api repos/dudgeon/ai-pm/contents/_includes/head_custom.html \
  --method PUT \
  --field message="<describe the change>" \
  --field sha="<SHA from step 1>" \
  --field content="$(base64 -i domains/professional-development/ai-pm/_includes/head_custom.html)"
```

Replace `head_custom.html` with the actual filename if different.

### 3. Verify the Pages rebuild

```bash
gh run list --repo dudgeon/ai-pm --limit 3
```

Wait for the "Deploy to GitHub Pages" run to show `completed success` (~30–40s).

### 4. Spot-check the live site

Navigate to a page on ai-pm.cc and confirm the change is visible.

## Notes

- The `sha` field is required by the GitHub API when updating an existing file — omitting it will fail
- `base64 -i` on macOS reads from a file path; on Linux use `base64 < file`
- If the file doesn't exist yet in the public repo, omit the `sha` field entirely
