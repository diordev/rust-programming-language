---
title: "Generic Enums"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, generics, enums]
source_count: 1
---

# Generic Enums

## Short Definition

Generic enum variant payload typelarini generic type parameterlar orqali ifodalaydigan enum.

## Why It Matters

Generic enums bir xil abstract meaningni turli concrete typelar bilan ishlatishga imkon beradi. `Option<T>` optional value conceptini, `Result<T, E>` esa success/failure conceptini har xil type'lar uchun reusable qiladi.

## Mental Model

`Option<T>` bitta generic parameterga ega: `Some(T)` value borligini, `None` value yo'qligini bildiradi. `Result<T, E>` ikkita generic parameterga ega: `Ok(T)` success value, `Err(E)` error value.

Generic enum abstractionni domain meaning bilan birlashtiradi: `Option<String>` va `Option<i32>` turli concrete typelar, lekin ikkalasi ham "value bor yoki yo'q" modelini ishlatadi.

## Syntax and Examples

```rust
enum Option<T> {
    Some(T),
    None,
}
```

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

## Common Mistakes

- `Option<T>`ni `T` bilan bir xil deb o'ylash.
- `Result<T, E>`dagi `T` va `E` nima anglatishini aralashtirish.
- `None` yoki `Err` kabi variantlardan type inference har doim yetarli bo'ladi deb kutish.
- Multiple enum definitionsni faqat payload type farqi uchun copy-paste qilish; generic enum ko'pincha aniqroq.

## Related Concepts

- [[enums]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[option|Option]]
- [[result|Result<T, E>]]
- [[pattern-matching|pattern matching]]
- [[type-inference|type inference]]

## Sources

- [[10-1-generic-data-types]]
