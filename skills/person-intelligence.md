---
name: person-intelligence
description: Handle person notes, multi-person interactions, and entity updates. Use when processing interactions involving multiple people, updating person profiles, or managing kids' friends and timeline entries.
---

# Person Intelligence

This skill covers how to handle person notes, including multi-person interactions and entity updates. This is sophisticated, working logic—preserve it exactly.

---

## Person Notes Structure

**Template structure**: Person notes have two main sections:
- **Profile**: Evergreen information (basic info, context, current life, important info)
- **Timeline**: Chronological record of interactions and life events

---

## Kids' Friends vs Personal Contacts

**Kids' friends**: Keep as entries in each child's `friends.md` file using `###` headings per friend. Don't create individual person files.

**User's personal contacts** (friends, colleagues, etc.): Create individual person files in `domains/people/` with full Profile/Timeline template. Do NOT put non-family members in the family domain.

---

## Kids' Timeline Entries

**IMPORTANT**: Owen and Alina have their own timeline files at `domains/family/[child]/memories.md` under a `## Timeline` section. These timelines capture memorable events, milestones, and experiences from their perspective.

**When to add a kids' timeline entry**: Any event that is meaningful, memorable, or milestone-worthy for that child:
- Trips and outings (family travel, day trips)
- Scout events, school milestones, recitals, competitions
- New discoveries (new hobby, new interest, new skill)
- Social events (birthday parties attended, notable playdates)
- Achievements (academic, athletic, creative)
- Memorable experiences (snow days, special hangouts, camps)

**Cross-posting rule**: When content is filed in a domain-specific location (e.g., boy-scouts, travel, swim), ALSO add a timeline entry to the child's memories.md if the event is meaningful to them. The domain file has the detail; the timeline has the summary for the child's personal record.

**Both kids for shared events**: Family trips, shared activities, and school closures should get entries in BOTH kids' timelines (customized per child if their experience differed).

---

## Nicknames & Aliases

Person files include a "Nicknames/Aliases" field for alternate names, initials, or relational terms. When processing user input:
- Match against nicknames (e.g., initials or pet names defined in person files)
- Match relational terms (e.g., "my brother" → look up brother in family context)
- Use nicknames when searching for or updating person files

---

## Multi-Person Interactions

**IMPORTANT**: When user shares notes from an interaction involving multiple people (e.g., lunch with five friends, dinner party, group outing):

1. **Identify all people mentioned** (by name OR nickname/alias) who have person files in the system
2. **Add the same timeline entry** to EACH person's file
3. **Customize slightly** if there's person-specific info (e.g., "Kevin mentioned he's job hunting" goes in Kevin's entry with more detail)
4. **Create person files** for anyone mentioned who doesn't have one yet (using the person template)
5. **Update Profile sections** if the interaction reveals new evergreen info (new job, moved, new hobby)

---

## Example

**User says**: "Had lunch with Kevin, Sarah, and Marcus. Kevin is switching jobs, Sarah just got engaged, Marcus is training for a marathon."

**Actions**:
- Add timeline entry to Kevin, Sarah, and Marcus files
- Update Kevin's Profile > Current Life > Work with job change info
- Update Sarah's Profile with engagement
- Update Marcus's Profile > Interests with marathon training

This ensures the system captures relationship context across all relevant person files, not just the first one mentioned.

---

## When to Create Person Files

Create a person file when:
- User's direct friend, colleague, or personal contact is mentioned multiple times
- Enough context exists to populate basic profile
- The relationship warrants tracking interactions

Don't create person files for:
- Kids' friends (use friends.md in child's folder)
- One-off mentions without relationship context
- Service providers unless there's ongoing relationship
