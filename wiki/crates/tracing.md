---
title: "tracing"
type: crate
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, crate, logging, tracing]
source_count: 1
---

# tracing

## Short Definition

`tracing` structured, contextual logging va spans uchun API/facade crate.

## Cargo Dependency

```toml
[dependencies]
tracing = "0.1"
```

## Basic Usage

```rust
tracing::info!("request accepted");
```

## Notes

- Source snapshot `tracing = "0.1"`ni ko'rsatadi; latest version deb da'vo qilinmaydi.
- Event yaratadi; eventni qayerga yozish subscriber implementation orqali hal qilinadi.
- `span!` va `#[instrument]` context fieldlarni log eventlarga bog'laydi.

## Related Pages

- [[logging]]
- [[structured-logging|structured logging]]
- [[tracing-span|tracing span]]
- [[instrument-attribute|#[instrument]]]
- [[wiki/sources/rust-for-backend-developers-logging]]

