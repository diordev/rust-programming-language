---
title: "LazyLock mutex hashmap"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, lazylock, mutex, hashmap, globals]
source_count: 1
---

# LazyLock mutex hashmap

`HashMap::new()` non-const bo'lgani uchun global `static`ga `LazyLock` bilan olib kiriladi.

```rust
use std::{
    collections::HashMap,
    sync::{LazyLock, Mutex},
};

static M: LazyLock<Mutex<HashMap<String, i32>>> =
    LazyLock::new(|| Mutex::new(HashMap::new()));

fn main() {
    M.lock().unwrap().insert("one".to_string(), 1);
    println!("{:?}", *M.lock().unwrap());
}
```

## Related

- [[lazylock|LazyLock]]
- [[global-data]]
