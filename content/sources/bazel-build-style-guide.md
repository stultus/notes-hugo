---
title: "Bazel BUILD Style Guide"
date: 2025-01-19
draft: false
tags: ["source", "bazel", "build-systems", "configuration", "style-guide"]
status: "evergreen"
source_url: "https://bazel.build/build/style-guide"
source_type: "documentation"
---

## Source Information

- **URL**: [Bazel BUILD Style Guide](https://bazel.build/build/style-guide)
- **Type**: Official documentation
- **Date Accessed**: 2025-01-19

## Key Content

### DAMP over DRY for BUILD Files

Bazel's style guide recommends preferring DAMP (Descriptive and Meaningful Phrases) over DRY (Don't Repeat Yourself) for BUILD files.

**Rationale**: BUILD files aren't code, they are configurations. They aren't tested like code, but do need to be maintained by people and tools. That makes DAMP better for them than DRY.

The DRY principle encourages uniqueness by introducing abstractions such as variables and functions to avoid redundancy in code.

In contrast, the DAMP principle encourages readability over uniqueness to make files easier to understand and maintain.

## Atomic Notes Derived

- [DRY Principle](dry-principle) - Don't Repeat Yourself
- [DAMP Principle](damp-principle) - Descriptive and Meaningful Phrases

## Related Concepts

- Build systems
- Configuration management
- Code maintainability
- [Engineering Strategy](engineering-strategy)

