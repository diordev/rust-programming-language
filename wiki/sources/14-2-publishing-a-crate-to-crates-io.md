---
title: "14.2. Publishing a Crate to Crates.io"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cargo, crates-io, documentation, pub-use, publishing, doc-tests]
source_count: 1
---

# 14.2. Publishing a Crate to Crates.io

## Source

- URL: https://doc.rust-lang.org/stable/book/ch14-02-publishing-to-crates-io.html
- Kitob: The Rust Programming Language
- Bob: 14.2

## Detailed Summary

### Documentation comments (`///` va `//!`)

**`///`** — keyingi public item'ni dokumentatsiya qiladi. Markdown qo'llab-quvvatlanadi. `cargo doc` HTML generatsiya qiladi.

```rust
/// Adds one to the number given.
///
/// # Examples
///
/// ```
/// let arg = 5;
/// let answer = my_crate::add_one(arg);
/// assert_eq!(6, answer);
/// ```
pub fn add_one(x: i32) -> i32 {
    x + 1
}
```

**`//!`** — o'z ichiga olgan item'ni dokumentatsiya qiladi (crate yoki module darajasi). Odatda `src/lib.rs` boshida:

```rust
//! # My Crate
//!
//! `my_crate` is a collection of utilities to make performing
//! certain calculations more convenient.
```

**Standart bo'limlar:**
- `# Examples` — ishlatish namunalari
- `# Panics` — qachon panic bo'lishi mumkin
- `# Errors` — `Result` qaytarsa, xato turlari
- `# Safety` — `unsafe` funksiya bo'lsa, invariantlar

**Doc-tests:** `# Examples` blokidagi kod `cargo test` bilan avtomatik test sifatida ishga tushadi. Kod va dokumentatsiya sinxronligini ta'minlaydi.

```shell
Doc-tests my_crate
running 1 test
test src/lib.rs - add_one (line 5) ... ok
```

### `pub use` — qulay public API

Ichki modul tuzilmasi foydalanuvchilar uchun noqulay bo'lishi mumkin:

```rust
// Noqulay: use art::kinds::PrimaryColor;
// Noqulay: use art::utils::mix;
```

`pub use` bilan top levelda re-export:

```rust
// src/lib.rs
pub use self::kinds::PrimaryColor;
pub use self::kinds::SecondaryColor;
pub use self::utils::mix;
```

Endi:
```rust
// Qulay: use art::PrimaryColor;
// Qulay: use art::mix;
```

`cargo doc` bosh sahifada re-exportlarni ham ko'rsatadi.

### crates.io'ga publish qilish

**1. Hisob yaratish:**
- crates.io'da GitHub account bilan kirish
- API token olish → `cargo login <token>`
- Token `~/.cargo/credentials.toml`'da saqlanadi — maxfiy!

**2. Majburiy metadata (`Cargo.toml`):**

```toml
[package]
name = "guessing_game"        # crates.io'da noyob nom (first-come, first-served)
version = "0.1.0"
edition = "2024"
description = "A fun game where you guess what number the computer has chosen."
license = "MIT OR Apache-2.0" # SPDX identifikatori
```

`description` va `license` bo'lmasa `cargo publish` xato beradi.

**3. Publish:**
```shell
$ cargo publish
```

**Muhim:** Publish **mangu**. Versiyani o'chirish yoki qayta yozish mumkin emas — crates.io barqaror arxiv sifatida ishlaydi.

**4. Yangi versiya:**
`Cargo.toml`'da `version` ni SemVer bo'yicha oshiring → `cargo publish`.

**5. Yank (versiyani bekor qilish):**
```shell
$ cargo yank --vers 1.0.1        # yangi loyihalar bu versiyani ishlatmasin
$ cargo yank --vers 1.0.1 --undo # bekor qilishni qaytarish
```
Yank mavjud `Cargo.lock`'larni buzmayin — faqat yangi dependency sifatida qo'shilmaydi.

## Key Concepts

- [[documentation-comments|documentation comments]] (`///`, `//!`, doc-tests)
- [[pub-use|pub use]] — ichki tuzilma va public API ajratish
- [[crates-io|crates.io]] — publish workflow
- [[semver|SemVer]] — versiyalash qoidasi

## Code Examples

- Listing 14-1: `///` doc comment va `# Examples` bo'limi
- Listing 14-2: `//!` crate darajasi dokumentatsiya
- Listing 14-3..6: `art` crate — ichki tuzilma vs `pub use` bilan toza API

## Exercises or Practice Ideas

- `my_crate` uchun to'liq doc comment yozing (`# Examples`, `# Panics`, `# Errors`)
- `cargo test` bilan doc-test ishlashini tekshiring
- `pub use` bilan ichki modul'ni top levelda re-export qiling

## Questions Raised

- `license-file` qachon `license` o'rniga ishlatiladi?
- `cargo yank` qilingan versiyani kim ko'rishi mumkin?

## Links To Update

- [[documentation-comments]]
- [[pub-use]]
- [[crates-io]]
- [[wiki/chapters/14-more-about-cargo-and-crates-io]] chapter
