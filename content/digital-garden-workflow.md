---
title: Digital Garden Workflow
date: 2025-11-23
draft: false
tags: ["process", "guide"]
summary: "How to maintain this digital garden using Zettelkasten principles."
status: evergreen
---

This guide outlines the workflow for adding notes to this digital garden, ensuring a clear distinction between external inputs (Sources) and internal thoughts (Atomic Notes).

## 1. Capture (Source Notes)
When consuming content (books, courses, articles), create a **Source Note**.

1. Run: `hugo new sources/my-book-title.md`
2. This uses the `archetypes/source.md` template.
3. Fill in the metadata:
   - `type: source` (auto-filled)
   - `author`: Author Name
   - `source_url`: Link to content
   - `status`: `processing`
4. Dump your raw highlights and summaries into this note.

## 2. Process (Atomic Notes)
Refactor the Source Note into **Atomic Notes**.

1. Identify a single, distinct concept in your source notes.
2. Create a new note in the root `content/` directory: `hugo new my-concept.md`.
3. Write the concept in your own words.
4. Link back to the source: `*Source: [[My Book Title]]*`.
5. Link the source to the concept: Update the Source Note's "Atomic Notes" section with `[[My Concept]]`.

## 3. Link (The Web)
Connect the new Atomic Note to other existing notes.

- Ask: "How does this relate to what I already know?"
- Add `[[Wikilinks]]` within the text.
- Add the note to the Map of Content (`content/_index.md`) if it's a major entry point.

## 4. Status
Update the `status` frontmatter as the note evolves:
- `seeding`: 🌱 Stub or rough draft.
- `growing`: 🌿 Developing idea, partially linked.
- `evergreen`: 🌳 Polished, well-linked, permanent reference.

