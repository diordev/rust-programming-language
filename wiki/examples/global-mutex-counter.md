---
title: "Global mutex counter"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, globals, mutex, threads]
source_count: 1
---

# Global mutex counter

`static` global `Mutex<u64>` scoped threadlar bilan safe increment qilinadi.

```rust
use std::{sync::Mutex, thread, time::Duration};

static COUNTER: Mutex<u64> = Mutex::new(0);

fn main() {
    thread::scope(|s| {
        s.spawn(|| {
            for _ in 0..100 {
                let mut guard = COUNTER.lock().unwrap();
                let curr = *guard;
                thread::sleep(Duration::from_millis(10));
                *guard = curr + 1;
            }
        });
        s.spawn(|| {
            for _ in 0..100 {
                let mut guard = COUNTER.lock().unwrap();
                let curr = *guard;
                thread::sleep(Duration::from_millis(10));
                *guard = curr + 1;
            }
        });
    });

    println!("{}", *COUNTER.lock().unwrap()); // 200
}
```

## Related

- [[global-data]]
- [[mutex-t|Mutex<T>]]
