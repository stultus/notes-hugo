---
title: "Federation is many independent servers speaking a shared protocol"
date: 2026-03-26
lastmod: 2026-03-26
draft: false
tags: ["distributed-systems", "federation", "decentralization", "protocols"]
summary: "Federation applies the email model to any networked application — independent servers interoperate through a shared protocol without central coordination."
status: "budding"
---

**Federation** means many independent servers that talk to each other using a shared protocol, without any central authority coordinating them.

The clearest analogy is email. A Gmail user can email a Yahoo user despite completely different servers, companies, and codebases. Neither asked the other for permission. They both implement SMTP, and that is sufficient for interoperability. Federation applies this same idea to any networked application.

## Key Properties

Each independent server (called an **instance** or **node**):
- Is owned and operated independently
- Has its own database, users, and rules
- Can communicate with other instances that speak the same protocol
- Can choose which instances to communicate with — or refuse to

The collection of all interconnected instances is called the **fediverse** (federated universe).

## Three Network Architectures

**Centralized**: One server, one operator. Twitter, Google Search, Facebook. Total control by the operator. Single point of failure and policy.

**Federated**: Many servers, many operators, one protocol. Email, Mastodon, Matrix. Each server is independent. If one goes down, others are unaffected.

**Peer-to-peer**: No servers at all. Every participant is both client and server. BitTorrent, IPFS. No single point of failure, but also no single point of responsibility.

Federation sits in the middle — it preserves the reliability of having dedicated servers while removing the single point of control. The tradeoff: identity depends on domain availability. If your instance's domain goes offline, your identity becomes unresolvable.

## Related Concepts
- [ActivityPub](activitypub) — the dominant federation protocol for the modern fediverse
- [Local vs Distributed Objects](local-vs-distributed-objects) — the fundamental challenges of distributed interaction that federation must handle
- [Federated search](federated-search) — applying federation to information retrieval
