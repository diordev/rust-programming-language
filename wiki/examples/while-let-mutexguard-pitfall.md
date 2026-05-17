---
title: "while let MutexGuard pitfall"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, mutex, while-let, concurrency]
source_count: 1
---

# while let MutexGuard pitfall

Listing 21-21 compile bo'ladigan, lekin worker concurrency'ni sindiradigan nozik variantni ko'rsatadi.

```rust
while let Ok(job) = receiver.lock().unwrap().recv() {
    println!("Worker {id} got a job; executing.");
    job();
}
```

## Why It Matters

Muammo syntax emas, lifetime. `while let` temporary `MutexGuard`ni loop body oxirigacha saqlab qoladi. Shuning uchun `job()` ishlayotgan paytda lock hali qo'yilmagan bo'ladi va boshqa workerlar navbatdan ish ola olmaydi.

To'g'ri yondashuv:

```rust
loop {
    let job = receiver.lock().unwrap().recv().unwrap();
    job();
}
```

## Related

- [[mutexguard-lifetime]]
- [[mutex-t|Mutex<T>]]
- [[while-let]]
- [[worker-thread]]
- [[wiki/chapters/21-2-from-single-threaded-to-multithreaded-server|Chapter 21.2]]
