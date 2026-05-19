---
title: "serde rename"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, serde, serialization, api-design]
source_count: 1
---

# serde rename

## Short Definition

Rust field yoki variant nomini serialized formatdagi nomdan ajratadigan serde attribute'lari.

## Why It Matters

Rust `snake_case` idiomini saqlab, API'da `camelCase` yoki boshqa naming convention berish kerak bo'ladi.

## Mental Model

Rust ichki nomi code ergonomikasi uchun, serialized nom esa external contract uchun.

## Syntax and Examples

```rust
#[derive(serde::Serialize, serde::Deserialize)]
#[serde(rename_all = "camelCase")]
struct Employee {
    first_name: String,
}
```

## Common Mistakes

- Source'dagi typo kabi `firstName` field nomini xato yozish.
- Rename public contract ekanini hisobga olmasdan keyin o'zgartirish.

## Related Concepts

- [[serialization]]
- [[api-design|API design]]
- [[snake-case|snake_case]]

## Sources

- [[wiki/sources/rust-for-backend-developers-serialization]]
