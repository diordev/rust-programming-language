---
title: "14.5. Extending Cargo with Custom Commands"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cargo, custom-commands, tooling]
source_count: 1
---

# 14.5. Extending Cargo with Custom Commands

## Source

- URL: https://doc.rust-lang.org/stable/book/ch14-05-extending-cargo.html
- Kitob: The Rust Programming Language
- Bob: 14.5

## Detailed Summary

Cargo o'zgartirilmasdan yangi subcommandlar bilan kengaytirilishi mumkin.

### Mexanizm

`$PATH`'da `cargo-something` nomli binary mavjud bo'lsa, uni `cargo something` sifatida ishlatish mumkin. Cargo tanlagan subcommand nomini `cargo-` prefiksli binary'ga moslashtiradi.

```shell
$ cargo --list        # barcha mavjud subcommandlar ro'yxati (custom ham)
$ cargo something     # cargo-something ni ishga tushiradi
```

### Amaliyot

```shell
$ cargo install cargo-edit   # cargo-edit → cargo add/rm/upgrade
$ cargo install cargo-expand # cargo-expand → macro'larni kengaytirish
$ cargo install cargo-watch  # cargo-watch → fayl o'zgarganda avtomatik build/test
```

Bu pattern tufayli ekosistem tool'lari Cargo'ning o'ziga integratsiya qilinganday his qildiriladi.

## Key Concepts

- [[cargo|Cargo]] — extensibility pattern
- [[crates-io|crates.io]] — custom tool'lar manbai

## Links To Update

- [[cargo]]
- [[wiki/chapters/14-more-about-cargo-and-crates-io]] chapter
