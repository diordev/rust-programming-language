---
title: "HashMap"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, collections]
source_count: 3
---

# HashMap

## Short Definition

`HashMap<K, V>` key bilan value'ni bog'laydigan standard library map collection.

## Why It Matters

Meaningful key orqali tez lookup kerak bo'lsa, vector indexidan ko'ra shu model to'g'ri.

## Mental Model

`HashMap` key'ni hashing qilib storage slot topadi. Data heapda saqlanadi va map runtime'da o'sishi mumkin. Muhim contract key tomonda: odatdagi map semantics uchun `K` `[[eq-trait|Eq]] + [[hash-trait|Hash]]` bo'lishi kerak.

`HashMap`ning asosiy tradeoff'i:

- sorted order yo'q
- iteration order stable emas
- membership/lookup/update kuchli

## Syntax and Examples

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
```

```rust
if let Some(score) = scores.get("Blue") {
    println!("{score}");
}
```

## Common Mistakes

- Iteration order stable bo'ladi deb o'ylash.
- Value type hamisha `PartialEq` talab qiladi deb o'ylash.
- Owned key/value insert qilingandan keyin oldingi binding ishlayveradi deb kutish.
- Sorted iteration kerak bo'lsa ham `HashMap` bilan qolish.

## Related Concepts

- [[collections]]
- [[hash-set|HashSet]]
- [[btree-map|BTreeMap]]
- [[entry-api|entry API]]
- [[hashing-function|hashing function]]
- [[eq-trait|Eq]]
- [[hash-trait|Hash]]
- [[ownership]]

## Sources

- [[wiki/sources/8-common-collections]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[wiki/sources/rust-for-backend-developers-collections]]

