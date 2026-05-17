---
title: "Trait Object VTable"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, trait-objects, runtime]
source_count: 1
---

# Trait Object VTable

## Short Definition

`trait object`ning metadata qismi bo'lib, dynamic dispatch uchun kerakli function pointer va layout ma'lumotlarini tutadi.

## Why It Matters

Vtable nima qilishi va nima qilmasligini tushunmaslik `Any`/downcast mavzusida noto'g'ri xulosa beradi.

## Mental Model

`&dyn Trait` conceptually data pointer + metadata'dan iborat. Shu metadata odatda method call, destructor, size, alignment kabi narsalar uchun xizmat qiladi; u universal runtime type registry emas.

## Syntax and Examples

```rust
// conceptual model:
// (&data, &vtable)
```

## Common Mistakes

- Vtable ichida concrete type name yoki generic `TypeId` avtomatik turadi deb o'ylash.
- Vtable'ni `Any` bilan bir narsa deb ko'rish.

## Related Concepts

- [[trait-object]]
- [[dynamic-dispatch]]
- [[any-trait|Any]]
- [[type-id|TypeId]]

## Sources

- [[wiki/sources/rust-for-backend-developers-any]]

