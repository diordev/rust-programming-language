---
title: "DerefMut Trait"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, traits, deref, mutability]
source_count: 2
---

# DerefMut Trait

## Short Definition

`DerefMut` trait mutable dereference behaviorini custom qiladi va `&mut T`dan `&mut U`ga deref coercion qilishga imkon beradi.

## Why It Matters

Mutable smart pointer yoki wrapper ichidagi value'ni mutable reference kabi ishlatish kerak bo'lsa, `DerefMut` borrowing rules bilan birga ishlaydi.

## Mental Model

`Deref` immutable access uchun, `DerefMut` mutable access uchun. Rust mutable reference'ni immutable reference'ga pasaytira oladi, lekin immutable reference'dan mutable reference yasamaydi.

## Syntax and Examples

Chapter 15.2 coercion qoidalari:

1. `&T` -> `&U` agar `T: Deref<Target = U>`
2. `&mut T` -> `&mut U` agar `T: DerefMut<Target = U>`
3. `&mut T` -> `&U` agar `T: Deref<Target = U>`

Oddiy `Box<T>` mutation:

```rust
let mut b = Box::new(1);
*b = 2;
```

## Common Mistakes

- `&T`dan `&mut U`ga coercion bo'ladi deb o'ylash.
- Mutable deref borrowing qoidalarini chetlab o'tadi deb o'ylash.
- `DerefMut`ni interior mutability bilan bir xil deb o'ylash; interior mutability keyingi bo'limlarda alohida pattern sifatida keladi.

## Related Concepts

- [[deref-trait|Deref trait]]
- [[deref-coercions]]
- [[mutable-reference|mutable reference]]
- [[borrowing]]
- [[smart-pointers]]

## Sources

- [[15-2-treating-smart-pointers-like-regular-references]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
