---
title: "OnceLock explicit init"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, oncelock, globals]
source_count: 1
---

# OnceLock explicit init

`OnceLock` explicit `set()` bilan bir marta initialize qilinadi.

```rust
use std::sync::{Mutex, OnceLock};

static O: OnceLock<Mutex<String>> = OnceLock::new();

fn main() {
    let _ = O.set(Mutex::new("1".to_string()));
    let second = O.set(Mutex::new("2".to_string()));
    assert!(second.is_err());

    let value = O.get().unwrap().lock().unwrap();
    println!("{value}");
}
```

## Related

- [[oncelock|OnceLock]]
- [[global-data]]
