---
title: "BTreeMap"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, collections]
source_count: 1
---

# BTreeMap

## Short Definition

`std::collections::BTreeMap<K, V>` sorted key-value map.

## Why It Matters

Stable sorted iteration va comparison-based key ordering kerak bo'lsa foydali.

## Mental Model

API ko'p jihatdan `HashMap`ga o'xshaydi, lekin key contracti `Eq + Hash` emas, `Ord`. Shu sabab elementlar key bo'yicha tartiblangan bo'ladi.

## Syntax and Examples

```rust
use std::collections::BTreeMap;

let mut map = BTreeMap::new();
map.insert(2, "b");
map.insert(1, "a");
```

## Common Mistakes

- `HashMap` bilan bir xil performance/order expectations ishlatish.

## Related Concepts

- [[collections]]
- [[hash-map|HashMap]]
- [[ord-trait|Ord]]

## Sources

- [[wiki/sources/rust-for-backend-developers-collections]]

