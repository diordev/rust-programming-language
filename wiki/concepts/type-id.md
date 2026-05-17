---
title: "TypeId"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, types, runtime-types]
source_count: 1
---

# TypeId

## Short Definition

`TypeId` concrete type uchun unique runtime-comparable identifier.

## Why It Matters

`Any` va downcast logic'i aynan `TypeId` orqali concrete type'ni ajratadi.

## Mental Model

`TypeId::of::<T>()` bir type'ning identity tokenini beradi. Bu human-readable type name emas, comparison uchun identifier.

## Syntax and Examples

```rust
use std::any::TypeId;

let a = TypeId::of::<String>();
let b = TypeId::of::<u32>();
assert_ne!(a, b);
```

## Common Mistakes

- `TypeId`ni stable external serialization ID deb o'ylash.
- `TypeId` borligi trait object reflection hamma joyda bor degani deb o'ylash.

## Related Concepts

- [[any-trait|Any]]
- [[downcasting]]
- [[trait-object-vtable]]

## Sources

- [[wiki/sources/rust-for-backend-developers-any]]

