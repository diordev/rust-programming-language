---
title: "Library Crate"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, crates, cargo]
source_count: 3
---

# Library Crate

## Short Definition

Library crate reusable functionality define qiladigan [[crate]] bo'lib, `main` function'ga ega emas va executable chiqarmaydi.

## Why It Matters

Rust ecosystem reusable library crates atrofida qurilgan. Boshqa projectlar library crate'ni [[dependencies|dependency]] sifatida ishlatadi; masalan Chapter 2'dagi [[rand]] random number generation beradi.

## Mental Model

- Library crate: boshqa crate'lar chaqiradigan code.
- `src/lib.rs`: default library crate root.
- Package eng ko'pi bilan bitta library crate tutadi.
- Rustaceanlar ko'pincha "crate" deganda library crate'ni nazarda tutadi.
- Binary + library package patternida module tree odatda `src/lib.rs`da define qilinadi, reusable functionality shu library crate public API'siga chiqadi.
- Local reusable code boshqa package'ga `path` dependency sifatida ulanib ishlatilishi mumkin.

## Syntax and Examples

```text
my-project/
  Cargo.toml
  src/
    lib.rs
```

`src/lib.rs` mavjud bo'lsa, Cargo package bilan bir xil nomdagi library crate bor deb biladi.

## Common Mistakes

- Library crate executable beradi deb kutish.
- Library crate uchun `main` function kerak deb o'ylash.
- Bitta package bir nechta library crate tutadi deb taxmin qilish.
- Reusable logicni binary crate ichida qoldirib, boshqa projectlar ishlata olmaydigan qilib qo'yish.

## Related Concepts

- [[crate]]
- [[binary-crate|binary crate]]
- [[package]]
- [[crate-root|crate root]]
- [[dependencies]]
- [[rand]]
- [[cargo|Cargo]]
- [[public-api|public API]]
- [[api-design|API design]]

## Sources

- [[7-1-packages-and-crates]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree]]
- [[wiki/sources/rust-for-backend-developers-creating-library]]
