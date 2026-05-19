---
title: "Tracing Layer"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging]
source_count: 1
---

# Tracing Layer

## Short Definition

Tracing subscriber ichida eventlarni filterlash, formatlash yoki yozish uchun ulanadigan composable qatlam.

## Why It Matters

Console va file output, turli filters, yoki turli formats bitta monolithic subscriber o'rniga layers bilan ajratiladi.

## Mental Model

Registry root, layers esa unga ketma-ket ulanadigan behavior qatlamlari.

## Syntax and Examples

```rust
tracing_subscriber::registry()
    .with(stdout_layer)
    .with(file_layer)
    .init();
```

## Common Mistakes

- Layerlar uchun `SubscriberExt` traitini scope'ga olib kirmaslik.
- Bitta output uchun oddiy `fmt()` yetarli bo'lgan joyda ortiqcha layer complexity qo'shish.

## Related Concepts

- [[tracing-registry|tracing registry]]
- [[subscriber]]
- [[log-writer|log writer]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

