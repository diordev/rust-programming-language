---
title: "Custom Error Enum"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, error-handling, api-design]
source_count: 1
---

# Custom Error Enum

## Short Definition

Domain yoki service failure variantlarini explicit enum bilan ifodalash patterni.

## Why It Matters

String message'dan farqli, caller structured variant va payload bilan branch qila oladi.

## Mental Model

Error ham domain data. U ham shape va semanticsga ega.

## Syntax and Examples

```rust
#[derive(Debug, thiserror::Error)]
enum ReserveError {
    #[error("No product with ID {id}")]
    NoSuchProduct { id: u64 },
    #[error("Asked {asked}, but available {available}")]
    NotEnough { asked: u64, available: u64 },
}
```

## Common Mistakes

- Hamma variantni bitta stringga tushirish.
- Public API uchun error semanticasini yashirish.

## Related Concepts

- [[std-error-trait|std::error::Error]]
- [[error-wrapping]]
- [[wiki/crates/thiserror]]

## Sources

- [[wiki/sources/rust-for-backend-developers-error-handling]]
