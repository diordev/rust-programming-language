---
title: "serde"
type: crate
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, crate, serialization]
source_count: 1
---

# serde

## Short Definition

`serde` Rust typelarini serialize/deserialize qilish uchun traitlar va derive macros beradigan crate.

## Cargo Dependency

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
```

## Basic Usage

```rust
#[derive(serde::Serialize, serde::Deserialize)]
struct Employee {
    id: u64,
    name: String,
}
```

`serde` JSON yoki XML formatni o'zi yozmaydi; format crate'lari `Serialize` va `Deserialize` implementationlardan foydalanadi.

## Notes

- Source snapshot `serde = "1"`ni ko'rsatadi; latest version deb da'vo qilinmaydi.
- `derive` feature odatda DTO va config typelar uchun kerak.
- Borrowed deserialization kerak bo'lmasa generic helpers ko'pincha `DeserializeOwned` talab qiladi.

## Related Pages

- [[serialization]]
- [[deserialization]]
- [[serialize-trait|Serialize trait]]
- [[deserialize-trait|Deserialize trait]]
- [[deserializeowned|DeserializeOwned]]
- [[wiki/sources/rust-for-backend-developers-serialization]]

