---
title: "crates.io"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, crates, tooling, publishing, cargo-publish, yank]
source_count: 3
---

# crates.io

## Short Definition

Rust ekotizimining rasmiy public crate registry'si. Ochiq manbali crate'larni ulashish, o'rnatish va versiyalash uchun markaziy platforma. Crates.io barqaror arxiv — publish qilingan versiyalar o'chirilmaydi.

## Common Commands

```shell
$ cargo add crate_name             # loyihaga dependency qo'shish
$ cargo login                      # API token bilan autentifikatsiya
$ cargo publish                    # crate'ni crates.io'ga yuklash
$ cargo publish -p crate_name      # workspace'da aniq crate
$ cargo yank --vers 1.0.1          # versiyani yangi dependency sifatida bloklash
$ cargo yank --vers 1.0.1 --undo   # yankni bekor qilish
```

## Crate ishlatish (dependency sifatida)

```toml
# Cargo.toml
[dependencies]
rand = "0.8.5"
```

`cargo build` da Cargo crates.io'dan versiyani yuklab, `Cargo.lock`'ga yozadi.

## Publish workflow

### 1. Hisob va token

- crates.io'da GitHub account bilan kirish
- `cargo login <token>` — token `~/.cargo/credentials.toml`'da saqlanadi (maxfiy!)

### 2. Majburiy metadata (`Cargo.toml`)

```toml
[package]
name = "my_crate"           # noyob nom — first-come, first-served
version = "0.1.0"
edition = "2024"
description = "A short description."
license = "MIT OR Apache-2.0"   # SPDX identifikatori
```

`description` yoki `license` yo'q bo'lsa `cargo publish` xato beradi.

### 3. Publish

```shell
$ cargo publish
```

**Mangu**: versiyani o'chirish mumkin emas. Crates.io barqaror arxiv — boshqa loyihalardagi `Cargo.lock`'lar ishlashda davom etishi uchun.

### 4. Yangi versiya

`Cargo.toml`'da `version` ni [[semver|SemVer]] qoidasi bo'yicha oshiring → `cargo publish`.

### 5. Yank

```shell
$ cargo yank --vers 1.0.1
```

- Yangi loyihalar bu versiyani dependency sifatida qo'sha olmaydi
- Mavjud `Cargo.lock`'lar ta'sirlanmaydi — eski loyihalar ishlayveradi
- Kod o'chirilmaydi (tasodifan yuklangan sirlar uchun alohida amal kerak)

## Notes

- External crates odatda crates.io'dan olinadi
- `cargo add crate_name` → `Cargo.toml`'ga qo'shib, versiyani topadi
- `Cargo.lock` aniq versiyalarni saqlaydi — reproducible builds uchun
- Crate sahifasi odatda available feature'lar va upstream repository'ga olib boradi; dependency capability'ni tekshirishda bu foydali start point.

## Related Pages

- [[crate]]
- [[dependencies]]
- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[cargo-features]]
- [[semver|SemVer]]
- [[pub-use|pub use]] — qulay public API
- [[documentation-comments|doc comments]]
- [[grapheme-clusters|grapheme clusters]]

## Sources

- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[14-2-publishing-a-crate-to-crates-io|14.2 Publishing a Crate]]
- [[wiki/sources/rust-for-backend-developers-dependencies]]
