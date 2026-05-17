---
title: "VecDeque"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, collections]
source_count: 2
---

# VecDeque

## Short Definition

`std::collections::VecDeque<T>` growable ring buffer.

## Why It Matters

Front va back'da queue-like operationlar kerak bo'lsa `Vec`ga qaraganda mosroq.

## Mental Model

`VecDeque` contiguous logical sequence beradi, lekin ichkarida ring buffer kabi ishlaydi. Front/back push-pop samarali, middle insert/remove esa hali ham umumiy tez yechim emas.

## Syntax and Examples

```rust
use std::collections::VecDeque;

let mut q = VecDeque::from([1, 2, 3]);
q.push_front(0);
q.pop_back();
```

## Common Mistakes

- Middle operationlar hamisha arzon deb o'ylash.
- Oddiy append-only list uchun `Vec` o'rniga avtomatik tanlash.

## Related Concepts

- [[collections]]
- [[linked-list|LinkedList]]
- [[read-trait|Read]]

## Sources

- [[wiki/sources/rust-for-backend-developers-collections]]
- [[wiki/sources/rust-for-backend-developers-io]]

