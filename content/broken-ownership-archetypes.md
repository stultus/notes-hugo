---
title: "Broken Ownership Archetypes"
date: 2025-11-27
draft: false
tags: ["ownership", "leadership", "organizational-design", "anti-patterns"]
status: "seeding"
summary: "Six patterns of broken ownership that occur when Knowledge, Mandate, or Responsibility are missing."
---

# Broken Ownership Archetypes

**Broken Ownership Archetypes** are six patterns that occur when one or two components of the [Ownership Trio](ownership-trio) (Knowledge, Mandate, Responsibility) are missing, leading to frustration, demoralization, waste, and poor operations.

## The Six Archetypes

### 1. Only Mandate (Monkey with a Gun)
**Missing**: Knowledge, Responsibility

Manager calls shots without understanding the system or being held responsible for consequences (on-call, alerting, debugging, rearchitecting).

**Context**: Common in top-down hierarchical cultures. Managers who were formerly developers may know enough to be dangerous.

**Impact**: People with knowledge and responsibility feel micromanaged and demoralized.

### 2. Responsibility + Knowledge - Mandate (Foot Soldier)
**Missing**: Mandate

Those with knowledge and responsibility but no mandate can't drive meaningful change. They must go through someone else who lacks both.

**Impact**: Frustration, demoralization. Those who can find better jobs will leave.

**Solution**: Those with mandate must know what they don't know and grow their org to take the lead.

### 3. Only Responsibility (Baby Parent)
**Missing**: Knowledge, Mandate

No knowledge or mandate over why and when problems happen, but must clean up anyway.

**Context**: Common with IT infrastructure, DevOps, or NOC teams. Some teams call themselves SRE but have little interest in understanding what they're responsible for.

**Impact**: Passive attitude at best, burnout at worst.

### 4. Mandate + Knowledge - Responsibility (Teenager)
**Missing**: Responsibility

Teams have enough knowledge to make decisions but lack responsibility for consequences.

**Impact**: Teams think they have full autonomy and take bolder risks, but often hurt the business and lose mandate. Symptoms include finger pointing, broken DevOps (throwing code over metaphorical wall), disconnect with business and product folks.

**Limitation**: Knowledge element is severely limited to technical aspects because being responsible for running a product is a great source of learning.

### 5. Only Knowledge (Coma)
**Missing**: Mandate, Responsibility

Those with knowledge are isolated from any control or responsibility.

**Impact**: Like having a dream without control over the body. Hard to demonstrate knowledge without mandate (catch-22).

**Ideal path**: Demonstrating knowledge leads to earning trust and accepting responsibilities, which may grow to having the mandate.

### 6. Mandate + Responsibility - Knowledge (The Gambler)
**Missing**: Knowledge

Those with mandate and responsibility don't have enough knowledge to make right decisions or understand their full responsibility.

**Impact**: Decisions made with gut feelings and partial data. They'll pay for consequences. Common in operations or infrastructure teams where decisions are made without developers in the room.

**Root cause**: Lack of trust between operations and developers, spiced with ignorance and old-school thinking.

## Why These Patterns Emerge

Broken ownership is much more common than true ownership because:
- Organizations create silos between teams
- Managers want control without understanding
- Teams lack trust and communication
- Old-school thinking separates dev and ops
- People avoid responsibility for consequences

## How to Fix

The solution is to establish the [Ownership Trio](ownership-trio):
- Ensure teams have **Knowledge** of what they own
- Give teams **Mandate** to make decisions
- Hold teams **Responsible** for outcomes

## Related Concepts

- [Ownership Trio](ownership-trio) — the three components needed for true ownership
- [You Build It You Own It](you-build-it-you-own-it) — philosophy of full ownership
- [RACI Matrix](raci-matrix) — responsibility assignment framework
- [Platform Engineering](platform-engineering) — building with ownership

## Source

- [6 Archetypes of Broken Ownership](../sources/alex-ewerlof-broken-ownership/) by Alex Ewerlöf

