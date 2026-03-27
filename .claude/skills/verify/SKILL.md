---
name: verify
description: Build the Hugo site and check for warnings, broken links, or issues. Use after editing content to verify everything is correct.
---

Verify the digital garden builds cleanly and content is well-formed.

## Steps

1. Run `hugo --minify` and check for warnings or errors in the output. The theme is a git submodule — run `git submodule update --init` first if layout warnings appear.
2. **Check links against generated HTML** (not markdown — the theme's render-link.html transforms links, so only the HTML output tells the truth):
   - Scan all `public/**/index.html` files for `href` attributes
   - For internal links starting with `/notes/`, verify the target directory exists in `public/`
   - Flag any bare relative slugs (href without `/notes/` prefix, not external, not anchor, not css/js) — these are broken because the theme only resolves them when link text exactly matches the target page's title
3. Check for common content issues:
   - Notes with `draft: true` that may have been left accidentally
   - Missing required frontmatter fields (title, date, tags, summary, status)
   - Empty or very short notes that may be incomplete stubs
   - Internal links using bare slug format `[Text](slug)` instead of absolute `/notes/slug/` — these will break unless the link text matches the target's title exactly
4. Report findings: build result, any broken links, any content warnings
