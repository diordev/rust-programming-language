---
title: "Atomic counter with Relaxed ordering"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, atomics, ordering, threads]
source_count: 1
---

# Atomic counter with Relaxed ordering

Counter faqat bitta atomik variable ustida yig'ilsa `Relaxed` ko'p hollarda yetadi.

```rust
use std::sync::atomic::{AtomicI32, Ordering};
use std::thread;

fn main() {
    let counter = AtomicI32::new(0);

    thread::scope(|scope| {
        for _ in 0..1000 {
            scope.spawn(|| {
                for _ in 0..1000 {
                    counter.fetch_add(1, Ordering::Relaxed);
                }
            });
        }
    });

    println!("{}", counter.load(Ordering::Relaxed)); // 1_000_000
}
```

## Related

- [[atomic-types]]
- [[atomic-memory-ordering]]
