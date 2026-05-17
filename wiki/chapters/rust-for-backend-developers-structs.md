---
title: "Rust for Backend Developers: Structs"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, structs, chapter]
source_count: 1
---

# Rust for Backend Developers: Structs

## Learning Goal

`struct`, `impl`, methods, tuple structs, unit-like structs, va struct lifetime'larini bir beginner-friendly mental modelga yig'ish.

## Main Ideas

- Struct named fields bilan custom type yaratadi.
- Field init shorthand va struct update syntax ergonomika beradi, lekin ownership qoidalari saqlanadi.
- Methods `impl` ichida yoziladi; `&self`, `&mut self`, va `self` receiverlari boshqa-boshqa semanticsga ega.
- Tuple struct va unit-like struct oddiy record structdan boshqa, lekin foydali variantlar.
- Reference field bo'lsa, struct lifetime annotation talab qiladi.

## Concepts

- [[structs]]
- [[struct-fields|struct fields]]
- [[struct-instances|struct instances]]
- [[field-init-shorthand]]
- [[struct-update-syntax]]
- [[methods]]
- [[impl-block|impl block]]
- [[associated-functions]]
- [[tuple-structs]]
- [[unit-like-structs]]
- [[lifetimes]]

## Examples

```rust
struct Person {
    first_name: String,
    last_name: String,
}
```

```rust
impl Person {
    fn new(first: &str, last: &str) -> Self {
        Self {
            first_name: first.to_string(),
            last_name: last.to_string(),
        }
    }
}
```

## Exercises

- `Account` struct yozing va `deposit(&mut self, amount: u64)` method qo'shing.
- Tuple struct bilan `Rgb(u8, u8, u8)` yarating va `.0`/`.1`/`.2` accessni sinab ko'ring.

## Review Questions

- Nega fieldni o'zgartirish uchun butun instance `mut` bo'lishi kerak?
- `..old_instance` qachon move qiladi, qachon copy qiladi?
- `&self`, `&mut self`, va `self` orasidagi ownership farqi nima?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-structs]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[structs]]
- [[methods]]
