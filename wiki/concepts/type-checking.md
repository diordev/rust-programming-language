---
title: "Type Checking"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, compiler, types]
source_count: 2
---

# Type Checking

## Short Definition

Type checking compilerning values, expressions, function calls, va fields expected typelarga mosligini tekshirishi.

## Why It Matters

Rust compile-time type checking orqali domain-specific typesdan foyda chiqaradi: `Color` va `Point` kabi structurally o'xshash values ham alohida type bo'lishi mumkin.

## Mental Model

Type system program shape haqida rules beradi; compiler shu rules buzilsa code'ni rad qiladi.

Type checking runtime checksning bir qismini compile time'ga olib chiqadi. Masalan function `T` qabul qilsa, caller `None` yubora olmaydi; `u32` qabul qilsa, negative value yuborilmaydi. Domain rule yanada maxsus bo'lsa, [[custom-validation-types|custom validation types]] ishlatiladi.

## Syntax and Examples

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);
```

## Common Mistakes

- Field structure bir xil bo'lsa types interchangeable deb o'ylash.
- Compiler type errorsni faqat syntax xato deb qabul qilish.
- Type orqali encode qilish mumkin bo'lgan invariantni har functionda runtime check qilish.

## Related Concepts

- [[data-types|data types]]
- [[structs]]
- [[tuple-structs]]
- [[compiler]]
- [[option|Option]]
- [[invariants]]
- [[custom-validation-types|custom validation types]]

## Sources

- [[wiki/sources/5-using-structs-to-structure-related-data]]
- [[9-3-to-panic-or-not-to-panic]]
