---
title: "LinkedList"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, collections]
source_count: 1
---

# LinkedList

## Short Definition

`std::collections::LinkedList<T>` ikki tomonlama bog'langan ro'yxat.

## Why It Matters

Front/back insert-remove qulay, lekin random indexing qimmat.

## Mental Model

Har element node bo'lib, qo'shnilariga pointerlar ushlaydi. Shuning uchun middle insert/delete position allaqachon topilgan bo'lsa qulay, lekin index bo'yicha access `O(n)`.

## Syntax and Examples

```rust
use std::collections::LinkedList;

let mut list = LinkedList::new();
list.push_back(1);
list.push_front(0);
```

## Common Mistakes

- `Vec` o'rniga har doim tezroq deb o'ylash.
- Random access workload'ida tanlash.

## Related Concepts

- [[collections]]
- [[vector|Vec<T>]]
- [[vecdeque|VecDeque]]

## Sources

- [[wiki/sources/rust-for-backend-developers-collections]]

