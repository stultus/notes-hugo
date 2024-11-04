+++
title = "Footnotes in Markdown"
date = 2024-11-03T20:26:05+05:30
lastmod = 2024-11-03T20:26:05+05:30
draft = false
garden_tags = ["note", "markdown", "syntax"]
summary = "Syntax to create footnotes in Markdown"
status = "evergreen"
+++

- The basic syntax is to append square brackets *without a space* to the end of the sentence you are footnoting.
- Inside the brackets, you begin with a caret symbol (^) followed by a label that connects the citation number to the footnote text. Citation numbers are autogenerated when the markdown file is processed, regardless of the label’s text.

```markdown
This is an example sentence.[^label]

[^label]: footnote text is displayed at the end of the page.
```


## Multiple References to the Same Footnote
```markdown
This is mentioned earlier.[^1] This refers to the same footnote.[^1]
```
---
- This is useful while creating [Digital gardens]({{<ref "garden/digital_garden.md">}})