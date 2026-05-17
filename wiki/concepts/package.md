---
title: "Package"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, cargo, crates]
source_count: 3
---

# Package

## Short Definition

Package Cargo boshqaradigan bundle bo'lib, bir yoki bir nechta [[crate|crates]] orqali bitta functionality set beradi.

## Why It Matters

Rust project layoutini tushunishda package va crate farqi muhim. `Cargo.toml` package darajasidagi manifest; crate esa compiler build qiladigan code birligi.

## Mental Model

- Package: Cargo project containeri.
- Crate: package ichidagi compile unit.
- Package kamida bitta crate tutadi.
- Package ko'p [[binary-crate|binary crates]] tutishi mumkin.
- Package eng ko'pi bilan bitta [[library-crate|library crate]] tutadi.
- `Cargo.toml` package darajasidagi manifest.
- `src/bin/*.rs` package ichida qo'shimcha binary crate'lar yaratadi.

## Syntax and Examples

```text
my-project/
  Cargo.toml
  src/
    main.rs
```

Bu `my-project` package bitta binary crate tutadi.

```text
my-project/
  Cargo.toml
  src/
    main.rs
    lib.rs
```

Bu package bitta binary crate va bitta library crate tutadi.

## Common Mistakes

- Package va crate'ni bir xil narsa deb o'ylash.
- `Cargo.toml` crate ichida emas, package root'ida turishini unutish.
- Package bir nechta library crate tutishi mumkin deb o'ylash.

## Related Concepts

- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[crate]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[src-bin|src/bin]]
- [[crate-root|crate root]]
- [[dependencies]]

## Sources

- [[wiki/sources/7-packages-crates-and-modules]]
- [[7-1-packages-and-crates]]
- [[wiki/sources/rust-for-backend-developers-package-crate-module]]
