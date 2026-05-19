---
title: "EnvFilter"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging, filter]
source_count: 1
---

# EnvFilter

## Short Definition

`tracing-subscriber` filter modeli bo'lib, target/module/level bo'yicha loglarni tanlaydi.

## Why It Matters

Dependency loglari shovqin bo'lishi mumkin; module-level filter production va debugging uchun zarur.

## Mental Model

Filter directive `target::module=level` shaklida yoziladi va comma bilan bir nechta directive birlashtiriladi.

## Syntax and Examples

```rust
tracing_subscriber::fmt()
    .with_env_filter(tracing_subscriber::EnvFilter::from_default_env())
    .init();
```

```shell
$ RUST_LOG="myapp::db=debug,info" cargo run
```

## Common Mistakes

- `DEBUG`ni global yoqib, dependency loglari bilan outputni to'ldirish.
- `EnvFilter` uchun `env-filter` feature kerakligini unutish.

## Related Concepts

- [[rust-log|RUST_LOG]]
- [[log-levels|log levels]]
- [[log-target|log target]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

