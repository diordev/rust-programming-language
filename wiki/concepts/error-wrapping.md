---
title: "Error Wrapping"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, error-handling, propagation]
source_count: 1
---

# Error Wrapping

## Short Definition

Pastki qatlam error'ini yuqori qatlam API error'iga o'rab qaytarish patterni.

## Why It Matters

Current API semantics saqlanadi, shu bilan birga original sabab ham yo'qolmaydi.

## Mental Model

Error chain'da root cause pastda qoladi, lekin tashqi boundary o'z nomi bilan gapiradi.

## Syntax and Examples

```rust
#[derive(Debug, thiserror::Error)]
enum PurchaseError {
    #[error("Nested reservation error: {0}")]
    ReservationFailed(#[from] ReserveError),
    #[error("Nested shipping error: {0}")]
    ShippingFailed(#[from] ShipmentError),
}
```

## Common Mistakes

- Keraksiz joyda har qatlamda yangi wrapper yasash.
- `#[from]` va `?` conversion chain'ini tushunmaslik.

## Related Concepts

- [[from-trait]]
- [[question-mark-operator|question mark operator]]
- [[custom-error-enum]]

## Sources

- [[wiki/sources/rust-for-backend-developers-error-handling]]
