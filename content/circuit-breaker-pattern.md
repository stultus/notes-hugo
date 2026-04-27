---
title: "Circuit breaker pattern stops cascading failures in distributed systems"
date: 2026-03-26
lastmod: 2026-03-26
draft: false
tags: ["distributed-systems", "software-architecture", "reliability", "design-patterns"]
summary: "The circuit breaker pattern prevents a system from wasting resources on a failing remote dependency by short-circuiting requests after consecutive failures."
status: "growing"
---

The **circuit breaker pattern** prevents a system from repeatedly calling a remote service that is failing, which would waste resources and potentially cause cascading failures. It is one of the most important reliability mechanisms in [federated](/notes/federation/) and distributed systems.

## Three States

The circuit breaker operates as a state machine:

**Green (Closed)** — requests flow normally. Every success keeps the circuit green.

**Red (Open)** — after N consecutive failures, the circuit trips open. All requests to this dependency are immediately skipped without attempting the call. This prevents the system from blocking threads and consuming resources on a service that is clearly down.

**Yellow (Half-Open)** — after a cooldown period, the circuit allows one "probe" request through. If the probe succeeds, the circuit returns to green. If it fails, the circuit returns to red and resets the cooldown.

```
GREEN  ── delivery succeeds normally
   │
   │ (N consecutive failures)
   v
RED    ── all calls are skipped
   │
   │ (cooldown period expires)
   v
YELLOW ── allow one probe attempt
   │
   ├── (probe succeeds) ──> GREEN
   └── (probe fails) ──> RED (reset cooldown)
```

## Paired with Exponential Backoff

Individual request retries typically use **exponential backoff** — doubling the wait between attempts (e.g., 1min, 4min, 16min, 64min). The circuit breaker operates at a higher level: once retries are exhausted for multiple requests, the circuit trips and stops all attempts for a cooldown period.

## In Federation

In [ActivityPub](/notes/activitypub/) federation, if an instance needs to deliver activities to 100 followers and one follower's server is down, without a circuit breaker that dead server consumes delivery worker threads on every update cycle. Mastodon implements this pattern (they call it "Stoplight") — it is essential for any system that pushes messages to many independent servers.

## Related Concepts
- [Federation](/notes/federation/) — the network model where circuit breakers are critical for reliability
- [Local vs Distributed Objects](/notes/local-vs-distributed-objects/) — partial failure is a fundamental difference in distributed systems that circuit breakers address
- [Broken Windows Theory](/notes/broken-windows-theory/) — in a different domain, but shares the principle that small failures left unaddressed compound into larger problems
