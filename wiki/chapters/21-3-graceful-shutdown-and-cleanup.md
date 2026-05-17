---
title: "21.3. Graceful Shutdown and Cleanup"
type: chapter
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, thread-pool, cleanup, drop, concurrency]
source_count: 1
---

# 21.3. Graceful Shutdown and Cleanup

## Learning Goal

Ishlayotgan [[thread-pool]]ni to'g'ri tugatish: workerlarga shutdown signali yuborish, ongoing ishlar tugashini kutish, va cleanup paytida ownership'ni `join`, `Option::take()`, va `Vec::drain(..)` bilan boshqarish.

## Main Ideas

- Thread pool faqat job bajarishi yetarli emas; u toza tugashi ham kerak.
- [[drop]] lifecycle hook sifatida `ThreadPool` cleanup'ini boshlash uchun qulay joy.
- `JoinHandle::join(self)` ownership oladi, shu sabab cleanup paytida `JoinHandle`ni struct ichidan to'g'ridan-to'g'ri yulib olish mumkin emas.
- `Vec::drain(..)` va `Option::take()` ownership extraction patternlari bu muammoni hal qiladi.
- `sender`ni drop qilish channelni yopadi; workerlar `recv()` orqali shutdown signalini `Err` sifatida ko'radi.
- Worker loop `Err(_)` ko'rganda `break` qilishi graceful shutdown'ning markaziy nuqtasi.

## Shutdown Sequence

1. Server request qabul qilishni to'xtatadi.
2. `ThreadPool` scope'dan chiqadi.
3. `Drop for ThreadPool` ishga tushadi.
4. `drop(self.sender.take())` channel'ni yopadi.
5. Workerlar `recv()` dan `Err` olib loopdan chiqadi.
6. Main thread har workerning `join()`ini kutadi.

## Ownership Nuances

### `join()` ownership oladi

```rust
worker.thread.join().unwrap();
```

Bu borrowed field ustida ishlamaydi, chunki `join(self)` handle'ni consume qiladi. Qarang: [[e0507-cannot-move-out-of-borrowed-content|E0507]].

### `Option::take()` bilan move

```rust
if let Some(thread) = worker.thread.take() {
    thread.join().unwrap();
}
```

### `Vec::drain(..)` bilan owned workerlar

```rust
for worker in self.workers.drain(..) {
    worker.thread.join().unwrap();
}
```

## Concepts

- [[graceful-shutdown]]
- [[drop]]
- [[join-handle]]
- [[channels]]
- [[option]]
- [[vector|Vec<T>]]
- [[thread-pool]]
- [[worker-thread]]
- [[e0507-cannot-move-out-of-borrowed-content|E0507]]

## Examples

- [[threadpool-drop-and-join|Drop for ThreadPool and join]]
- [[sender-option-take-shutdown|sender Option::take shutdown]]
- [[worker-recv-disconnect-shutdown|worker recv disconnect shutdown]]
- [[incoming-take-two-shutdown-demo|incoming().take(2) demo]]

## Exercises

1. `drop(self.sender.take())` o'rniga `self.sender = None;` yozsangiz semantik farq bormi, tahlil qiling.
2. Worker `recv()` loopini `while let` bilan shutdown-safe yozish mumkinmi, yo'qmi? Sababini yozing.
3. `join()` paytida `unwrap()` panic qilsa cleanup oqimi bilan nima bo'lishini tushuntiring.
4. `Vec::drain(..)` va `Option<JoinHandle>` usullaridan qaysi birini siz tanlardingiz va nega?
5. `take(2)` demosida ikki request tugagach loglar qaysi tartibda chiqishi mumkinligini izohlang.

## Review Questions

1. Graceful shutdown nimani anglatadi?
2. `Drop for ThreadPool` nega kerak?
3. `join()` nega `E0507`ga olib kelishi mumkin?
4. `sender`ni yopish nega workerlarga signal bo'ladi?
5. `recv()` `Err` qaytarganda worker nima qilishi kerak?
6. `Option::take()` va `Vec::drain(..)` qanday ownership muammolarini hal qiladi?
7. `listener.incoming().take(2)` demo nima uchun qo'shilgan?

## Related Pages

- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|Source: 21.3]]
- [[wiki/chapters/21-2-from-single-threaded-to-multithreaded-server|Chapter 21.2]]
- [[thread-pool]]
- [[graceful-shutdown]]
