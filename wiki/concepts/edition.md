---
title: "Edition"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-13
tags: [rust, edition]
source_count: 2
---

# Edition

## Short Definition

`edition` Rust projectining language compatibility settingi bo'lib, source code qaysi edition parsing va semantics boundary ichida o'qilishini bildiradi.

## Why It Matters

Edition yangi language changesni projectlarga boshqarilgan tarzda kiritadi va ecosystemni sindirmasdan evolyutsiya qilishga yordam beradi.

## Mental Model

Compiler version toolchainni bildiradi; edition esa source code qaysi Rust language edition qoidalari bilan o'qilishini bildiradi. Bir xil compiler turli editiondagi crate'larni compile qila oladi.

## Syntax and Examples

```toml
[package]
edition = "2024"
```

`Cargo.toml`da edition ko'rsatilmasa, historical default `2015` deb olinadi. Modern projectlarda editionni explicit yozish kerak.

Turli editiondagi crates bir-biri bilan bog'lana oladi; edition change ko'proq parser va language surface darajasida compatibility boundary hisoblanadi.

## Common Mistakes

- Editionni dependency version yoki compiler version bilan aralashtirish.
- Edition upgrade bo'lsa barcha dependencylar ham shu editionga o'tishi shart deb o'ylash.

## Related Concepts

- [[rust-2024-edition|Rust 2024 Edition]]
- [[cargo-toml|Cargo.toml]]
- [[rust-editions]]
- [[edition-migration]]
- [[cargo-fix-edition-migration]]

## Sources

- [[0-the-rust-programming-language]]
- [[wiki/sources/22-5-e-editions|22.5. E - Editions]]
