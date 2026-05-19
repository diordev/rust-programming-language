---
title: "tracing-subscriber"
type: crate
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, crate, logging, tracing]
source_count: 1
---

# tracing-subscriber

## Short Definition

`tracing-subscriber` tracing eventsni filterlash, formatlash, layer qilish va writerga yuborish uchun subscriber implementation beradi.

## Cargo Dependency

```toml
[dependencies]
tracing-subscriber = { version = "0.3", features = ["env-filter", "time", "json"] }
```

## Basic Usage

```rust
tracing_subscriber::fmt()
    .with_env_filter(tracing_subscriber::EnvFilter::from_default_env())
    .init();
```

## Notes

- Source snapshot `0.3`ni ko'rsatadi; latest version deb da'vo qilinmaydi.
- `env-filter` feature `EnvFilter`, `json` feature JSON formatter, `time` feature time formatting uchun kerak.
- `registry()` + layers multi-output logging uchun ishlatiladi.

## Related Pages

- [[subscriber]]
- [[env-filter|EnvFilter]]
- [[tracing-layer|tracing layer]]
- [[tracing-registry|tracing registry]]
- [[wiki/crates/tracing]]

