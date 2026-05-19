---
title: "tracing-appender"
type: crate
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, crate, logging, tracing]
source_count: 1
---

# tracing-appender

## Short Definition

`tracing-appender` tracing logsni rolling filelarga yoki non-blocking writer orqali yozish uchun crate.

## Cargo Dependency

```toml
[dependencies]
tracing-appender = "0.2"
```

## Basic Usage

```rust
let appender = tracing_appender::rolling::daily("logs", "app.log");
```

```rust
let (non_blocking, _guard) = tracing_appender::non_blocking(std::io::stdout());
```

## Notes

- Source snapshot `0.2`ni ko'rsatadi; latest version deb da'vo qilinmaydi.
- `WorkerGuard` process tugashidan oldin queued loglarni flush qilish uchun scope'da tirik turishi kerak.
- Non-blocking queue to'lsa loglar drop bo'lishi mumkin.

## Related Pages

- [[log-writer|log writer]]
- [[non-blocking-logging|non-blocking logging]]
- [[worker-guard|WorkerGuard]]
- [[wiki/crates/tracing]]

