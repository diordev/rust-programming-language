---
title: "Rust for Backend Developers: If"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, if, chapter]
source_count: 1
---

# Rust for Backend Developers: If

## Learning Goal

`if` Rustda braces talab qiladigan va value qaytara oladigan expression ekanini tushunish.

## Main Ideas

- Har branch braces bilan yoziladi.
- `if` resultini variable'ga assign qilish mumkin.
- Semicolon branch resultini `()`ga aylantirishi mumkin.

## Concepts

- [[if-expressions|if expressions]]
- [[statements-and-expressions|statements and expressions]]
- [[scope]]
- [[unit-type|unit type]]

## Examples

```rust
let abs_value = if a < 0 { -a } else { a };
```

## Exercises

- `if`ni expression sifatida ishlatadigan 2 ta misol yozing.
- `;` qo'yilgan branch bilan `()` chiqishini tekshiring.

## Review Questions

- Nega Rust `if`ni statement emas, expression sifatida ko'rish foydali?
- Branch type'lari nega mos bo'lishi kerak?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-if]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[if-expressions|if expressions]]
