---
title: "tracing EnvFilter with RUST_LOG"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging, env-filter]
source_count: 1
---

# tracing EnvFilter with RUST_LOG

Runtime log filtering `RUST_LOG` orqali boshqariladi.

```rust
fn main() {
    tracing_subscriber::fmt()
        .with_env_filter(tracing_subscriber::EnvFilter::from_default_env())
        .init();

    tracing::debug!("debug detail");
    tracing::info!("normal info");
}
```

```shell
$ RUST_LOG="debug" cargo run
```

## Related

- [[env-filter|EnvFilter]]
- [[rust-log|RUST_LOG]]
- [[log-levels|log levels]]

