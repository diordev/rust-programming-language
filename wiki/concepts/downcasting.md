---
title: "Downcasting"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, trait-objects, runtime-types]
source_count: 1
---

# Downcasting

## Short Definition

Trait object yoki boshqa erased abstraction ortidagi concrete type'ga qaytish jarayoni.

## Why It Matters

Ba'zi adapter, plugin, yoki panic payload scenariylarida concrete type'ni qayta ajratish kerak bo'lishi mumkin.

## Mental Model

Rust'da safe amaliy downcast odatda `Any` orqali qilinadi: avval type check, keyin `downcast_ref` yoki `downcast_mut`. Qo'lda pointer cast qilish default emas.

## Syntax and Examples

```rust
use std::any::Any;

fn inspect(value: &dyn Any) {
    if let Some(n) = value.downcast_ref::<u32>() {
        println!("{n}");
    }
}
```

## Common Mistakes

- Downcast'ni polymorphismning asosiy strategiyasi qilish.
- `transmute` asosidagi demo'ni production pattern deb o'qish.

## Related Concepts

- [[any-trait|Any]]
- [[type-id|TypeId]]
- [[trait-object]]

## Sources

- [[wiki/sources/rust-for-backend-developers-any]]

