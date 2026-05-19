---
title: "EnteredSpan"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging, ownership]
source_count: 1
---

# EnteredSpan

## Short Definition

`Span::entered()` qaytaradigan guard; span ownershipni o'zida saqlab, active span lifecycle’ini boshqaradi.

## Why It Matters

Temporary span yaratib darhol enter qilishda borrowed `Entered<'_>` emas, owned guard kerak bo'lishi mumkin.

## Mental Model

`enter()` span'ga reference tutadi; `entered()` span'ni guard ichiga move qiladi.

## Syntax and Examples

```rust
let entered = tracing::span!(tracing::Level::INFO, "request").entered();
tracing::info!("inside span");
entered.exit();
```

## Common Mistakes

- Guard scope tugamaguncha span active bo'lishini unutish.
- `exit()`dan keyin span context qoladi deb o'ylash.

## Related Concepts

- [[tracing-span|tracing span]]
- [[ownership]]
- [[lifetimes]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

