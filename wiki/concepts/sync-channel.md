---
title: "sync_channel"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, concurrency, channels, backpressure]
source_count: 1
---

# sync_channel

## Short Definition

`std::sync::mpsc::sync_channel(bound)` bounded capacity'li channel bo'lib, to'lsa sender'ni block qiladi.

## Why It Matters

Producer throughput'ni consumer tezligiga bog'lab, backpressure beradi.

## Mental Model

Cheklangan navbat. Joy bo'lsa yozasiz, to'lsa kutasiz.

## Syntax and Examples

```rust
let (tx, rx) = std::sync::mpsc::sync_channel::<i32>(3);
```

## Common Mistakes

- `channel()` bilan bir xil deb o'ylash.
- Block semantics sabab sender thread ham kutishi mumkinligini unutish.

## Related Concepts

- [[channels]]
- [[message-passing|message passing]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multithreading]]
