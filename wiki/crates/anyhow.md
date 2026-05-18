---
title: "anyhow"
type: crate
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, crate, error-handling]
source_count: 1
---

# anyhow

## Short Definition

`anyhow` application-level error handlingni soddalashtiradigan crate: `anyhow::Error`, `anyhow::Result<T>`, `context`, `root_cause`, va backtrace ergonomikasini beradi.

## Cargo Dependency

```toml
[dependencies]
anyhow = "1"
```

## Basic Usage

```rust
fn call_failable() -> anyhow::Result<()> {
    fail_with_specific_error()?;
    Ok(())
}
```

```rust
use anyhow::Context;

let text = std::fs::read_to_string("file.txt")
    .context("Cannot read file.txt")?;
```

## Notes

- App-level boundary uchun yaxshi; public library/domain API uchun default emas.
- `downcast_ref`, `root_cause`, va `backtrace` debugging signalini boyitadi.
- `thiserror` bilan raqib emas; ikkalasi turli qatlam uchun.

## Related Pages

- [[error-context]]
- [[root-cause]]
- [[box-dyn-error|Box<dyn Error>]]
- [[wiki/sources/rust-for-backend-developers-error-handling]]
