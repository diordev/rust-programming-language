---
title: "Rust for Backend Developers: Enums"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, enums, chapter]
source_count: 1
---

# Rust for Backend Developers: Enums

## Learning Goal

`enum`ni oddiy named constants emas, payload tashiy oladigan ADT sifatida tushunish; `match`, `if let`, methods, discriminant, va enum memory modeli haqida ehtiyotkor mental model qurish.

## Main Ideas

- Rust enum ikki ko'rinishni bitta modelga yig'adi: C-like unit variants va payload tashiydigan tuple/struct-like variants.
- `Shape::Square { width }` va `Shape::Rectangle { width, height }` misollari enum'ning "bitta type, bir nechta shape" kuchini ko'rsatadi.
- `match` enum bilan ishlashning default instrumenti, chunki u exhaustive handlingni majbur qiladi.
- `if let` faqat bitta variant qiziq bo'lganda qisqaroq syntax beradi, lekin exhaustivenessni yo'qotadi.
- Numeric discriminant berish mumkin, lekin memory layoutni haddan tashqari aniq deb o'qish xato; bu yerda operational mental model yetarli.

## Concepts

- [[enums]]
- [[enum-variants|enum variants]]
- [[match]]
- [[if-let|if let]]
- [[methods]]

## Examples

```rust
enum HttpStatus {
    Ok = 200,
    NotFound = 404,
}
```

```rust
enum Shape {
    Square { width: f32 },
    Rectangle { width: f32, height: f32 },
}
```

```rust
if let Shape::Square { width } = s {
    println!("{width}");
}
```

## Exercises

- `Shape`ga yangi variant qo'shib, qaysi `match`lar darhol sinishini tekshiring.
- C-like enum va payload enum uchun bitta-bittadan API yozing.
- `match` va `if let`ni bir xil inputda solishtiring.

## Review Questions

- Nega Rust enum "qiymatlar ro'yxati"dan ko'ra "variants ro'yxati" deb o'qilgani to'g'riroq?
- `if let` qachon `match`dan yaxshiroq, qachon yomonroq?
- Numeric discriminant enum memory layoutini to'liq belgilaydimi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-enums]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[enums]]
- [[enum-variants|enum variants]]
- [[match]]
- [[if-let|if let]]
