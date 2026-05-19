---
title: "WorkerGuard"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging]
source_count: 1
---

# WorkerGuard

## Short Definition

`tracing_appender::non_blocking` qaytaradigan guard; drop paytida queued loglarni flush qilishga yordam beradi.

## Why It Matters

Process tugaganda oxirgi loglar yo'qolmasligi uchun guard `main` scope oxirigacha tirik turishi kerak.

## Mental Model

Guard background logging worker lifecycle owneriga o'xshaydi.

## Syntax and Examples

```rust
let (non_blocking, _guard) = tracing_appender::non_blocking(std::io::stdout());
```

## Common Mistakes

- Guardni `_`ga bind qilib darhol drop qilish.
- Test yoki short-lived CLI’da log flushni tekshirmaslik.

## Related Concepts

- [[non-blocking-logging|non-blocking logging]]
- [[drop]]
- [[wiki/crates/tracing-appender]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

