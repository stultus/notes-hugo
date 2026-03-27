---
title: "ActivityPub is the W3C protocol that powers the fediverse"
date: 2026-03-26
lastmod: 2026-03-26
draft: false
tags: ["distributed-systems", "federation", "protocols", "web-standards"]
summary: "ActivityPub is a W3C standard that defines how federated servers communicate using actors, inboxes, and a shared activity vocabulary."
status: "budding"
---

**ActivityPub** is a W3C protocol that defines how independent servers communicate in a [federated](/notes/federation/) network. It is the protocol behind Mastodon, Lemmy, BookWyrm, PeerTube, and the broader fediverse.

## Core Concepts

ActivityPub is built on three pillars:

**Actors** are JSON documents hosted at URLs that represent entities — a person, a service, an organization, or an entire application. Each actor has an inbox (where it receives messages), an outbox (where it publishes activities), and a public key for verifying identity. Actors are identified by handles in the format `@username@domain`, resolved via [WebFinger](/notes/webfinger/).

**Activities** are the vocabulary of communication. Every message is a combination of an actor (who), a verb (what), and an object (to what). The vocabulary comes from the W3C ActivityStreams 2.0 specification. Core activity types include:
- **Follow** / **Accept** / **Reject** — subscription management
- **Announce** — broadcasting/sharing content (like a "boost")
- **Create** / **Update** / **Delete** — content lifecycle
- **Undo** — cancelling a previous action

**Inbox/Outbox** is the communication pattern. All federation communication flows through inboxes. To communicate with an actor, POST an activity to their inbox. There is no direct message API, no RPC, no webhook — everything goes through the inbox. This means every server only needs to implement one receiving endpoint.

## How a Federated Interaction Works

1. Resolve the target's handle via WebFinger to find their actor document
2. Fetch the actor document to discover their inbox URL
3. POST an activity (e.g., Follow) to their inbox, signed with [HTTP Signatures](http-signatures-in-federation)
4. The receiving server verifies the signature, processes the activity, and responds (e.g., Accept) to the sender's inbox

Every step is an HTTP request to an inbox. The protocol is stateless at the transport layer — each POST is independent. State (who follows whom, what has been sent) is maintained by each instance in its own database.

## Data Format

ActivityPub uses JSON-LD (JSON for Linked Data). In practice, this means regular JSON with a `@context` field that declares the vocabulary. Mastodon and most implementations treat incoming activities as plain JSON and check for expected fields — a full JSON-LD processor is not required.

Servers use **content negotiation**: return JSON-LD for `application/activity+json` requests and HTML for browser visits to the same URL.

## Related Concepts
- [Federation](/notes/federation/) — the architectural pattern ActivityPub implements
- [WebFinger](/notes/webfinger/) — the discovery protocol for resolving handles to actor documents
- [HTTP Signatures in federation](http-signatures-in-federation) — how ActivityPub proves identity without shared secrets
- [Circuit breaker pattern](/notes/circuit-breaker-pattern/) — essential reliability pattern for federation delivery
