---
title: "Graceful Shutdown"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, concurrency, cleanup, shutdown]
source_count: 1
---

# Graceful Shutdown

## Short Definition

Dastur yoki subsystem yangi ish qabul qilishni to'xtatib, mavjud ishlarni tartibli tugatib, resurslarni xavfsiz bo'shatadigan shutdown usuli.

## Why It Matters

Concurrency'da "tez to'xtatish" va "to'g'ri to'xtatish" boshqa-boshqa narsalar. Threadlarni keskin o'ldirish active request, lock, yoki partial state qoldirishi mumkin. Graceful shutdown ongoing ishlarni yakunlash, worker looplarni to'xtatish, va resurslarni deterministic cleanup qilish imkonini beradi.

## Mental Model

Yangi buyurtma olishni to'xtatasiz, oshxonadagi mavjud buyurtmalarni tayyorlatib tugatasiz, keyin oshpazlarni uyiga jo'natasiz. 21.3 thread pool'da bu: sender'ni yopish, workerlar `recv()`dan `Err` olish, loopdan chiqish, keyin `join()`.

## Syntax and Examples

### Shutdown signal

```rust
drop(self.sender.take());
```

### Worker exit

```rust
match receiver.lock().unwrap().recv() {
    Ok(job) => job(),
    Err(_) => break,
}
```

### Final cleanup

```rust
if let Some(thread) = worker.thread.take() {
    thread.join().unwrap();
}
```

## Common Mistakes

- **Threadlarni shunchaki tashlab yuborish**: active ishlar yarim yo'lda qolishi mumkin.
- **Shutdown signal yubormasdan `join()` qilish**: workerlar abadiy `recv()` kutishi mumkin.
- **Graceful shutdownni faqat log message deb o'ylash**: bu ownership, lifecycle, va blocking behavior masalasi.

## Related Concepts

- [[thread-pool]]
- [[worker-thread]]
- [[drop]]
- [[join-handle]]
- [[channels]]

## Sources

- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
