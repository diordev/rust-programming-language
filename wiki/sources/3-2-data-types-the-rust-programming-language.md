---
title: "3.2. Data Types - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, types]
source_count: 1
source_path: "raw/books/the_rust_programming_language/3.2. Data Types - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch03-02-data-types.html"
---

# 3.2. Data Types - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/3.2. Data Types - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch03-02-data-types.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section Rustdagi [[data-types|data types]]ni tushuntiradi. Rust statically typed language: compiler compile time'da har bir variable typeini bilishi kerak. Ko'p hollarda compiler type inference qiladi, lekin bir nechta type mumkin bo'lsa, explicit annotation kerak bo'ladi. Masalan, `"42".parse()` qaysi numeric typega parse qilinishini bilmaydi; `let guess: u32 = ...` annotation kerak. Annotation bo'lmasa `E0284 type annotations needed` chiqadi.

Rust primitive data types ikkiga bo'linadi: [[scalar-types|scalar types]] va [[compound-types|compound types]]. Scalar type single value ifodalaydi. Rustdagi primary scalar types: integers, floating-point numbers, Booleans, va characters.

Integer types signed (`i8`, `i16`, `i32`, `i64`, `i128`, `isize`) va unsigned (`u8`, `u16`, `u32`, `u64`, `u128`, `usize`) turlarga bo'linadi. Default integer type `i32`. `isize` va `usize` architecture-dependent; collection indexingda ko'p ishlatiladi. Integer literals decimal, hex, octal, binary, byte literal ko'rinishida yozilishi mumkin; `_` visual separator sifatida ishlatiladi.

Integer overflow debug buildda runtime panic qiladi. Release buildda overflow two's complement wrapping behaviorga o'tadi. Overflow behaviorga tayanish error hisoblanadi; explicit handling uchun `wrapping_*`, `checked_*`, `overflowing_*`, `saturating_*` method families bor.

Floating point types `f32` va `f64`; default `f64`. Boolean type `bool`, values `true` va `false`. `char` single quotes bilan yoziladi, 4 bytes, Unicode scalar value ifodalaydi; bu human "character" intuitioniga har doim mos kelmasligi mumkin.

Compound types multiple valuesni bitta type ichida guruhlaydi. Tuple turli typelarni fixed length bilan birlashtiradi. Tuple destructuring yoki dot-index (`x.0`) orqali ishlatiladi. Empty tuple `()` unit deb ataladi va empty value / empty return type sifatida ishlatiladi.

Array bir xil type elementlardan iborat fixed-length collection. Array stackda allocated bo'lishi mumkin va length o'zgarmaydi. Type notation `[i32; 5]`; repeated value initialization `[3; 5]`. Array indexing runtime bounds check qiladi. Out-of-bounds index Rustda panic qiladi va invalid memory accessga yo'l qo'ymaydi; bu Rust memory safety principlesdan biri.

## Key Concepts

- [[data-types|data types]]
- [[type-inference|type inference]]
- [[type-annotations|type annotations]]
- [[scalar-types|scalar types]]
- [[compound-types|compound types]]
- [[integer-types|integer types]]
- [[integer-overflow|integer overflow]]
- [[boolean-type|bool]]
- [[char-type|char]]
- [[tuple]]
- [[array]]
- [[unit-type|unit type]]
- [[e0284-type-annotations-needed|E0284 type annotations needed]]

## Code Examples

Type annotation:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

Tuple destructuring:

```rust
let tup = (500, 6.4, 1);
let (x, y, z) = tup;
println!("The value of y is: {y}");
```

Array:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
let first = a[0];
```

## Exercises or Practice Ideas

- `parse()`dan type annotationni olib tashlab `E0284`ni ko'rish.
- Integer literal formats: decimal, hex, octal, binary, byte literal yozib ko'rish.
- Tuple destructuring va dot-index accessni solishtirish.
- Array out-of-bounds indexing panicini kuzatish.

## Questions Raised

- Qachon `usize`/`isize` ishlatish kerak?
- Debug va release build integer overflow farqi production codega qanday ta'sir qiladi?
- Array va vector tanlashda qaysi mezonlar bor?

## Links To Update

- [[data-types|data types]]
- [[e0284-type-annotations-needed|E0284 type annotations needed]]
- [[e0308-mismatched-types|E0308 mismatched types]]
- [[array]]
- [[tuple]]
