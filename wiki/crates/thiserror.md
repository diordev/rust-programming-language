---
title: "thiserror"
type: crate
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, crate, error-handling]
source_count: 1
---

# thiserror

## Short Definition

`thiserror` custom error type'lar uchun `Display` va `std::error::Error` impl'larini derive bilan soddalashtiradigan crate.

## Cargo Dependency

```toml
[dependencies]
thiserror = "1"
```

## Basic Usage

```rust
#[derive(Debug, thiserror::Error)]
enum ReserveError {
    #[error("No product with ID {id}")]
    NoSuchProduct { id: u64 },
}
```

`#[from]` bilan wrapping conversion ham generatsiya qilinadi.

## Notes

- Source misolidagi `thiserror = "1"` source snapshot sifatida saqlanadi; latest deb da'vo qilinmaydi.
- Library/domain API uchun concrete error enum yozishda ayniqsa foydali.
- `thiserror` error modelni o'zi emas, boilerplate'ni kamaytiradi.

## Related Pages

- [[custom-error-enum]]
- [[error-wrapping]]
- [[std-error-trait|std::error::Error]]
- [[wiki/sources/rust-for-backend-developers-error-handling]]
