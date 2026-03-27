---
title: "Relay pattern reduces N-squared delivery to 2N in federation"
date: 2026-03-26
lastmod: 2026-03-26
draft: false
tags: ["distributed-systems", "federation", "design-patterns", "scalability"]
summary: "A relay instance acts as a hub that redistributes activities, reducing the delivery cost from N×(N-1) direct connections to 2N."
status: "budding"
---

In a [federated](/notes/federation/) network, if every instance sends updates to every other instance, the delivery cost is N × (N-1). For 100 instances, that is 9,900 deliveries per update cycle. For 1,000 instances, it is 999,000.

A **relay** reduces this to 2N: each instance sends one update to the relay, and the relay forwards it to all other subscribers.

## How It Works

A relay is a special instance with no content of its own. Its sole job is to receive activities and redistribute them. It uses the standard [ActivityPub](/notes/activitypub/) Follow/Accept flow:

1. An instance follows the relay; the relay auto-accepts
2. When an instance sends an activity to the relay's inbox, the relay re-announces it to all other subscribers (excluding the original sender)
3. The relay wraps the original activity in a new `Announce` with itself as actor

In ActivityPub, the relay is typically an actor of type `Group` — which has the specific semantic of redistributing received content to its followers.

## When to Use Relays

- **Small networks (< 20 instances)**: direct federation is fine, N-squared overhead is manageable
- **Medium networks (20–200)**: one relay significantly reduces traffic
- **Large networks (200+)**: multiple regional or topic-based relays

The relay introduces a single point of throughput concern (all traffic routes through it) but not a single point of failure — if the relay goes down, instances can fall back to direct delivery.

## Related Concepts
- [Federation](/notes/federation/) — the network architecture where the relay pattern applies
- [ActivityPub](/notes/activitypub/) — the protocol that defines the Group actor type used for relays
- [Circuit breaker pattern](/notes/circuit-breaker-pattern/) — essential for relay implementations that must handle delivery failures to many endpoints
