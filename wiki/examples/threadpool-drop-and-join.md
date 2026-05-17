---
title: "ThreadPool drop and join"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, drop, join, thread-pool]
source_count: 1
---

# ThreadPool drop and join

21.3 cleanup'ning birinchi maqsadi: pool scope'dan chiqayotganda har worker thread tugashini kutish.

### `Vec::drain(..)` bilan ownership olish

```rust
impl Drop for ThreadPool {
    fn drop(&mut self) {
        for worker in self.workers.drain(..) {
            println!("Shutting down worker {}", worker.id);
            worker.thread.join().unwrap();
        }
    }
}
```

### Final code'dagi `Option::take()`

```rust
impl Drop for ThreadPool {
    fn drop(&mut self) {
        drop(self.sender.take());

        for worker in &mut self.workers {
            println!("Shutting down worker {}", worker.id);

            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}
```

## Why It Matters

`join()` owned `JoinHandle` oladi. Shu sabab cleanup paytida handle'ni struct ichidan ownership bilan chiqarish kerak.

## Related

- [[drop]]
- [[join-handle]]
- [[e0507-cannot-move-out-of-borrowed-content|E0507]]
- [[graceful-shutdown]]
