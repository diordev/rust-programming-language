---
title: "Tracing Registry"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging]
source_count: 1
---

# Tracing Registry

## Short Definition

`tracing_subscriber::registry()` layersni birlashtiradigan base subscriber.

## Why It Matters

Multi-layer logging setup uchun registry root sifatida ishlatiladi.

## Mental Model

`fmt()` shortcut bitta layerli subscriber; `registry().with(...)` explicit layer composition.

## Syntax and Examples

```rust
use tracing_subscriber::{layer::SubscriberExt, util::SubscriberInitExt};

tracing_subscriber::registry()
    .with(tracing_subscriber::fmt::layer())
    .init();
```

## Common Mistakes

- `registry()`ni formatter deb o'ylash; formatter layer alohida qo'shiladi.
- `init()` uchun `SubscriberInitExt` kerakligini unutish.

## Related Concepts

- [[tracing-layer|tracing layer]]
- [[subscriber]]
- [[wiki/crates/tracing-subscriber]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

