---
title: "DAMP Principle"
date: 2025-01-19
draft: false
tags: ["software-engineering", "principles", "readability", "configuration"]
status: "evergreen"
summary: "Descriptive and Meaningful Phrases - a principle encouraging readability over uniqueness."
---

**DAMP** stands for "Descriptive and Meaningful Phrases" - a principle that encourages readability over uniqueness to make files easier to understand and maintain.

## Core Idea

The DAMP principle prioritizes:
- Clarity and explicitness
- Easy comprehension at a glance
- Maintainability by humans and tools
- Descriptive, self-documenting code

## When DAMP Works Better Than DRY

DAMP is particularly effective for:
- **Configuration files**: BUILD files, config files that aren't tested like code
- **Test code**: Where each test should be independently understandable
- **Tool-maintained code**: Files that need to be parsed and modified by automated tools

## Bazel's Rationale

According to the [Bazel BUILD Style Guide](https://bazel.build/build/style-guide), BUILD files aren't code, they are configurations. They aren't tested like code, but do need to be maintained by people and tools. That makes DAMP better for them than [DRY](dry-principle).

## Trade-offs

DAMP accepts some repetition in exchange for:
- Better discoverability
- Easier automated refactoring
- Clearer intent
- Reduced cognitive load

## Related Concepts

- [DRY Principle](dry-principle) - Alternative approach for application code
- [Source: Bazel BUILD Style Guide](sources/bazel-build-style-guide)
- Code readability
- Configuration management

