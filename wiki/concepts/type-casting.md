---
title: "Type Casting"
type: concept
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, types, casting]
source_count: 1
---

# Type Casting

## Short Definition

Type casting Rustda bir typedagi qiymatni boshqa typega explicit o'tkazish bo'lib, odatda `as` operatori bilan yoziladi.

## Why It Matters

Rust implicit numeric conversion qilmaydi. Demak type o'zgarishi sodir bo'lsa, u kodda ko'rinishi kerak. Bu accidental conversion va yashirin ma'no yo'qotishlarni kamaytiradi.

## Mental Model

`as` "compiler, men bu conversionni ataylab qilyapman" degan signal. Bu convenience beradi, lekin har doim lossless degani emas.

`as` ayniqsa primitive typelar orasida ko'p uchraydi: integer widening/narrowing, float -> integer, `bool` -> integer, `char` -> integer.

## Syntax and Examples

```rust
let a: i32 = 5;
let b: i64 = a as i64;
```

```rust
let d: f32 = 7.0;
let e: i32 = d as i32;
let g: i32 = true as i32; // 1
let code = 'A' as i32;    // 65
```

## Common Mistakes

- Rust C kabi implicit conversion qiladi deb o'ylash.
- `as` bilan qilingan har conversion ma'no yo'qotmaydi deb o'ylash.
- Fallible conversion kerak joyda ham `as` ishlatib, truncation yoki sign-change xavfini yashirish.

## Related Concepts

- [[scalar-types|scalar types]]
- [[type-annotations|type annotations]]
- [[type-inference|type inference]]
- [[unit-type|unit type]]
- [[rust-operators-and-symbols]]

## Sources

- [[wiki/sources/rust-for-backend-developers-primitive-types]]
