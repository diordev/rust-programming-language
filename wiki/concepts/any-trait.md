---
title: "Any"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, traits, runtime-types]
source_count: 1
---

# Any

## Short Definition

`std::any::Any` runtime'da concrete type identity bilan ishlashga imkon beradigan standard trait.

## Why It Matters

`trait object` bilan ishlaganda ba'zan concrete type'ni tekshirish yoki safe downcast qilish kerak bo'ladi. Standard yo'l shu trait.

## Mental Model

Oddiy `dyn Trait` reflection bermaydi. `dyn Any` esa `TypeId` asosida `is::<T>()`, `downcast_ref::<T>()`, `downcast_mut::<T>()` kabi API beradi.

## Syntax and Examples

```rust
use std::any::Any;

fn inspect(value: &dyn Any) {
    if let Some(s) = value.downcast_ref::<String>() {
        println!("{s}");
    }
}
```

## Common Mistakes

- `Any`ni universal serialization yoki debugging mexanizmi deb o'ylash.
- `Any` bo'lsa ham downcast'ni default API dizaynga aylantirish.
- `'static` bound impactini unutish.

## Related Concepts

- [[type-id|TypeId]]
- [[downcasting]]
- [[trait-object]]
- [[trait-object-vtable]]
- [[static-lifetime|static lifetime]]

## Sources

- [[wiki/sources/rust-for-backend-developers-any]]

