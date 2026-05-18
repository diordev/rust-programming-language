---
title: "thiserror reserve error"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, thiserror, error-handling]
source_count: 1
---

# thiserror reserve error

Domain-specific error enum `thiserror` bilan ixcham yoziladi.

```rust
#[derive(Debug, thiserror::Error)]
enum ReserveError {
    #[error("No product with ID {id}")]
    NoSuchProduct { id: u64 },
    #[error("Asked {asked}, but available {available}")]
    NotEnough { asked: u64, available: u64 },
}
```

## Related

- [[custom-error-enum]]
- [[wiki/crates/thiserror]]
