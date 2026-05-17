---
title: "Rust Editions"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, editions, compatibility]
source_count: 1
---

# Rust Editions

## Short Definition

Rust editions — til o'zgarishlarini tushunarli package'larga yig'uvchi compatibility boundary'lar.

## Why It Matters

Rust har olti haftada release bo'ladi. Editionlar shu incremental changes'ni ma'lum davrlarda birlashtirib, ecosystem barqarorligini saqlagan holda tilni evolyutsiya qilishga yordam beradi.

## Mental Model

Stable release cadence va edition cadence bir xil emas:

- stable release'lar tez-tez keladi;
- edition esa bir necha yilda bir marta keladigan "language package".

## Syntax and Examples

```toml
[package]
edition = "2024"
```

## Common Mistakes

- Editionni compiler version bilan bir xil deb o'ylash.
- Yangi featurelar faqat yangi compiler bilan keladi, edition irrelevant deb o'ylash.

## Related Concepts

- [[edition]]
- [[rust-2024-edition|Rust 2024 Edition]]
- [[edition-migration]]

## Sources

- [[wiki/sources/22-5-e-editions|22.5]]
