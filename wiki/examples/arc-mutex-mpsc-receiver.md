---
title: "Arc<Mutex<mpsc::Receiver<Job>>>"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, arc, mutex, channels, concurrency]
source_count: 1
---

# Arc<Mutex<mpsc::Receiver<Job>>>

Listing 21-18 std `mpsc` receiver'ni bir nechta worker thread orasida xavfsiz ulashish uchun `Arc<Mutex<_>>` ishlatadi.

```rust
let (sender, receiver) = mpsc::channel();
let receiver = Arc::new(Mutex::new(receiver));

for id in 0..size {
    workers.push(Worker::new(id, Arc::clone(&receiver)));
}
```

## Why It Matters

`Receiver<Job>`ni loop ichida ownership bilan berish `E0382` beradi va std `mpsc` single-consumer bo'lgani uchun clone ham qilinmaydi. `Arc` shared ownership, `Mutex` esa serialized access beradi.

## Related

- [[arc-t|Arc<T>]]
- [[mutex-t|Mutex<T>]]
- [[channels]]
- [[job-queue]]
- [[wiki/chapters/21-2-from-single-threaded-to-multithreaded-server|Chapter 21.2]]
