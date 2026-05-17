---
title: "Rust for Backend Developers: Functions"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, functions, chapter]
source_count: 1
---

# Rust for Backend Developers: Functions

## Learning Goal

Typed function signatures, final expression return, explicit `return`, inner functions, `const fn`, va `static mut` xavfini bir joyda tushunish.

## Main Ideas

- Parameter types majburiy.
- Final expression default return.
- `return` early exit uchun.
- Inner helper functionlar mumkin.
- `return` va `!` bog'langan.
- `static mut` faqat `unsafe` ichida.

## Concepts

- [[functions]]
- [[never-type|never type (!)]]
- [[statements-and-expressions|statements and expressions]]
- [[static-items]]
- [[unsafe-rust|unsafe Rust]]

## Examples

```rust
fn safe_divide(a: f32, b: f32) -> f32 {
    if b == 0.0 {
        return 0.0;
    }
    a / b
}
```

## Exercises

- Early return ishlatadigan va ishlatmaydigan ikki function yozing.
- `const fn` bilan bitta constant hisoblang.

## Review Questions

- `return` nega `!` bilan bog'lanadi?
- `static mut` nega odatda yomon tanlov?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-functions]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[functions]]
- [[never-type|never type (!)]]
