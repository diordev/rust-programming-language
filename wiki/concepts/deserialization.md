---
title: "Deserialization"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, serialization, serde]
source_count: 1
---

# Deserialization

## Short Definition

Tashqi formatdagi data'ni typed Rust value'ga aylantirish.

## Why It Matters

Request body, config file, message queue payload, va persisted data Rust type systemiga qaytarilishi kerak.

## Mental Model

Deserialization - wire/storage formatdan Rust type'ga kirish. Bu jarayonda schema mismatch error bo'lishi mumkin.

## Syntax and Examples

```rust
let employee: Employee = serde_json::from_str(json_text)?;
```

## Common Mistakes

- Har qanday JSONni darhol `Value`ga parse qilish.
- `unwrap()` bilan invalid inputni panicga aylantirish.

## Related Concepts

- [[serialization]]
- [[deserialize-trait|Deserialize trait]]
- [[deserializeowned|DeserializeOwned]]
- [[wiki/crates/serde]]

## Sources

- [[wiki/sources/rust-for-backend-developers-serialization]]

