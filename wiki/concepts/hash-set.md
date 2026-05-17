---
title: "HashSet"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, collections]
source_count: 1
---

# HashSet

## Short Definition

`std::collections::HashSet<T>` unique elementlar to'plami.

## Why It Matters

Membership check va uniqueness uchun payload'siz `HashMap` kerak bo'lganda ishlatiladi.

## Mental Model

Conceptual jihatdan `HashMap<T, ()>`ga yaqin. Element type odatda `Eq + Hash` contractiga tayanadi.

## Syntax and Examples

```rust
use std::collections::HashSet;

let mut set = HashSet::new();
set.insert("one".to_string());
```

## Common Mistakes

- Stable order kutish.
- Duplicate insertion eski qiymatni "update" qiladi deb o'ylash.

## Related Concepts

- [[collections]]
- [[hash-map|HashMap]]
- [[eq-trait|Eq]]
- [[hash-trait|Hash]]

## Sources

- [[wiki/sources/rust-for-backend-developers-collections]]

