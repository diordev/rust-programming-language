---
title: "Condvar flag wait"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, condvar, mutex, threads]
source_count: 1
---

# Condvar flag wait

Bir thread flag'ni `true` qilguncha boshqa thread `Condvar`da kutadi.

```rust
use std::sync::{Arc, Condvar, Mutex};
use std::thread;

fn main() {
    let pair = Arc::new((Mutex::new(false), Condvar::new()));

    {
        let pair = Arc::clone(&pair);
        thread::spawn(move || {
            let (lock, cvar) = &*pair;
            let mut ready = lock.lock().unwrap();
            *ready = true;
            cvar.notify_one();
        });
    }

    let (lock, cvar) = &*pair;
    let mut ready = lock.lock().unwrap();
    while !*ready {
        ready = cvar.wait(ready).unwrap();
    }
}
```

## Related

- [[condvar|Condvar]]
- [[mutex-t|Mutex<T>]]
