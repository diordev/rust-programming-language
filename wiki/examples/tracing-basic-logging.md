---
title: "tracing basic logging"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging]
source_count: 1
---

# tracing basic logging

Minimal tracing setup: subscriber init qilinadi, keyin log event yoziladi.

```rust
fn main() {
    tracing_subscriber::fmt().init();

    tracing::info!("Hello");
    tracing::warn!("something needs attention");
}
```

## Related

- [[logging]]
- [[subscriber]]
- [[wiki/crates/tracing]]
- [[wiki/crates/tracing-subscriber]]

