---
name: entity-verification
description: Verify vague entities before capturing. Use when user mentions approximate names (recipes, books, products, people) or provides links that contain canonical names. Research before asking.
---

# Entity Verification

Clarifying vague but verifiable entities before capturing them — especially when the user provides a source link.

---

## When This Applies

- User mentions a vague entity (recipe name, book title, person's full name, product name, etc.)
- User provides a link that would contain the actual/canonical name
- User uses shorthand or approximate names for things that have official names

---

## The Goal

Act like a chief of staff: research and verify facts from primary or secondary sources, but never assume or hallucinate. When the user provides a link or reference, do the legwork to find the actual name. If verification fails, ask the user for clarification — don't silently proceed with vague information, and never invent a "better" name.

---

## How to Think About It

**Links are verification opportunities.** When the user provides a URL, the source likely contains the canonical name. Fetch and verify before capturing.

**Do the research before asking.** Don't immediately ask the user when direct fetch fails. Try multiple approaches:
1. Try fetching the URL directly
2. If blocked, try oEmbed endpoints (e.g., `noembed.com/embed?url=...`)
3. Search for the URL/video ID + platform name
4. Search for the vague description + platform + recent timeframe
5. Look for the creator/channel name in search results and cross-reference

**Ask only as a last resort.** After exhausting research options, then ask the user for the actual name rather than capturing the vague version.

**Specificity matters for retrieval.** Vague names make notes harder to find later. "Steve Cusato's Spaghetti in Tomato Water" is more useful than "tomato water recipe."

---

## Watch Out For

**Hallucinating "better" names.** Never invent a more specific or formal-sounding name. "Tomato water" does NOT become "Clarified Tomato Consommé" unless you found that exact name in a source. Unverified upgrades are worse than vague originals.

**Capturing before verifying.** Take the extra step to check the source, especially for:
- Recipe names
- Book/article titles
- Product names
- People's full names
- Company/organization names
- Event names

**Giving up too early on blocked sources.** YouTube, some news sites, and paywalled content may not be directly fetchable — but web searches, oEmbed APIs, and indirect lookups often work. Exhaust these options before asking the user.

---

## Examples

**Bad:**
> User: "Add this pasta recipe to my list: [link]"
> Claude: *Adds "pasta recipe" to backlog*

**Good:**
> User: "Add this pasta recipe to my list: [link]"
> Claude: *Fetches link, finds it's "Marcella Hazan's Tomato Butter Sauce"*
> Claude: *Adds "Marcella Hazan's Tomato Butter Sauce" to backlog*

**Good (when direct fetch fails):**
> User: "Add this tomato water recipe to my list: [YouTube link]"
> Claude: *Can't fetch YouTube directly*
> Claude: *Tries oEmbed API — also blocked*
> Claude: *Searches for video ID + "youtube" — no results*
> Claude: *Searches for "tomato water recipe youtube 2024 2025"*
> Claude: *Finds "Not Another Cooking Show" mentioned with tomato water content*
> Claude: *Cross-references and confirms it's "Spaghetti in Tomato Water" by Steve Cusato*
> Claude: *Adds "Spaghetti in Tomato Water (Not Another Cooking Show)" to backlog*

**Last resort (after exhausting research):**
> User: "Add this recipe to my list: [link]"
> Claude: *Tries all verification approaches, none yield the actual name*
> Claude: "I couldn't find the specific recipe name from that link. What's the actual title?"
