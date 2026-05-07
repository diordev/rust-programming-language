---
title: "Standard Library"
type: concept
status: needs-review
created: 2026-05-06
updated: 2026-05-06
tags: [rust, std]
source_count: 4
---

# Standard Library

## Short Definition

Rust standard library common types, traits, macros, I/O, collections, va platform abstractionsni beradi.

## Why It Matters

Beginner examples `std::io`, `String`, `Result`, va `println!` kabi standard library elementlariga tayanadi.

## Mental Model

Language core minimalroq; kundalik functionalityning katta qismi `std` orqali keladi.

`std` package'ingizga external crate kabi pathda ko'rinadi, lekin Rust bilan birga kelgani uchun `Cargo.toml`ga dependency sifatida yozilmaydi.

Standard library common [[collections|collections]] ham beradi: [[vector|Vec<T>]], [[string-type|String]], va [[hash-map|HashMap]]. `HashMap` `std::collections` ichida turadi, lekin prelude'da emas, shuning uchun odatda explicit `use` kerak.

## Syntax and Examples

```rust
use std::io;
```

```rust
use std::collections::HashMap;
```

## Common Mistakes

- Standard library va external [[crate]]larni aralashtirish.
- Offline documentationni [[rustup]] orqali ochish mumkinligini unutish.
- `std` ishlatish uchun `Cargo.toml`ga dependency qo'shish kerak deb o'ylash.
- `HashMap` standard libraryda bo'lgani uchun avtomatik scope'da bo'ladi deb o'ylash.

## Related Concepts

- [[crate]]
- [[result|Result]]
- [[string-type|String]]
- [[use-declarations|use declarations]]
- [[collections]]
- [[vector|Vec<T>]]
- [[hash-map|HashMap]]

## Sources

- [[1-1-installation-the-rust-programming-language]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
- [[8-common-collections-the-rust-programming-language]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language]]
