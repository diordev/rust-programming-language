---
title: "Log Writer"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, logging, io]
source_count: 1
---

# Log Writer

## Short Definition

Log eventlar yoziladigan sink: stdout, file, rolling appender, yoki custom writer.

## Why It Matters

Console dev uchun qulay, productionda file, stdout collector, yoki structured pipeline kerak bo'lishi mumkin.

## Mental Model

Subscriber eventni formatlaydi, writer esa bytes sifatida qayergadir yozadi.

## Syntax and Examples

```rust
tracing_subscriber::fmt()
    .with_writer(std::io::stdout)
    .init();
```

## Common Mistakes

- File output uchun ANSI color codesni o'chirmaslik.
- Blocking writerning latency impactini hisobga olmaslik.

## Related Concepts

- [[write-trait|Write]]
- [[non-blocking-logging|non-blocking logging]]
- [[wiki/crates/tracing-appender]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

