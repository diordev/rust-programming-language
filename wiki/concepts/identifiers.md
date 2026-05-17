---
title: "Identifiers"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-16
tags: [rust, syntax, naming]
source_count: 2
---

# Identifiers

## Short Definition

Rust'dagi nomlar: function, variable, parameter, struct field, module, crate, constant, macro, static value, attribute, type, trait, yoki lifetime'ni belgilaydigan syntax birliklari.

## Why It Matters

Naming qoidalari parser, scope, va API design bilan bevosita bog'liq. [[keywords]] odatda identifier bo'la olmaydi; kerak bo'lsa [[raw-identifiers]] ishlatiladi.

## Mental Model

Identifier — code ichidagi "yorliq". Parser qaysi joyda qanday turdagi nom kutilayotganini biladi: function nomi, module nomi, type nomi, yoki lifetime nomi. Har joyda istalgan so'zni ishlatib bo'lmaydi, chunki grammar reserved keywordlarni alohida ma'noda o'qiydi.

Declarative macro'larda `ident` fragment aynan identifier capture qilish uchun ishlatiladi:

```rust
macro_rules! make_empty_func {
    ($func_name:ident) => {
        fn $func_name() {}
    }
}
```

## Syntax and Examples

Function va variable identifierlari:

```rust
fn contains_needle(haystack: &str, needle: &str) -> bool {
    let result = haystack.contains(needle);
    result
}
```

Module, type, field, va lifetime identifierlari:

```rust
mod parser {
    struct Config {
        timeout_ms: u64,
    }

    fn parse<'a>(input: &'a str) -> &'a str {
        input
    }
}
```

## Common Mistakes

- Keywordni oddiy identifier sifatida ishlatishga urinish.
- Lifetime nomlari ham identifier familyasiga kirishini unutish.
- Identifier va string literal bir xil narsa deb o'ylash.

## Related Concepts

- [[keywords]]
- [[raw-identifiers]]
- [[functions]]
- [[module]]
- [[crate]]
- [[traits]]
- [[static-items]]
- [[static-lifetime]]

## Sources

- [[wiki/sources/22-1-a-keywords|22.1]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
