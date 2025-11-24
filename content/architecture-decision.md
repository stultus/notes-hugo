---
title: "Architecture Decision (AD)"
date: 2025-11-24
draft: false
tags: ["architecture", "software-design", "decision-making", "engineering"]
status: "seeding"
summary: "A software design choice that addresses an architecturally-significant requirement."
---

An **Architecture Decision (AD)** is a software design choice that addresses an [Architecturally-significant Requirement (ASR)](architecturally-significant-requirement).

## Attributes

### Rationale

A textual explanation of the "why" of the decision, its justification. It should not paraphrase or repeat information captured in other attributes, but add value. If the rationale is expressed in a complete external document (e.g., a tradeoff analysis), the rationale should point to that document.

### Scope

The scope of an architecture decision can be defined along multiple dimensions:

- **System Scope**: The part of the system affected
  - Example: "The Communication subsystem is programmed in C++ and not in Java"
- **Time Scope**: The period during which the decision applies
  - Example: "Until the first customer release, testing is done using glider"
- **Organization Scope**: The organizational units affected
  - Example: "The Japanese team uses a different bug tracking system"

### State

The current state of the decision in its lifecycle:

- **Idea**: Initial concept
- **Tentative**: Under consideration
- **Decided**: Decision made but not yet approved
- **Approved**: Formally approved
- **Challenged**: Being questioned or reconsidered
- **Rejected**: Decision not adopted
- **Obsolesced**: No longer relevant or superseded

### Author, Time-stamp, History

The person who made the decision, when it was taken, and the history of changes to the decision.

### Cost

The cost implications of the decision, including development, maintenance, and operational costs.

### Risk

The risks associated with the decision, including technical, business, and organizational risks.

## Related Concepts

- [Architecturally-significant Requirement (ASR)](architecturally-significant-requirement) — requirements that drive architecture decisions
- [Architecture Decision Records (ADRs)](architecture-decision-records) — documentation format for capturing ADs
- [Engineering Strategy](engineering-strategy)
- [Pre-Mortem Analysis](pre-mortem-analysis) — identifying risks before decisions are finalized

---
*Definition based on standard software architecture practice.*

