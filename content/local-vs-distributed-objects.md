---
title: Local vs Distributed Objects
date: 2024-11-05
draft: false
tags: ["software-architecture", "distributed-systems"]
status: "seeding"
summary: "The fundamental differences between local and distributed object interactions."
---

The distinction between **Local and Distributed Objects** is not just an implementation detail; it is a fundamental difference that cannot be fully abstracted.

Key differences typically include:
1. **Latency**: Remote calls are orders of magnitude slower.
2. **Memory Access**: Separate address spaces mean no shared state/pointers.
3. **Partial Failure**: A remote machine can fail while the local one keeps running (and vice-versa), unlike a single local process.
4. **Concurrency**: Distributed systems are inherently concurrent.

Attempts to make distribution transparent (hiding these differences) often lead to fragile and inefficient systems.

## Related
- [A Note on Distributed Computing](a-note-on-distributed-computing-paper)

---
*Source: [Paper: A Note On Distributed Computing](paper-a-note-on-distributed-computing)*

