---
title: "RwLock read and write"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, rwlock, concurrency]
source_count: 1
---

# RwLock read and write

`RwLock<T>` avval ko'p reader, keyin writer modelini beradi.

```rust
use std::sync::RwLock;

fn main() {
    let value = RwLock::new(5);

    let read = value.read().unwrap();
    println!("Read: {}", *read);
    drop(read);

    let mut write = value.write().unwrap();
    *write = 10;
    println!("Updated: {}", *write);
}
```

## Related

- [[rwlock|RwLock]]
- [[mutex-t|Mutex<T>]]
