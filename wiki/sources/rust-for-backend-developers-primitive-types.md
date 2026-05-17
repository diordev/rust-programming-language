---
title: "Примитивные типы данных - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, types]
source_count: 1
---

# Примитивные типы данных - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/9. Примитивные типы данных.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/primitive-types.html

## Detailed Summary

Bu source Rustdagi primitive type qatlamini compact reference sifatida beradi: signed/unsigned integerlar, floating-point typelar, `bool`, `char`, `unit`, `never type`, va explicit type conversion.

Integer qismi o'lchamga qarab `i8`/`u8` dan `i128`/`u128` gacha va pointer-width bilan bog'liq `isize`/`usize`ni sanab o'tadi. Muqarrar amaliy detail: explicit type bo'lmasa butun son literal odatda `i32`, floating literal odatda `f64` bo'ladi. Source suffix notationni ham ko'rsatadi: `5u8`, `5.0f32`, `1u128`.

Floating-point bo'limi Rustning `f32` va `f64` type'lari IEEE-754 modeliga tayanishini eslatadi. Natijada oddiy arifmetik bilan birga `inf` va `NaN` kabi qiymatlar ham bor. Bu backend code uchun muhim, chunki numeric parsing va external inputlar bilan ishlaganda oddiy "hamma sonlar oddiy real son" degan taxmin yetmaydi.

`bool` faqat `true` yoki `false`, `char` esa 4-byte Unicode scalar value sifatida tushuntiriladi. Bu `char`ni byte yoki UTF-8 code unit bilan aralashtirmaslik uchun kerak.

Source `()`ni unit type sifatida, ya'ni "voidga o'xshash, lekin bitta qiymatga ega type" deb beradi. `!` esa never type sifatida keyinroq batafsil tushuntiriladigan, hech qachon caller'ga value qaytarmaydigan expressions bilan bog'liq type deb qisqacha belgilanadi.

Oxirida explicit type conversion uchun `as` operatori ko'rsatiladi. Muhim nuqta: Rust implicit conversion qilmaydi. Har qanday type cast kodda ochiq yoziladi. Source bool, float, char, integer misollarini beradi; wiki uchun qo'shimcha ehtiyotkorlik shuki, `as` ba'zi conversionlarda truncation yoki ma'lumot yo'qotishiga ham olib kelishi mumkin.

## Key Concepts

- [[scalar-types|scalar types]]
- [[boolean-type|bool]]
- [[unit-type|unit type]]
- [[never-type|never type (!)]]
- [[type-casting]]
- [[type-inference|type inference]]
- [[type-annotations|type annotations]]
- [[data-types|data types]]

## Code Examples

```rust
let a = 5;      // i32
let b = 5.0;    // f64
let c = 5u8;
let d = 5.0f32;
```

```rust
let flag: bool = true;
let letter: char = 'A';
let smile = '☺';
```

```rust
let unit_value: () = ();
```

```rust
let n: i32 = 5;
let wide: i64 = n as i64;
let code = 'A' as i32;
let bit = true as i32;
```

## Exercises or Practice Ideas

- Default type inference va suffix notation farqini ko'rsatadigan 6 ta literal yozing.
- `char`, `u8`, va `String` bir xil narsa emasligini ko'rsatadigan qisqa note yozing.
- `as` bilan bajarilgan conversionlardan qaysilari ma'no yo'qotishi mumkinligini sanang.

## Questions Raised

- `as` qachon oddiy cast, qachon xavfli shortcut bo'lib qoladi?
- `unit type` va `never type` function body expressions bilan qanday bog'lanadi?
- `isize`/`usize`ni amaliy API design'da qachon ishlatish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-primitive-types]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[scalar-types|scalar types]]
- [[unit-type|unit type]]
- [[never-type|never type (!)]]
- [[type-casting]]
