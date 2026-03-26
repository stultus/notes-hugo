# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Hugo-based digital garden (personal notes site) published at https://stultus.in/notes/. Theme is a git submodule — always clone with `--recurse-submodules`.

## Build Commands

- **Build**: `hugo --minify`
- **Dev server**: `hugo server` (http://localhost:1313)
- No package manager or build tools beyond Hugo itself

## Content Conventions

- Notes live as flat markdown files in `content/` (sources in `content/sources/`)
- File names use kebab-case (e.g., `broken-windows-theory.md`)
- Frontmatter uses TOML (`+++`) for archetypes but existing notes use YAML (`---`)
- Required frontmatter fields: `title`, `date`, `lastmod`, `draft`, `tags`, `summary`, `status`
- **Type** field: `note` (atomic idea), `essay` (long form), `moc` (map of content), `resource` (external), `source` (in sources/)
- **Status** field: `seeding` (stub), `budding` (draft), `evergreen` (polished)
- Internal links between notes use relative markdown: `[Title](slug)` — no leading slash, no `.md` extension
- The `_index.md` homepage uses absolute paths: `[Title](/notes/slug/)`

## Deployment

- Push directly to `main` — GitHub Actions builds and deploys to the separate `stultus/notes` repo
- CI uses Hugo extended (latest) with `hugo --minify`
