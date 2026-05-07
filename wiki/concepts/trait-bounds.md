---
title: "Trait Bounds"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, traits, generics]
source_count: 1
---

# Trait Bounds

## Short Definition

Trait bound generic type parameter qaysi traitlarni implement qilishi kerakligini bildiradigan constraint.

## Why It Matters

Trait bounds generic code ichida qaysi methods yoki operations ishlatilishi mumkinligini compile time'da aniq qiladi. Bu Rustga flexible generic code va strong type checkingni birlashtirishga imkon beradi.

## Mental Model

`T: Summary` degani "`T` istalgan type emas; `Summary` implement qilgan type". Function body `T` ustida `summarize()` chaqira oladi. Multiple bounds `T: Summary + Display` bo'lsa, body `summarize()` ham, `{}` formatting ham ishlata oladi.

## Syntax and Examples

```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

Same concrete type for both parameters:

```rust
pub fn notify<T: Summary>(item1: &T, item2: &T) {
    println!("{} {}", item1.summarize(), item2.summarize());
}
```

Multiple bounds:

```rust
pub fn notify<T: Summary + Display>(item: &T) {
    println!("Breaking news! {}", item.summarize());
    println!("Display: {}", item);
}
```

## Common Mistakes

- `impl Trait` va `T: Trait` har doim bir xil expressiveness beradi deb o'ylash.
- Ikki parameter bir xil concrete type bo'lishi kerak bo'lsa ham `&impl Trait` ishlatish.
- `+` bilan multiple bounds yozilganda function body nima qila olishini aniq ko'rmaslik.
- Bounds ko'payganda [[where-clauses|where clause]] ishlatmasdan signature'ni o'qilmas qilish.

## Related Concepts

- [[traits]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[impl-trait|impl Trait]]
- [[where-clauses|where clauses]]
- [[generic-functions|generic functions]]
- [[display-formatting|Display]]
- [[partial-ord|PartialOrd]]
- [[conditional-method-implementations|conditional method implementations]]

## Sources

- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
