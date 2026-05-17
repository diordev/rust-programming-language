---
title: "worker recv disconnect shutdown"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, channels, worker, shutdown]
source_count: 1
---

# worker recv disconnect shutdown

Worker endi `recv().unwrap()` bilan yashay olmaydi; channel yopilganda loopdan chiqishi kerak.

```rust
let thread = thread::spawn(move || {
    loop {
        let message = receiver.lock().unwrap().recv();

        match message {
            Ok(job) => {
                println!("Worker {id} got a job; executing.");
                job();
            }
            Err(_) => {
                println!("Worker {id} disconnected; shutting down.");
                break;
            }
        }
    }
});
```

## Why It Matters

`Err(_)` panic emas, shutdown signalidir. Worker shu signalni ko'rib o'z loopini tugatadi, keyin `join()` bilan orderly cleanup bo'ladi.

## Related

- [[channels]]
- [[worker-thread]]
- [[graceful-shutdown]]
- [[thread-pool]]
