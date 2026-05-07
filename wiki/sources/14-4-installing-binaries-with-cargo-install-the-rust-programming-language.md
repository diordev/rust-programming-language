---
title: "14.4. Installing Binaries with cargo install"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cargo, cargo-install, tooling]
source_count: 1
---

# 14.4. Installing Binaries with cargo install

## Source

- URL: https://doc.rust-lang.org/stable/book/ch14-04-installing-binaries.html
- Kitob: The Rust Programming Language
- Bob: 14.4

## Detailed Summary

`cargo install` — crates.io'dagi binary crate'larni lokal o'rnatish buyrug'i. Tizim paket menejerini almashtirmaydi; Rust dasturchilari uchun qulay tool o'rnatish usuli.

### Faqat binary target

Faqat **binary target** bo'lgan crate'lar o'rnatilishi mumkin — ya'ni `src/main.rs` yoki binary sifatida belgilangan fayli bor crate'lar. Library-only crate'larni `cargo install` qilib bo'lmaydi.

### O'rnatish joyi

```shell
~/.cargo/bin/    ← barcha o'rnatilgan binary'lar
```

`$PATH` ga `~/.cargo/bin` qo'shilmagan bo'lsa, o'rnatilgan dasturlar ishlamayi.

### Misol: ripgrep

```shell
$ cargo install ripgrep
    Updating crates.io index
  Downloaded ripgrep v14.1.1
  Installing ripgrep v14.1.1
  ...
  Installing ~/.cargo/bin/rg
   Installed package `ripgrep v14.1.1` (executable `rg`)

$ rg --help   # ishlatish
```

## Key Concepts

- [[cargo|Cargo]] — `cargo install` subcommand
- [[binary-crate|binary crate]] — install qilish uchun binary target zarur
- [[crates-io|crates.io]] — tool'lar manbai

## Questions Raised

- `cargo install` bilan o'rnatilgan versionni yangilash qanday (`cargo install --force`)?
- `cargo uninstall` ishlaydi, lekin qayerda hujjatlashtirilgan?

## Links To Update

- [[cargo]]
- [[14-more-about-cargo-and-crates-io]] chapter
