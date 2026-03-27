---
title: "Reciprocal Rank Fusion merges ranked lists without comparable scores"
date: 2026-03-26
lastmod: 2026-03-26
draft: false
tags: ["information-retrieval", "search", "algorithms"]
summary: "RRF combines ranked result lists from multiple sources using only rank positions, avoiding the problem of incomparable scoring systems."
status: "budding"
---

**Reciprocal Rank Fusion (RRF)** is an algorithm for merging ranked result lists from multiple independent sources. Its key insight: ignore raw scores entirely and use only rank positions. This sidesteps the problem that different systems use different scoring parameters, making their raw scores incomparable.

## The Formula

```
RRF_score(document) = Σ  1 / (k + rank_in_list)
                      for each list containing the document
```

where `k` is a constant (standard value: 60) that controls how much weight top-ranked results get relative to lower-ranked ones.

## Example

Document X appears at rank 2 in List A and rank 5 in List B:
```
RRF(X) = 1/(60+2) + 1/(60+5) = 0.01613 + 0.01538 = 0.03151
```

Document Y appears at rank 1 in List A but is absent from List B:
```
RRF(Y) = 1/(60+1) = 0.01639
```

Even though Y ranked higher in List A, X's presence in **both** lists gives it a higher fused score. This is a desirable property: documents confirmed by multiple independent sources are likely more relevant. Consensus across sources is a strong relevance signal.

## Why It Works

- **No score normalization needed** — only ordinal rankings matter
- **Rewards cross-source agreement** — appearing in multiple lists boosts a document
- **Simple to implement** — no machine learning, no parameter tuning beyond k
- **Robust** — works even when different lists use entirely different retrieval methods (e.g., combining keyword search with semantic vector search)

## Related Concepts
- [Federated search](/notes/federated-search/) — the primary use case where RRF merges results from independent indexes
