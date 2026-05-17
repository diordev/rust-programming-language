---
title: "Rust for Backend Developers: Primitive Types"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, types, chapter]
source_count: 1
---

# Rust for Backend Developers: Primitive Types

## Learning Goal

Rustdagi basic scalar types, `()`, `!`, va explicit `as` castingni bir reference qatlam sifatida tushunish.

## Main Ideas

- Integer literal default'i `i32`, float literal default'i `f64`.
- `bool` va `char` oddiy ko'rinsa ham aniq type semanticsga ega.
- `()` unit type, `!` never type.
- Rust implicit numeric conversion qilmaydi; cast ochiq yoziladi.

## Concepts

- [[scalar-types|scalar types]]
- [[unit-type|unit type]]
- [[never-type|never type (!)]]
- [[type-casting]]
- [[type-inference|type inference]]

## Examples

```rust
let a = 5u8;
let b = 5.0f32;
let c: () = ();
```

```rust
let code = 'A' as i32;
let bit = true as i32;
```

## Exercises

- 6 ta literal yozib, ularning inferred yoki explicit type'ini ajrating.
- `as` bilan data loss bo'lishi mumkin bo'lgan 3 conversion yozing.

## Review Questions

- Nima uchun Rust implicit conversionlardan qochadi?
- `unit type` va `never type`ni qanday ajratish kerak?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-primitive-types]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[scalar-types|scalar types]]
- [[type-casting]]
