---
title: "Log Levels"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, logging]
source_count: 1
---

# Log Levels

## Short Definition

Log event severity yoki verbosity darajasi: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`.

## Why It Matters

Productionda hamma detailni yozish shovqin va cost beradi; level policy signalni boshqaradi.

## Mental Model

`TRACE` eng batafsil, `ERROR` eng muhim failure signal. Default tracing output odatda `INFO` va undan yuqorisini ko'rsatadi.

## Syntax and Examples

```rust
tracing::debug!("db query started");
tracing::warn!("retrying request");
```

## Common Mistakes

- Expected user errorni `ERROR` deb loglash.
- Debug detailni productionda cheksiz yoqib qo'yish.

## Related Concepts

- [[env-filter|EnvFilter]]
- [[rust-log|RUST_LOG]]
- [[logging]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

