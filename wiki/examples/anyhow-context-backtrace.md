---
title: "anyhow context and backtrace"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, anyhow, error-context, backtrace]
source_count: 1
---

# anyhow context and backtrace

`anyhow` original error'ga context va debugging signal qo'shadi.

```rust
use anyhow::Context;

fn read_non_existing_file() -> anyhow::Result<String> {
    let text = std::fs::read_to_string("non_existing_file.txt")
        .context("Cannot read non_existing_file.txt")?;
    Ok(text)
}
```

Backtrace uchun source `RUST_LIB_BACKTRACE=1` environment variable'ni ko'rsatadi.

## Related

- [[error-context]]
- [[backtrace]]
- [[wiki/crates/anyhow]]
