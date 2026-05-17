---
title: "BinaryHeap"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, collections]
source_count: 1
---

# BinaryHeap

## Short Definition

`std::collections::BinaryHeap<T>` priority queue.

## Why It Matters

Har safar eng katta yoki eng prioritetli elementni olish kerak bo'lsa, sorted vector'dan ko'ra mosroq model beradi.

## Mental Model

Standard implementation max-heap: `pop()` eng katta elementni qaytaradi. Priority comparison `Ord` orqali belgilanadi.

## Syntax and Examples

```rust
use std::collections::BinaryHeap;

let mut heap = BinaryHeap::new();
heap.push(9);
heap.push(2);
assert_eq!(heap.pop(), Some(9));
```

## Common Mistakes

- Uni oddiy binary tree deb o'ylash.
- Iteration order sorted bo'ladi deb kutish.

## Related Concepts

- [[collections]]
- [[ord-trait|Ord]]

## Sources

- [[wiki/sources/rust-for-backend-developers-collections]]

