---
title: "Dev Dependencies"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, cargo, testing, dependencies]
source_count: 1
---

# Dev Dependencies

## Short Definition

`[dev-dependencies]` test, benchmark, va boshqa development-only code uchun kerak bo'ladigan crate'larni alohida e'lon qiladigan Cargo manifest section.

## Why It Matters

Test-only tooling'ni oddiy `[dependencies]`ga qo'shish production dependency graph'ni keraksiz kattalashtiradi va intentni loyqalashtiradi.

## Mental Model

Application yoki library runtime kodi uchun kerak bo'lgan crate'lar `[dependencies]`da turadi. Faqat tests yoki development workflow uchun kerak bo'lgan crate'lar esa `[dev-dependencies]`da turadi.

## Syntax and Examples

```toml
[dev-dependencies]
testcontainers = "0.23"
```

## Common Mistakes

- Test helper crate'ni `[dependencies]`ga yozib yuborish.
- `dev-dependencies` binary/library runtime ichida available bo'ladi deb o'ylash.
- Test infra dependency'larini production API design bilan aralashtirish.

## Related Concepts

- [[testing]]
- [[integration-tests]]
- [[cargo-toml|Cargo.toml]]
- [[dependencies]]

## Sources

- [[wiki/sources/rust-for-backend-developers-testing]]

