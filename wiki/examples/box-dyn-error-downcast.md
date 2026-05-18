---
title: "Box dyn Error downcast"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, error-handling, box-dyn-error, downcast]
source_count: 1
---

# Box dyn Error downcast

Erased error ichidan concrete error `downcast_ref` bilan ajratiladi.

```rust
#[derive(Debug, thiserror::Error)]
#[error("Error A")]
struct ErrA;

fn fail_a() -> Result<(), ErrA> {
    Err(ErrA)
}

fn fail_something() -> Result<(), Box<dyn std::error::Error>> {
    fail_a()?;
    Ok(())
}

fn main() {
    if let Err(e) = fail_something() {
        if let Some(err_a) = e.downcast_ref::<ErrA>() {
            println!("Handle ErrA separately: {err_a}");
        }
    }
}
```

## Related

- [[box-dyn-error|Box<dyn Error>]]
- [[error-downcasting]]
