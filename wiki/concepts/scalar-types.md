---
title: "Scalar Types"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, types]
source_count: 2
---

# Scalar Types

## Short Definition

Scalar type single value ifodalaydi. Rustdagi primary scalar types: integers, floating-point numbers, `bool`, va `char`.

## Why It Matters

Scalar types Rustdagi arithmetic, conditionals, indexing, va simple values uchun asos.

## Mental Model

- Integers: `i32`, `u32`, `usize`, va boshqalar.
- Floats: `f32`, `f64`.
- Boolean: `bool`, values `true` yoki `false`.
- Character: `char`, single quotes bilan, Unicode scalar value.

Default inference amaliy jihatdan muhim: integer literal odatda `i32`, floating literal odatda `f64`. Suffix notation (`5u8`, `5.0f32`) bilan literalning type'i to'g'ridan-to'g'ri value yonida ko'rsatilishi mumkin.

## Syntax and Examples

```rust
let integer = 42; // default i32
let unsigned: u32 = 42;
let float = 2.0; // default f64
let flag: bool = true;
let letter: char = 'z';
```

```rust
let narrow = 5u8;
let precise = 5.0f32;
```

## Common Mistakes

- Integer overflow behaviorini debug va release buildda bir xil deb o'ylash.
- `if` conditionda integer ishlatish; Rust faqat `bool` qabul qiladi.
- `char` uchun double quotes ishlatish.

## Related Concepts

- [[data-types|data types]]
- [[integer-overflow|integer overflow]]
- [[boolean-type|bool]]
- [[if-expressions|if expressions]]
- [[unit-type|unit type]]
- [[never-type|never type (!)]]
- [[type-casting]]

## Sources

- [[3-2-data-types]]
- [[wiki/sources/rust-for-backend-developers-primitive-types]]
