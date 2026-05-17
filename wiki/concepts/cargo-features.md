---
title: "Cargo Features"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, cargo, dependencies, features]
source_count: 1
---

# Cargo Features

## Short Definition

Cargo features — crate functionality'sining optional qismlarini yoqish yoki o'chirish uchun dependency declaration'da ishlatiladigan named flags.

## Why It Matters

Bitta crate hamma capability'ni default yoqib yubormaydi. Feature'lar build footprint, transitive dependency graph, va available API surface'ni boshqaradi.

## Mental Model

Crate author `Cargo.toml` ichida `[features]` bilan capability setlarini e'lon qiladi. Crate consumer esa dependency yozuvida qaysi feature'lar yoqilishini tanlaydi. `default` feature'lar automatic, qolganlari explicit opt-in.

## Syntax and Examples

```toml
[dependencies]
uuid = { version = "1", features = ["v4"] }
```

Crate ichidagi deklaratsiya:

```toml
[features]
default = ["std"]
v4 = ["rng"]
```

## Common Mistakes

- Nightly `#![feature(...)]` gate'lari bilan Cargo dependency feature'larini bir xil narsa deb o'ylash.
- `version = "1"` yozuvi kerakli API'ni kafolatlaydi, feature kerak emas deb taxmin qilish.
- `default` yoqilgan feature'larni ko'rmasdan crate capability'sini to'liq tushundim deb o'ylash.

## Related Concepts

- [[dependencies]]
- [[cargo-toml|Cargo.toml]]
- [[crates-io|crates.io]]
- [[semver|SemVer]]
- [[feature-flags]]

## Sources

- [[wiki/sources/rust-for-backend-developers-dependencies]]

