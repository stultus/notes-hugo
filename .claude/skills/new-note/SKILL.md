---
name: new-note
description: Create a new digital garden note following Zettelkasten and evergreen note principles. Use when adding a new note to the garden.
---

Create a new note in the digital garden. Use `$ARGUMENTS` as the topic if provided.

## Digital Garden Principles

Follow these principles when writing every note:

- **Atomic**: One discrete idea per note. If you cannot summarize the note in a single sentence, it likely needs to be split into multiple notes.
- **Concept-oriented titles**: Title by the idea, not the source. Use complete declarative phrases when possible — "Distributed objects cannot behave like local objects" over "Distributed Computing Notes." The title should work as an API: usable as a reference in other notes without needing to open it.
- **Write in your own words**: Paraphrase and synthesize rather than quoting. Direct quotes are evidence, not notes.
- **Densely linked**: Every note should link to at least 2–3 existing notes. Weave links into the body text where the relationship is discussed, not just in a "Related Concepts" section. The most valuable links cut across domains.
- **Self-contained**: Write enough context that the note is understandable months later without re-reading the source material.
- **Low bar for creation, high bar for evergreen**: Create seedlings freely. Only mark a note "evergreen" when it is polished, well-linked, and represents a stable understanding.

## Steps

1. Choose a kebab-case filename for `content/<slug>.md` (or `content/sources/<slug>.md` for source type)
2. Write the file with YAML frontmatter (`---` delimiters):
   - `title`: Concept-oriented title (a claim or complete phrase, not just a topic word)
   - `date`: Today's date (YYYY-MM-DD)
   - `lastmod`: Today's date (YYYY-MM-DD)
   - `draft`: false (set to true only if explicitly asked)
   - `tags`: Relevant tags as a YAML list
   - `summary`: One-sentence description of the core idea
   - `status`: "seeding" for stubs, "budding" for developed drafts, "evergreen" for polished notes
   - `type`: "note" (atomic idea), "essay" (long form), "moc" (map of content), "source" (external source material)
3. Write the note body:
   - State the core idea clearly in the opening
   - Develop with supporting reasoning, examples, or evidence
   - Weave internal links inline where connections are discussed: `[Title](slug)`
   - End with a **Related Concepts** section listing connected notes as relative links
4. After creating the note, check if existing notes should link back to it — suggest specific edits with the link placed at the point of relevance in the existing note's body
