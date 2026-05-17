---
title: "Generic Enums"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, generics, enums]
source_count: 3
---

# Generic Enums

## Short Definition

Generic enum variant payload typelarini generic type parameterlar orqali ifodalaydigan enum.

## Why It Matters

Generic enums bir xil abstract meaningni turli concrete typelar bilan ishlatishga imkon beradi. `Option<T>` optional value conceptini, `Result<T, E>` esa success/failure conceptini har xil type'lar uchun reusable qiladi.

## Mental Model

`Option<T>` bitta generic parameterga ega: `Some(T)` value borligini, `None` value yo'qligini bildiradi. `Result<T, E>` ikkita generic parameterga ega: `Ok(T)` success value, `Err(E)` error value.

Generic enum abstractionni domain meaning bilan birlashtiradi: `Option<String>` va `Option<i32>` turli concrete typelar, lekin ikkalasi ham "value bor yoki yo'q" modelini ishlatadi.

Backend beginner sources `Option<T>` va `Result<T, E>`ni generic enum'ning eng amaliy ikki ko'rinishi sifatida ochadi. `Option<T>` absence modelini, `Result<T, E>` esa success/error modelini beradi. Yana muhim signal: `Some`, `None`, `Ok`, va `Err` shunchaki variant nomlari emas, constructor role ham bajaradi.

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

Amaliy foydalanish:

```rust
let maybe_i32: Option<i32> = Some(5);
let sqrt: Result<f32, String> = Ok(2.0);
```

## Common Mistakes

- `Option<T>`ni `T` bilan bir xil deb o'ylash.
- `Result<T, E>`dagi `T` va `E` nima anglatishini aralashtirish.
- `None` yoki `Err` kabi variantlardan type inference har doim yetarli bo'ladi deb kutish.
- Multiple enum definitionsni faqat payload type farqi uchun copy-paste qilish; generic enum ko'pincha aniqroq.
- Variant constructorlar va final enum type'ni aralashtirib yuborish.

## Related Concepts

- [[enums]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[option|Option]]
- [[result|Result<T, E>]]
- [[pattern-matching|pattern matching]]
- [[type-inference|type inference]]
- [[closures]]

## Sources

- [[10-1-generic-data-types]]
- [[wiki/sources/rust-for-backend-developers-option]]
- [[wiki/sources/rust-for-backend-developers-result]]
