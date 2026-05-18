---
title: "Barrier batch processing"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, barrier, threads, batching]
source_count: 1
---

# Barrier batch processing

`Barrier` worker'larni har iteratsiya boshida bir nuqtaga yig'adi.

```rust
use std::sync::{Arc, Barrier, Mutex};
use std::thread;

const WORKERS: usize = 4;

fn main() {
    let data = Arc::new(Mutex::new((0..8).collect::<Vec<_>>()));
    let barrier = Arc::new(Barrier::new(WORKERS));

    let mut handles = Vec::new();
    for _ in 0..WORKERS {
        let data = Arc::clone(&data);
        let barrier = Arc::clone(&barrier);
        handles.push(thread::spawn(move || loop {
            barrier.wait();
            let Some(item) = data.lock().unwrap().pop() else { break };
            println!("Processing {item}");
        }));
    }

    for handle in handles {
        handle.join().unwrap();
    }
}
```

## Related

- [[barrier|Barrier]]
- [[threads]]
