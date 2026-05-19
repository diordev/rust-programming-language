---
title: "tracing file appender layers"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, logging, file]
source_count: 1
---

# tracing file appender layers

Stdout va file output alohida layer sifatida ulanadi.

```rust
use tracing_subscriber::{fmt::layer, layer::SubscriberExt, util::SubscriberInitExt};

fn main() {
    let file_layer = layer()
        .with_writer(tracing_appender::rolling::daily("logs", "app.log"))
        .with_ansi(false);

    let stdout_layer = layer().with_writer(std::io::stdout);

    tracing_subscriber::registry()
        .with(stdout_layer)
        .with(file_layer)
        .init();

    tracing::info!("Hello");
}
```

## Related

- [[tracing-layer|tracing layer]]
- [[tracing-registry|tracing registry]]
- [[log-writer|log writer]]
- [[wiki/crates/tracing-appender]]

