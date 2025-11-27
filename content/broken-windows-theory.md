---
title: "Broken Windows Theory"
date: 2025-11-27
draft: false
tags: ["criminology", "software-engineering", "tech-debt", "maintainability", "organizational-culture"]
status: "seeding"
summary: "Theory that visible signs of disorder encourage further disorder. In software, tech debt compounds as teams care less about quality over time."
---

# Broken Windows Theory

The **Broken Windows Theory** is a criminological theory that states visible signs of crime, antisocial behavior, and civil disorder create an urban environment that encourages further crime and disorder, including serious crimes.

## Original Theory (Criminology)

The theory suggests that policing methods that target minor crimes, such as vandalism, loitering, public drinking, and fare evasion, help to create an atmosphere of order and lawfulness, preventing more serious crimes.

**Core principle**: Small signs of disorder signal that no one cares, which encourages more disorder and eventually serious crime.

## Application to Software Maintainability

In the context of software engineering, the broken windows theory means: **the more tech debt there is, the less care is given to new development.**

### How It Works

- **Visible tech debt** (broken windows) signals that code quality doesn't matter
- Teams become less careful with new code when surrounded by existing debt
- The negative effect of tech debt **compounds over time**
- Small issues accumulate into larger problems
- Eventually, the codebase becomes unmaintainable

### Why It Matters

- **Prevents compounding debt**: Addressing small issues early prevents larger problems
- **Maintains code quality culture**: Clean codebases encourage careful development
- **Reduces maintenance burden**: Less debt means easier changes
- **Improves team morale**: Working in clean code is more satisfying

## Key Insight

The theory highlights that **environmental signals matter**. Whether in urban neighborhoods or codebases, visible disorder encourages more disorder. The solution is to maintain order by addressing small issues before they compound.

## Related Concepts

- [Theory of Constraints](theory-of-constraints) — focusing on bottlenecks
- [Platform Engineering](platform-engineering) — maintaining quality in internal platforms
- [You Build It You Own It](you-build-it-you-own-it) — ownership includes maintaining quality

## Sources

- [Wikipedia - Broken Windows Theory](https://en.wikipedia.org/wiki/Broken_windows_theory)
- [Tech Debt Day](https://blog.alexewerlof.com/p/tech-debt-day) by Alex Ewerlöf

