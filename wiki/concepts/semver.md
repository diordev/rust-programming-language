---
title: "SemVer"
type: concept
status: needs-review
created: 2026-05-06
updated: 2026-05-06
tags: [rust, cargo]
source_count: 1
---

# SemVer

## Short Definition

SemVer, ya'ni Semantic Versioning, package versionsni `major.minor.patch` shaklida ifodalaydi.

## Why It Matters

Cargo dependency version constraints orqali compatible crate versionsni tanlaydi.

## Mental Model

Version constraint "qaysi versions qabul qilinadi"ni, lock file esa "qaysi version tanlandi"ni bildiradi.

## Syntax and Examples

```toml
rand = "0.8.5"
```

## Common Mistakes

- Constraint exact versionni majburlaydi deb o'ylash.
- Breaking changes major version bilan bog'lanishini unutish.

## Related Concepts

- [[dependencies]]
- [[cargo-lock|Cargo.lock]]
- [[crate]]

## Sources

- [[2-programming-a-guessing-game-the-rust-programming-language]]
