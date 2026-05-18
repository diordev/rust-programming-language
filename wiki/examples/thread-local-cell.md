---
title: "Thread-local Cell"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, tls, cell, threads]
source_count: 1
---

# Thread-local Cell

Har thread bir xil nomdagi `Cell<u32>`ning o'z nusxasini ko'radi.

```rust
use std::{cell::Cell, thread};

thread_local! {
    static NUM: Cell<u32> = const { Cell::new(0) };
}

fn main() {
    let t1 = thread::spawn(|| NUM.set(1));
    let t2 = thread::spawn(|| NUM.set(2));
    t1.join().unwrap();
    t2.join().unwrap();
}
```

## Related

- [[thread-local-storage]]
- [[cell-t|Cell<T>]]
