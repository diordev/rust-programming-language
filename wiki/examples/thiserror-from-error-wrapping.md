---
title: "thiserror from error wrapping"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, thiserror, from, error-wrapping]
source_count: 1
---

# thiserror from error wrapping

`#[from]` lower-level error'ni yuqori qatlam error'iga conversion qiladi.

```rust
#[derive(Debug, thiserror::Error)]
enum PurchaseError {
    #[error("Nested reservation error: {0}")]
    ReservationFailed(#[from] ReserveError),
    #[error("Nested shipping error: {0}")]
    ShippingFailed(#[from] ShipmentError),
}
```

## Related

- [[error-wrapping]]
- [[from-trait]]
- [[wiki/crates/thiserror]]
