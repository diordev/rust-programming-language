---
title: "Raw Identifiers"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-16
tags: [rust, syntax, identifiers, editions]
source_count: 2
---

# Raw Identifiers

## Short Definition

`r#name` syntax'i keyword bo'lgan so'zni Rust'da [[identifiers]] sifatida ishlatishga ruxsat beradigan escape hatch.

## Why It Matters

Keyword bilan nom conflict bo'lsa yoki boshqa [[edition]]da yozilgan API sizning edition'ingizda keyword bo'lib qolgan bo'lsa, raw identifier compile bo'ladigan va idiomatic yechim beradi.

## Mental Model

Parser odatda `match`, `try`, `crate` kabi so'zlarni grammar signali deb ko'radi. `r#` prefiksi parserga "buni keyword ma'nosida emas, identifier sifatida o'qi" deydi.

## Syntax and Examples

### Keyword'ni function nomi qilish

```rust
fn r#match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}

fn main() {
    assert!(r#match("foo", "foobar"));
}
```

### Cross-edition call

```rust
let result = legacy_crate::r#try();
```

Bu pattern 2015 edition'da yozilgan `try` API'ni 2018+ edition crate'dan chaqirish uchun kerak bo'lishi mumkin.

Backend beginner source bu syntaxning yana oddiyroq use case'ini beradi:

```rust
let r#if = 5;
```

Bu raw identifier faqat functions yoki external APIlar uchun emasligini ko'rsatadi; oddiy local bindingda ham ishlaydi.

## Common Mistakes

- `r#`ni faqat definitionda yozib, use site'da unutish.
- Raw identifier faqat future reserved so'zlar uchun kerak deb o'ylash.
- Edition compatibility muammosini oddiy naming muammosi deb ko'rish.

## Related Concepts

- [[keywords]]
- [[identifiers]]
- [[edition]]
- [[rust-2024-edition|Rust 2024 Edition]]
- [[functions]]
- [[match]]
- [[crate]]

## Sources

- [[wiki/sources/22-1-a-keywords|22.1]]
- [[wiki/sources/rust-for-backend-developers-variables]]
