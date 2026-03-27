---
title: "DRY Principle"
date: 2025-01-19
draft: false
tags: ["software-engineering", "principles", "code-quality", "abstraction"]
status: "evergreen"
summary: "Don't Repeat Yourself - a principle encouraging uniqueness through abstraction."
---

**DRY** stands for "Don't Repeat Yourself" - a software engineering principle that encourages uniqueness by introducing abstractions such as variables and functions to avoid redundancy in code.

## Core Idea

The DRY principle aims to reduce repetition by:
- Creating reusable abstractions
- Extracting common patterns into functions or variables
- Maintaining a single source of truth for each piece of knowledge

## When DRY Works Well

DRY is effective for:
- Application code that is tested
- Logic that truly represents the same concept
- Reducing maintenance burden through centralization

## When DRY May Not Apply

Not all repetition is bad. Consider [DAMP](/notes/damp-principle/) for:
- Configuration files
- Test code
- Cases where readability matters more than uniqueness

## Related Concepts

- [DAMP Principle](/notes/damp-principle/) — alternative approach for configuration
- [Broken Windows Theory](/notes/broken-windows-theory/) — code duplication as a form of technical decay
- [Source: Bazel BUILD Style Guide](../sources/bazel-build-style-guide)

