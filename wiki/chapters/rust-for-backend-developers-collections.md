---
title: "Rust for Backend Developers: Collections"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, chapter, collections, advance]
source_count: 1
---

# Rust for Backend Developers: Collections

## Learning Goal

Collection tanlashni nominal nom bilan emas, access/update/order workload'i bilan bog'lash.

## Main Ideas

- `Vec` ko'p hollarda default.
- `VecDeque` front/back queue workload'lari uchun.
- `HashMap`/`HashSet` hashing asosida, `BTreeMap` ordering asosida ishlaydi.
- `BinaryHeap` priority queue.

## Concepts

- [[collections]]
- [[linked-list|LinkedList]]
- [[vecdeque|VecDeque]]
- [[hash-map|HashMap]]
- [[hash-set|HashSet]]
- [[btree-map|BTreeMap]]
- [[binary-heap|BinaryHeap]]

## Examples

```rust
let mut q = std::collections::VecDeque::from([1, 2, 3]);
q.push_front(0);
```

## Exercises

- Bitta queue scenario uchun `VecDeque`ni, bitta sorted lookup scenario uchun `BTreeMap`ni tanlang va sababini yozing.

## Review Questions

- `LinkedList` qachon foydali, qachon ortiqcha?
- `HashMap` va `BTreeMap` o'rtasidagi asosiy contract farqi nima?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-collections]]
- [[collections]]
- [[hash-map|HashMap]]
- [[vecdeque|VecDeque]]

