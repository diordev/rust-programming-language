---
title: "Config Deserialization"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, configuration, serde]
source_count: 1
---

# Config Deserialization

## Short Definition

Raw config valuesni `serde::Deserialize` orqali typed Rust structlarga aylantirish.

## Why It Matters

Stringly-typed config butun app bo'ylab tarqalmasligi kerak; startup’da typed config olinadi.

## Mental Model

Config crate sourcelarni merge qiladi, serde esa final config tree'ni struct shape'ga tekshiradi.

## Syntax and Examples

```rust
#[derive(Debug, serde::Deserialize)]
struct AppConfig {
    server: ServerConfig,
}
```

## Common Mistakes

- `try_deserialize().unwrap()` bilan startup errorni yomon report qilish.
- Deserialize bo'ldi degani semantic validation ham bo'ldi deb o'ylash.

## Related Concepts

- [[deserialization]]
- [[deserialize-trait|Deserialize trait]]
- [[application-configuration|application configuration]]

## Sources

- [[wiki/sources/rust-for-backend-developers-application-configuration]]

