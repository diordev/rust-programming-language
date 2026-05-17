---
title: "Rust for Backend Developers: Declarative Macros"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, macros, chapter]
source_count: 1
---

# Rust for Backend Developers: Declarative Macros

## Learning Goal

`macro_rules!` macro'larning compile-time expansion, fragment specifier, repetition, va `vec![]` bilan bog'liqligini tushunish.

## Main Ideas

- Declarative macro pattern/replacement modeli bilan ishlaydi.
- Fragment specifierlar argument type-kategoriyalarini aniq belgilaydi.
- `$(...),*` repetition variadic-like macro'larni yozishga imkon beradi.
- `vec![]` macro bo'lishining sababi - functions variadic emas.

## Concepts

- [[macro]]
- [[declarative-macros|declarative macros]]
- [[procedural-macros|procedural macros]]
- [[vec-macro|vec! macro]]
- [[identifiers]]

## Examples

```rust
macro_rules! sum_nums {
    ( $x:expr, $y:expr ) => { $x + $y }
}
```

## Exercises

- `ident` va `literal` fragmentlari bilan ikki kichik macro yozing.
- `sum_nums!`ni repetition bilan qayta yozing.

## Review Questions

- `expr` va `stmt` fragmentlari nima uchun turli?
- `vec![]`ni oddiy function bilan almashtirib bo'lmasligining sababi nima?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[declarative-macros|declarative macros]]
- [[vec-macro|vec! macro]]
