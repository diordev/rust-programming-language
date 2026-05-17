---
title: "Collections"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, collections]
source_count: 5
---

# Collections

## Short Definition

Collections bir nechta value'ni bitta data structure ichida saqlaydigan standard library typelari.

## Why It Matters

Real kodda data ko'pincha bitta value emas, ro'yxat, queue, set, map, yoki priority queue ko'rinishida keladi. To'g'ri collection tanlovi API va performance'ni birga belgilaydi.

## Mental Model

`array` va `tuple` compile-time shape'ga ega built-in compound types. Common collections esa standard library typelari bo'lib, ko'pincha heap allocation ishlatadi va runtime'da hajmini o'zgartira oladi.

Chapter 8 uchta common collectionni bergan edi:

- [[vector|Vec<T>]]
- [[string-type|String]]
- [[hash-map|HashMap]]

Advance collections source bu modelni kengaytiradi:

- [[linked-list|LinkedList]] — front/back insert-delete uchun node-based ro'yxat
- [[vecdeque|VecDeque]] — front/back queue workload uchun ring buffer
- [[hash-set|HashSet]] — unique elementlar to'plami
- [[btree-map|BTreeMap]] — sorted key-value map
- [[binary-heap|BinaryHeap]] — priority queue

Durable qoida: collection tanlash "eng tez" degan bitta gap bilan emas, operation pattern bilan qilinadi.

## Syntax and Examples

```rust
let numbers = vec![1, 2, 3];
let text = String::from("hello");
```

```rust
use std::collections::{HashMap, VecDeque};

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);

let mut queue = VecDeque::from([1, 2, 3]);
queue.push_front(0);
```

## Common Mistakes

- Runtime'da o'sadigan list uchun fixed-size [[array]] ishlatish.
- Har bir collectionning capability va cost'i bir xil deb o'ylash.
- `LinkedList` yoki `VecDeque`ni "middle operation bor" degan bitta sabab bilan avtomatik tanlash.
- Sorted order kerak bo'lsa ham `HashMap` ishlatish.
- Priority queue kerak bo'lsa oddiy sorted vector bilan qolib ketish.

## Related Concepts

- [[standard-library|standard library]]
- [[vector|Vec<T>]]
- [[string-type|String]]
- [[hash-map|HashMap]]
- [[linked-list|LinkedList]]
- [[vecdeque|VecDeque]]
- [[hash-set|HashSet]]
- [[btree-map|BTreeMap]]
- [[binary-heap|BinaryHeap]]
- [[stack-and-heap|stack and heap]]
- [[ownership]]
- [[borrowing]]

## Sources

- [[wiki/sources/8-common-collections]]
- [[8-1-storing-lists-of-values-with-vectors]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[wiki/sources/rust-for-backend-developers-collections]]

