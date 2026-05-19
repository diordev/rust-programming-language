---
title: "Non-blocking Logging"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, logging, concurrency]
source_count: 1
---

# Non-blocking Logging

## Short Definition

Application thread log yozishni bevosita bajarmay, log eventlarni background workerga channel orqali yuborishi.

## Why It Matters

Blocking disk yoki stdout writes request latencyga ta'sir qilishi mumkin.

## Mental Model

App thread senderga yozadi; worker thread channel’dan olib actual writerga yozadi.

## Syntax and Examples

```rust
let (writer, _guard) = tracing_appender::non_blocking(std::io::stdout());
```

## Common Mistakes

- `WorkerGuard`ni darhol drop qilib yuborish.
- Queue overflowda loglar drop bo'lishi mumkinligini unutish.

## Related Concepts

- [[worker-guard|WorkerGuard]]
- [[channels]]
- [[log-writer|log writer]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

