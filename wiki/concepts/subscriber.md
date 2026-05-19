---
title: "Subscriber"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging]
source_count: 1
---

# Subscriber

## Short Definition

Tracing event va spanlarni qabul qilib, filterlash/formatlash/yozishni bajaradigan komponent.

## Why It Matters

`tracing::info!` faqat event yaratadi; event qayerga va qanday yozilishi subscriber orqali hal bo'ladi.

## Mental Model

Facade event yuboradi, global dispatcher eventni registered subscriberga beradi.

## Syntax and Examples

```rust
tracing_subscriber::fmt().init();
```

## Common Mistakes

- `tracing` API borligi bilan log output avtomatik paydo bo'ladi deb o'ylash.
- Bir processda global subscriber faqat bir marta init qilinishini unutish.

## Related Concepts

- [[tracing-registry|tracing registry]]
- [[tracing-layer|tracing layer]]
- [[wiki/crates/tracing-subscriber]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

