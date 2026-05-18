---
title: "Poisoned mutex recovery"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, mutex, poison, threads]
source_count: 1
---

# Poisoned mutex recovery

Thread panic qilgandan keyin `PoisonError`dan `MutexGuard`ni olib, state'ni tekshirish va poison flag'ni tozalash.

```rust
use std::{sync::{Arc, Mutex}, thread};

fn main() {
    let m = Arc::new(Mutex::new(5));

    {
        let m = Arc::clone(&m);
        let t = thread::spawn(move || {
            let _guard = m.lock().unwrap();
            panic!("poisoning mutex");
        });
        let _ = t.join();
    }

    if let Err(err) = m.lock() {
        let guard = err.into_inner();
        println!("Value after panic: {}", *guard);
        drop(guard);
        m.clear_poison();
    }
}
```

## Related

- [[poisoned-mutex]]
- [[mutex-t|Mutex<T>]]
