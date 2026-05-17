---
title: "SemVer"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, cargo]
source_count: 2
---

# SemVer

## Short Definition

SemVer, ya'ni Semantic Versioning, crate versionlarini `major.minor.patch` shaklida ifodalaydi.

## Why It Matters

Cargo dependency version constraints orqali compatible crate versionsni tanlaydi. Shu sabab version string resolved exact version bilan bir xil narsa emas.

## Mental Model

Version constraint "qaysi versions qabul qilinadi"ni, lock file esa "qaysi version tanlandi"ni bildiradi. Masalan `uuid = "1"` latest compatible `1.x.y`ni qabul qiladi; exact patch versionni emas.

## Syntax and Examples

```toml
rand = "0.8.5"
```

```toml
uuid = "1"
```

## Common Mistakes

- Constraint exact versionni majburlaydi deb o'ylash.
- Breaking changes major version bilan bog'lanishini unutish.
- Constraint satisfiy bo'lsa Cargo yangiroq compatible patch/minor versiyani tanlashi mumkinligini unutish.

## Related Concepts

- [[dependencies]]
- [[cargo-lock|Cargo.lock]]
- [[crate]]
- [[cargo-toml|Cargo.toml]]

## Sources

- [[wiki/sources/2-programming-a-guessing-game]]
- [[wiki/sources/rust-for-backend-developers-dependencies]]
