---
title: "serde enum tagging"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, serde, enum, serialization]
source_count: 1
---

# serde enum tagging

## Short Definition

Serde enum variant nomi va payloadini serialized formatda qanday ifodalash strategiyasi.

## Why It Matters

Rust enum JSON kabi native enum yo'q formatga chiqqanda variant identity yo'qolmasligi kerak.

## Mental Model

Tag - serialized data ichida "bu qaysi variant" degan signal. Payload - shu variant data'si.

## Syntax and Examples

```rust
#[derive(serde::Serialize, serde::Deserialize)]
#[serde(tag = "type", content = "obj")]
enum Event {
    Created { id: u64 },
    Deleted { id: u64 },
}
```

## Common Mistakes

- `#[serde(untagged)]`ni ambiguity xavfini hisobga olmasdan ishlatish.
- Internally tagged strategy tuple variants bilan ishlamasligini unutish.

## Related Concepts

- [[enums]]
- [[serialization]]
- [[deserialization]]
- [[wiki/crates/serde]]

## Sources

- [[wiki/sources/rust-for-backend-developers-serialization]]

