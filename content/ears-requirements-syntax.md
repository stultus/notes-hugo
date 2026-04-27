---
title: "EARS: Easy Approach to Requirements Syntax"
date: 2026-04-07
lastmod: 2026-04-07
draft: false
tags: ["requirements-engineering", "software-engineering", "systems-engineering", "templates"]
summary: "EARS is a template-based method for writing natural language requirements that are clear, testable, and unambiguous. Five patterns — ubiquitous, event-driven, unwanted behavior, state-driven, and optional — cover most real-world needs."
status: "growing"
---

EARS was developed by **Alistair Mavin and colleagues at Rolls-Royce** to fix a common problem: natural language requirements are often vague, ambiguous, and untestable. EARS provides five syntactic templates that constrain how requirements are written, without abandoning natural language entirely.

The key insight: most requirements fall into a small number of structural patterns. By naming those patterns and giving each a template, you make the writer's job easier and the reader's job possible.

## The problem

Natural language requirements tend to suffer from:

- **Ambiguity** — words like "appropriate", "sufficient", "timely" mean different things to different people
- **Incompleteness** — missing triggers, conditions, or context
- **Untestability** — no clear way to verify the requirement is met
- **Inconsistency** — different writers use different structures for the same kind of requirement

EARS doesn't eliminate natural language. It constrains it just enough to avoid these problems.

## The five patterns

### 1. Ubiquitous

Requirements that apply all the time, without any trigger or condition.

**Template:** `The <system> shall <response>`

> The control system shall maintain a log of all operator commands.

This is the simplest pattern. If you don't need a trigger, state, or condition — it's ubiquitous.

### 2. Event-driven

Requirements triggered by a specific event.

**Template:** `WHEN <trigger>, the <system> shall <response>`

> When the pilot presses the emergency stop button, the engine control system shall cut fuel supply within 200ms.

The keyword **when** signals a discrete event. Something happens, and the system responds.

### 3. State-driven

Requirements that apply only while the system is in a particular state or mode.

**Template:** `WHILE <state>, the <system> shall <response>`

> While the aircraft is in cruise mode, the autopilot shall maintain the selected altitude within ±50 feet.

The keyword **while** signals a continuous condition. The requirement holds for the duration of the state.

### 4. Unwanted behavior

Requirements that handle error conditions, failures, or edge cases.

**Template:** `IF <unwanted condition>, THEN the <system> shall <response>`

> If the sensor reading is outside the valid range, then the monitoring system shall raise an alert and switch to the backup sensor.

The **if/then** pattern is reserved for situations you want to prevent or recover from. It is not for normal operational logic — that's what event-driven and state-driven patterns are for.

### 5. Optional features

Requirements that depend on the presence of a feature, configuration, or capability.

**Template:** `WHERE <feature>, the <system> shall <response>`

> Where the vehicle is equipped with a rear-view camera, the display system shall show the camera feed when reverse gear is engaged.

The keyword **where** signals a feature or configuration condition — not all deployments will have it.

## Complex requirements

Real-world requirements often combine patterns. EARS handles this by chaining keywords:

`WHILE <state>, WHEN <trigger>, the <system> shall <response>`

> While the engine is running, when oil pressure drops below the minimum threshold, the monitoring system shall activate the warning light.

`WHERE <feature>, WHILE <state>, WHEN <trigger>, the <system> shall <response>`

The order matters: WHERE → WHILE → WHEN → shall.

## Keywords at a glance

| Keyword | Signals | Pattern |
|---------|---------|---------|
| *(none)* | Always applies | Ubiquitous |
| **When** | A discrete event/trigger | Event-driven |
| **While** | A system state or mode | State-driven |
| **If/Then** | An unwanted condition | Unwanted behavior |
| **Where** | A feature or configuration | Optional |
| **Shall** | Mandatory behavior | All patterns |

## Why it works

- **Consistency** — every requirement follows a known structure
- **Testability** — each pattern maps directly to a test case (is the trigger present? is the state active? does the response happen?)
- **Completeness** — the templates prompt writers to fill in the missing pieces (what's the trigger? what state? what condition?)
- **Communication** — stakeholders and engineers read the same structure, reducing misinterpretation

EARS was originally designed for aerospace and defense at Rolls-Royce, where ambiguity in requirements can have safety-critical consequences. It has since been adopted in automotive, medical devices, and general software engineering.

## Applying EARS

1. Start by classifying the requirement — is it always on (ubiquitous)? triggered by an event? dependent on a state?
2. Pick the matching template
3. Fill in the blanks with specific, measurable language
4. If it doesn't fit one pattern, combine them
5. Avoid negation — use the unwanted behavior pattern instead of saying what the system shall *not* do

## Related
- [Architecture Decision Records](/notes/architecture-decision-records/) — another structured format for capturing engineering decisions
- [Process Documentation](/notes/process-documentation/) — why structured formats help teams work better
