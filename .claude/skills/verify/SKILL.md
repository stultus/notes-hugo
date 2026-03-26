---
name: verify
description: Build the Hugo site and check for warnings, broken links, or issues. Use after editing content to verify everything is correct.
---

Verify the digital garden builds cleanly and content is well-formed.

## Steps

1. Run `hugo --minify` and check for warnings or errors in the output
2. Grep content files for internal links and verify each linked slug has a corresponding file in `content/`:
   - Extract links matching the pattern `[...](slug)` where slug has no protocol prefix
   - Check that `content/<slug>.md` exists for each
3. Check for common issues:
   - Notes with `draft: true` that may have been left accidentally
   - Missing required frontmatter fields (title, date, tags, summary, status)
   - Empty or very short notes that may be incomplete stubs
4. Report findings: build result, any broken links, any content warnings
