---
title: "Static Dispatch"
type: concept
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, traits, generics, performance]
source_count: 1
---

# Static Dispatch

## Short Definition

Static dispatch — qaysi function yoki method implementation chaqirilishi compile time'da ma'lum bo'ladigan dispatch modeli.

## Why It Matters

Traits bilan polymorphism ishlatilganda static dispatch odatda zero-cost yo'l bo'ladi: compiler concrete type uchun kodni specialize qiladi va vtable kerak bo'lmaydi.

## Mental Model

`&impl Trait` yoki `T: Trait` ko'rgan compiler "runtime'da qaysi type kelishini kutaman" demaydi. U chaqirilgan concrete typelar bo'yicha alohida kod yaratadi. Shu sabab static dispatch ko'pincha [[monomorphization]] bilan birga tushuniladi.

## Syntax and Examples

```rust
fn print_introduction(v: &impl CanIntroduce) {
    println!("{}", v.introduce());
}
```

Generic bound bilan xuddi shu model:

```rust
fn print_introduction<T: CanIntroduce>(v: &T) {
    println!("{}", v.introduce());
}
```

## Common Mistakes

- `impl Trait` parameterini dynamic dispatch deb o'ylash.
- Static dispatch bo'lsa har doim bitta umumiy binary fragment ishlaydi deb o'ylash; aslida concrete type'lar bo'yicha kod ko'payishi mumkin.

## Related Concepts

- [[traits]]
- [[impl-trait|impl Trait]]
- [[trait-bounds|trait bounds]]
- [[monomorphization]]
- [[dynamic-dispatch|dynamic dispatch]]
- [[polymorphism]]

## Sources

- [[wiki/sources/rust-for-backend-developers-traits]]
