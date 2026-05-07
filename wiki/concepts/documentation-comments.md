---
title: "Documentation Comments"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, comments, documentation, doc-tests, cargo-doc]
source_count: 2
---

# Documentation Comments

## Short Definition

Rust API dokumentatsiyasi uchun maxsus comment shakllari. `///` (item dokumentatsiyasi) va `//!` (container dokumentatsiyasi) — ikki tur. `cargo doc` bilan HTML ga aylanadi; `# Examples` bloki `cargo test` bilan avtomatik test sifatida ishga tushadi.

## Why It Matters

Doc comments faqat insonlar uchun emas — `cargo test` ular ichidagi kod misollarini sinaydi. Shuning uchun dokumentatsiya va kod har doim sinxron bo'lishi kafolatlanadi.

## Mental Model

`///` — keyingi item haqida (funksiya, struct, method).
`//!` — o'z ichiga olgan narsa haqida (crate, module) — odatda `src/lib.rs` boshida.

## Syntax and Examples

```rust
// src/lib.rs
//! # My Crate
//!
//! `my_crate` is a collection of utilities to make performing
//! certain calculations more convenient.

/// Adds one to the number given.
///
/// # Examples
///
/// ```
/// let arg = 5;
/// let answer = my_crate::add_one(arg);
/// assert_eq!(6, answer);
/// ```
///
/// # Panics
///
/// This function never panics.
pub fn add_one(x: i32) -> i32 {
    x + 1
}
```

**Standart bo'limlar:**

| Bo'lim | Maqsad |
|--------|--------|
| `# Examples` | Ishlatish namunalari (doc-test) |
| `# Panics` | Qachon panic bo'lishi mumkin |
| `# Errors` | `Result` qaytarsa, xato turlari |
| `# Safety` | `unsafe` funksiya uchun invariantlar |

**Doc-tests — `cargo test` bilan:**
```shell
Doc-tests my_crate
running 1 test
test src/lib.rs - add_one (line 5) ... ok
```

**HTML generatsiya:**
```shell
$ cargo doc --open    # browser'da ochiladi
```

## Common Mistakes

- `//!` o'rniga `///` ishlatib crate umumiy tavsifini yozishga urinish
- `# Examples` blokida ishlamaydigan kod qoldirish — `cargo test` uni ushlaydi
- Private item'larni `///` bilan dokumentatsiya qilish — `cargo doc` public API'ni ko'rsatadi

## Related Concepts

- [[comments]] — oddiy `//` comments
- [[readable-code|readable code]]
- [[public-api|public API]] — doc comments asosan public item'lar uchun
- [[testing]] — doc-tests `cargo test` bilan

## Sources

- [[3-4-comments-the-rust-programming-language]]
- [[14-2-publishing-a-crate-to-crates-io-the-rust-programming-language|14.2 Publishing a Crate]]
