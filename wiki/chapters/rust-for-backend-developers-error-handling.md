---
title: "Rust for Backend Developers: Error Handling"
type: chapter
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, backend, error-handling, chapter, advance]
source_count: 1
---

# Rust for Backend Developers: Error Handling

## Learning Goal

Backend code'da domain/library errors, wrapping, erased errors, `anyhow` context, va debugging signal'larini amaliy boundary sifatida ajratish.

## Main Ideas

- Domain/service API uchun concrete error enum ko'pincha to'g'ri default.
- `thiserror` `Display` va `std::error::Error` boilerplate'ini kesadi, error modelning o'zini emas.
- `#[from]` wrapping va `?` propagation'ni birlashtiradi.
- `Box<dyn Error>` typed recovery kerak bo'lmagan app-level boundary'larda foydali.
- `downcast_ref` erased error ichidan selected concrete type'ni ajratib olishga imkon beradi.
- `anyhow` app-level ergonomika beradi: `Result` alias, `context`, `root_cause`, va backtrace.

## Concepts

- [[error-handling]]
- [[custom-error-enum]]
- [[std-error-trait|std::error::Error]]
- [[error-wrapping]]
- [[error-context]]
- [[error-downcasting]]
- [[root-cause]]
- [[box-dyn-error|Box<dyn Error>]]
- [[from-trait]]
- [[question-mark-operator|question mark operator]]

## Examples

```rust
#[derive(Debug, thiserror::Error)]
enum ReserveError {
    #[error("No product with ID {id}")]
    NoSuchProduct { id: u64 },
    #[error("Asked {asked}, but available {available}")]
    NotEnough { asked: u64, available: u64 },
}
```

```rust
use anyhow::Context;

let text = std::fs::read_to_string("non_existing_file.txt")
    .context("Cannot read non_existing_file.txt")?;
```

## Exercises

- `thiserror` bilan ikki service error va bitta wrapping error yozing.
- `Box<dyn Error>` va `anyhow::Error` bilan bir xil failure flow'ni qayta yozing.
- `context()` qaysi qatlamda qo'shilishi kerakligini konkret misolda belgilang.

## Review Questions

- Qachon concrete enum, qachon erased error?
- `#[from]` `?` bilan qanday ishlaydi?
- `context` va `root_cause` nimani alohida beradi?
- `anyhow` nega library public API uchun default emas?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-error-handling]]
- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/crates/thiserror]]
- [[wiki/crates/anyhow]]
