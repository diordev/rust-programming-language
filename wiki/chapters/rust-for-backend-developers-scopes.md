---
title: "Rust for Backend Developers: Scopes"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, scope, chapter]
source_count: 1
---

# Rust for Backend Developers: Scopes

## Learning Goal

Scope variable lifetime va block expression value'sini bir modelga yig'ib tushunish.

## Main Ideas

- Variable scope tugaganda yo'q qilinadi.
- Har bir block expression value qaytaradi.
- Oxirgi expressiondan keyingi `;` block natijasini `()`ga aylantiradi.

## Concepts

- [[scope]]
- [[statements-and-expressions|statements and expressions]]
- [[unit-type|unit type]]
- [[drop]]

## Examples

```rust
let a = {
    let x = 1;
    x + 1
};
```

## Exercises

- `;` bilan va `;`siz ikki block yozib, typesni solishtiring.
- Inner scope variable'sini tashqarida ishlatib compiler xatosini kuzating.

## Review Questions

- Scope value qaytarishi Rust syntaxining qaysi boshqa qismlariga asos bo'ladi?
- Nima uchun `x + y;` bilan block natijasi o'zgaradi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-scopes]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[scope]]
- [[statements-and-expressions|statements and expressions]]
