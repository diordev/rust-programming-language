---
title: "22.5. E - Editions"
type: chapter
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, editions, compatibility]
source_count: 1
---

# 22.5. E - Editions

## Learning Goal

Rust edition tizimi nimani boshqarishini, compiler versiondan qanday farq qilishini, va cross-edition compatibility hamda migration workflow'ini tushunish.

## Main Ideas

- Editions incremental language changes'ni tushunarli package'ga yig'adi.
- `edition` key `Cargo.toml`da source code qaysi rules bilan parse qilinishini belgilaydi.
- Edition changes opt-in; compiler yangilansa ham code avtomatik sinib ketmasligi kerak.
- Turli editiondagi crates bir-biri bilan link bo'la oladi.
- Edition migration cargo-fix-assisted workflow bilan qo'llab-quvvatlanadi.

## Concepts

- [[edition]]
- [[rust-editions]]
- [[rust-2024-edition|Rust 2024 Edition]]
- [[edition-migration]]
- [[cargo-fix-edition-migration]]

## Examples

- [[cargo-fix-unused-mut-example|cargo fix unused mut example]]

## Exercises

1. Edition va compiler version farqini jadval qilib yozing.
2. Cross-edition linking nega ecosystem stability uchun muhim?
3. Yangi keyword qo'shilganda edition boundary nega kerak bo'lishini tushuntiring.

## Review Questions

1. Edition nima?
2. Default edition qaysi?
3. Opt-in compatibility nimani anglatadi?
4. Rust 2015 va Rust 2024 crates birga ishlay oladimi?
5. Migration uchun qaysi tool tilga olinadi?

## Related Pages

- [[wiki/sources/22-5-e-editions|Source: 22.5]]
- [[wiki/chapters/22-appendix|Chapter 22]]
- [[edition]]
- [[rust-editions]]
