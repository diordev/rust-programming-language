---
title: "Structured Logging"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, logging, observability]
source_count: 1
---

# Structured Logging

## Short Definition

Log message bilan birga machine-readable fields yozish.

## Why It Matters

Production log aggregation systems fieldlar bo'yicha search, filter, va dashboard qilishni talab qiladi.

## Mental Model

Plain text "user created" desa, structured log `user_id=42 action=create` kabi query qilinadigan data beradi.

## Syntax and Examples

```rust
tracing::info!(user_id = 42, action = "create", "user event");
```

## Common Mistakes

- Faqat message stringga tayanish.
- Password, token, yoki PII fieldlarini logga chiqarish.

## Related Concepts

- [[logging]]
- [[tracing-span|tracing span]]
- [[instrument-attribute|#[instrument]]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

