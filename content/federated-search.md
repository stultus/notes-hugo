---
title: "Federated search distributes queries across independent indexes"
date: 2026-03-26
lastmod: 2026-03-26
draft: false
tags: ["distributed-systems", "federation", "search", "information-retrieval"]
summary: "Federated search solves the problem of finding the best results across N independent search indexes without querying all of them."
status: "budding"
---

**Federated search** is the challenge of finding relevant results across many independent search indexes without querying all of them. It is a well-studied problem in information retrieval, and the core enabler of [federated](federation) search engines.

A centralized search engine (Google) maintains one index of the entire web, requiring enormous infrastructure. A federated search engine distributes that burden: Instance A indexes climate science, Instance B indexes local news, Instance C indexes open-source docs. No single instance indexes everything. No single operator controls the whole system.

## The Three Stages

### 1. Resource Selection — which indexes to query

Given 100 federated instances and a query, you cannot query all of them — it would be too slow. You need to determine which instances are most likely to have relevant results.

This uses **collection summaries**: compact representations of what each instance contains (e.g., topic vectors). For each remote instance, compute similarity between the query and the instance's summary, rank instances by relevance, and select the top K. This is a local operation — all summaries are cached. No network requests needed.

The classic algorithm for this is **CORI (Collection Retrieval Inference)**.

### 2. Query Forwarding — sending the query to selected indexes

The query is sent to the K selected instances. Each executes the query against its local index and returns results.

### 3. Result Merging — combining results from multiple sources

Results from K instances plus the local instance need to be merged into a single ranked list. The challenge: different instances may use different scoring parameters, making raw scores incomparable.

[Reciprocal Rank Fusion](reciprocal-rank-fusion) solves this elegantly by ignoring raw scores and using only rank positions.

## Real-World Implementations

**BookWyrm** (federated Goodreads alternative) has the most mature pattern: it searches the local database, remote BookWyrm instances via [ActivityPub](activitypub), and external APIs, then merges all results. Its search is sequential and blocking — a known performance issue that async approaches can solve.

**Lemmy** and **Misskey** only search locally-federated content (content already delivered to the instance). They have no query-forwarding mechanism — a gap that true federated search addresses.

**PeerTube's Sepia Search** takes the opposite approach: a centralized crawler indexes all PeerTube instances. This works but reintroduces the central point that federation aims to eliminate.

## Related Concepts
- [Federation](federation) — the network architecture that federated search operates within
- [Reciprocal Rank Fusion](reciprocal-rank-fusion) — the algorithm for merging ranked results from multiple sources
- [A Note on Distributed Computing](a-note-on-distributed-computing-paper) — the fundamental challenges of distributed systems apply here too
