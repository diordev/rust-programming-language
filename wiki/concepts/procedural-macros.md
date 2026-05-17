---
title: "Procedural Macros"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-16
tags: [rust, macros, proc-macro, code-generation]
source_count: 2
---

# Procedural Macros

## Short Definition

Compile-time functionga o'xshash macro turi: Rust tokenlarini input sifatida oladi va generated Rust tokenlarini output sifatida qaytaradi.

## Why It Matters

Procedural macros Rust syntaxini parse qilib, murakkab code generation qiladi. Custom derive, web framework attributes, SQL DSL checks, serialization derives kabi real-world Rust tooling shu asosda quriladi.

## Mental Model

```rust
TokenStream input -> Rust code transform -> TokenStream output
```

`macro_rules!` pattern/replacement bo'lsa, procedural macro Rust code orqali tokenlarni manipulyatsiya qiladi.

Backend beginner source procedural macro'larni shu declarative macro chapter ichida faqat contrast sifatida tilga oladi: ular odatiy backend app yozishda odatda yozilmaydi, ko'proq framework va tooling authorlari qatlamiga tegishli.

## Syntax and Examples

```rust
use proc_macro::TokenStream;

#[some_attribute]
pub fn some_name(input: TokenStream) -> TokenStream {
    input
}
```

Procedural macro crate:

```toml
[lib]
proc-macro = true
```

Common helper crates:

```toml
[dependencies]
syn = "2.0"
quote = "1.0"
```

## Three Forms

- [[custom-derive-macros|Custom derive macros]] - `#[derive(HelloMacro)]`.
- [[attribute-like-macros|Attribute-like macros]] - `#[route(GET, "/")]`.
- [[function-like-macros|Function-like macros]] - `sql!(SELECT * FROM posts)`.

## Common Mistakes

- Procedural macro main crate ichida yoziladi deb o'ylash; `proc-macro` crate kerak.
- `TokenStream`ni string manipulation bilan qo'lda parse qilish; `syn` yordam beradi.
- `unwrap()` bilan yomon error message berish; production macro'larda aniq diagnostic kerak.
- `quote!` outputini `TokenStream`ga `.into()` bilan convert qilishni unutish.

## Related Concepts

- [[tokenstream|TokenStream]]
- [[custom-derive-macros|custom derive macros]]
- [[attribute-like-macros|attribute-like macros]]
- [[function-like-macros|function-like macros]]
- [[metaprogramming]]
- [[crate]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
