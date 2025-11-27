---
title: "6 Archetypes of Broken Ownership"
date: 2025-11-27
draft: false
type: "source"
author: "Alex Ewerlöf"
source_url: "https://blog.alexewerlof.com/p/broken-ownership"
source_type: "blog-post"
tags: ["source", "ownership", "leadership", "organizational-design", "reliability-engineering"]
status: "seeding"
summary: "Explores six archetypes of broken ownership when knowledge, mandate, or responsibility are missing, and how to fix them."
---

## Source Information

- **Author**: Alex Ewerlöf
- **Platform**: Substack (Alex Ewerlöf Notes)
- **Type**: Blog post
- **Date**: August 1, 2023
- **URL**: [6 Archetypes of Broken Ownership](https://blog.alexewerlof.com/p/broken-ownership)

## Core Concept: The Ownership Trio

True ownership requires three components that work together:

1. **Knowledge**: Understanding the system
2. **Mandate**: Authority to make decisions and take action
3. **Responsibility**: Accountability for consequences

### The Rationale

> 1. You cannot be **responsible** for something you don't control. You need the **mandate**.
> 2. You cannot use that **mandate** effectively over something you don't understand. You need **knowledge**.
> 3. You gain **knowledge** only if you are fully **responsible** for the consequences of your **mandate**.

## "You Build It, You Own It" vs "You Build It, You Run It"

The author argues against "You build it, you run it" because it misses the full spectrum of duties required to fully own a product or service. It's not just about running things—it's about fully owning it:

> You build it, you own it.

## The Six Archetypes of Broken Ownership

### 1. Only Mandate (Monkey with a Gun)
**Symptoms**: Manager calls shots without understanding the system or being held responsible for consequences (on-call, alerting, debugging, rearchitecting).

**Context**: Common in top-down hierarchical cultures. Managers who were formerly developers may know enough to be dangerous but push opinions when the team doesn't fight back.

**Impact**: People with knowledge and responsibility feel micromanaged and demoralized.

### 2. Responsibility + Knowledge - Mandate (Foot Soldier)
**Symptoms**: Those with knowledge and responsibility but no mandate can't drive meaningful change. They must go through someone else who lacks both.

**Impact**: Frustration, demoralization. Those who can find better jobs will leave. Company left with those who don't see ahead.

**Quote from managers**: "Running a kindergarten" without acknowledging their role in creating it.

**Solution**: Those with mandate must know what they don't know and grow their org to take the lead. Reference: David Marquet's "Turn the Ship Around."

### 3. Only Responsibility (Baby Parent)
**Symptoms**: No knowledge or mandate over why and when problems happen, but must clean up anyway.

**Context**: Common with IT infrastructure, DevOps, or NOC teams. Some teams call themselves SRE but have little interest in understanding what they're responsible for.

**Impact**: Passive attitude at best, burnout at worst.

**SRE vs DevOps distinction**:
- **DevOps**: Rolls back (reverts changes, treats system as black box, goes to last known good state)
- **SRE**: Fixes forward (fixes the problem and commits a new change)

> [SRE is] what happens when you ask a software engineer to design an operations function

### 4. Mandate + Knowledge - Responsibility (Teenager)
**Symptoms**: Teams have enough knowledge to make decisions but lack responsibility for consequences.

**Impact**: Teams think they have full autonomy and take bolder risks, but often hurt the business and lose mandate. Symptoms include finger pointing, broken DevOps (throwing code over metaphorical wall), disconnect with business and product folks.

**Limitation**: Knowledge element is severely limited to technical aspects because being responsible for running a product is a great source of learning.

### 5. Only Knowledge (Coma)
**Symptoms**: Those with knowledge are isolated from any control or responsibility.

**Impact**: Like having a dream without control over the body. Hard to demonstrate knowledge without mandate (catch-22).

**Ideal path**: Demonstrating knowledge leads to earning trust and accepting responsibilities, which may grow to having the mandate.

### 6. Mandate + Responsibility - Knowledge (The Gambler)
**Symptoms**: Those with mandate and responsibility don't have enough knowledge to make right decisions or understand their full responsibility.

**Impact**: Decisions made with gut feelings and partial data. They'll pay for consequences. Common in operations or infrastructure teams where decisions are made without developers in the room.

**Root cause**: Lack of trust between operations and developers, spiced with ignorance and old-school thinking.

**Reference**: "The Phoenix Project" for ways to address this situation.

## Key Insights

### Broken Ownership Leads To:
- Frustration
- Demoralization
- Waste
- Poor operations

### When Ownership Works:
The three elements (knowledge, mandate, responsibility) go hand-in-hand to shape the ownership trio.

### Limitations of This Framework

The author notes two "bugs" to address in follow-ups:
1. How to align extremely autonomous teams?
2. How to deal with the cognitive load of owning everything in a team?

### Future Topics Mentioned
- Personal ownership
- Organizational ownership
- Why generalist roles should be the default
- How to minimize damage of specialist, ceremonial, and ivory tower roles

## Atomic Notes

- [Ownership Trio](../ownership-trio/) — Knowledge, Mandate, and Responsibility
- [Broken Ownership Archetypes](../broken-ownership-archetypes/) — The six patterns when ownership is incomplete
- [You Build It You Own It](../you-build-it-you-own-it/) — Philosophy of full ownership vs just running things

## Related Concepts

- [Platform Engineering](platform-engineering) — building internal platforms with ownership
- [Engineering Strategy](engineering-strategy) — organizational approaches
- [Intrinsic Qualities in Hiring](intrinsic-qualities-hiring) — ownership as an intrinsic quality

---
*Source: [6 Archetypes of Broken Ownership](https://blog.alexewerlof.com/p/broken-ownership) by Alex Ewerlöf*

