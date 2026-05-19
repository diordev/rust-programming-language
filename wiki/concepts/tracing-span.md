---
title: "Tracing Span"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging]
source_count: 1
---

# Tracing Span

## Short Definition

`tracing`da code region yoki logical operation contextini ifodalovchi scope.

## Why It Matters

Request id, user id, operation name kabi context har bir log eventga qo'lda qayta yozilmasdan ulanadi.

## Mental Model

Active span ichida yozilgan log event span fieldlarini ham ko'taradi.

## Syntax and Examples

```rust
let span = tracing::span!(tracing::Level::INFO, "request", request_id = 42);
let _entered = span.enter();
tracing::info!("handling request");
```

## Common Mistakes

- Temporary span ustida `enter()` qilib, borrowed guard yaratishga urinish.
- Async `.await` boundary ichida entered span guardni noto'g'ri ushlab turish.

## Related Concepts

- [[entered-span|EnteredSpan]]
- [[instrument-attribute|#[instrument]]]
- [[thread-local-storage|thread local storage]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

