---
title: "Edition Migration"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, editions, migration]
source_count: 1
---

# Edition Migration

## Short Definition

Edition migration — projectning `Cargo.toml`dagi edition settingini keyingi editionga o'tkazish va shu boundary bilan bog'liq parsing incompatibilitylarni tozalash jarayoni.

## Why It Matters

Editions opt-in bo'lgani uchun project eski editionda qolishi mumkin, lekin yangi idiomlar va keyword-related featurelardan foydalanish uchun migration kerak bo'ladi.

## Mental Model

Compiler yangilanishi bilan code avtomatik yangi editionga o'tmaydi. Migration alohida qaror va workflow.

## Syntax and Examples

```toml
[package]
edition = "2024"
```

## Common Mistakes

- Compiler upgrade bo'ldi, demak edition ham upgrade bo'ldi deb o'ylash.
- Cross-edition dependency ishlagani uchun migration hech qachon kerak bo'lmaydi deb o'ylash.

## Related Concepts

- [[edition]]
- [[rust-editions]]
- [[cargo-fix-edition-migration]]
- [[cargo-fix]]

## Sources

- [[wiki/sources/22-5-e-editions|22.5]]
