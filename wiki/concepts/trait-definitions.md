---
title: "Trait Definitions"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-16
tags: [rust, traits]
source_count: 2
---

# Trait Definitions

## Short Definition

Trait definition type'lar share qilishi kerak bo'lgan method signatures yoki default methods to'plamini belgilaydi.

## Why It Matters

Trait definition shared behaviorni abstract contractga aylantiradi. Generic code keyin shu contractga tayanib, concrete type nomini bilmasdan method chaqira oladi.

## Mental Model

Trait "bu behavior mavjud bo'lishi kerak" degan ro'yxat. Method signature semicolon bilan tugasa, implementor body yozishi shart. Method body trait ichida berilsa, bu [[default-trait-implementations|default implementation]] bo'ladi. Trait definition ichida concrete type nomi noma'lum bo'lgani uchun `Self` ham ko'p ishlatiladi: masalan `fn make_default() -> Self;`.

Trait `pub` bo'lsa, crate users uni import qilib ishlatishi yoki local typelari uchun implement qilishi mumkin.

## Syntax and Examples

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

Multiple methods:

```rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
```

## Common Mistakes

- Trait method signature'ida semicolon o'rniga bo'sh body kutish.
- Trait `pub` bo'lmasa external crate users uni ishlata olmaydi deb hisobga olmaslik.
- Trait definitionni implementation bilan chalkashtirish.
- Default method boshqa required methodni chaqirishi mumkinligini unutish.

## Related Concepts

- [[traits]]
- [[trait-implementations|trait implementations]]
- [[default-trait-implementations|default trait implementations]]
- [[public-api|public API]]
- [[generic-functions|generic functions]]

## Sources

- [[10-2-defining-shared-behavior-with-traits]]
- [[wiki/sources/rust-for-backend-developers-traits]]
