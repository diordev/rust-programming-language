---
title: "17.6. Futures, Tasks, and Threads"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency, threads]
source_count: 1
---

# 17.6. Futures, Tasks, and Threads

## Learning Goal

Thread, task, va future'larning uch darajali concurrency modelini tushunish. Qachon thread, qachon async ishlatish kerakligini bilish va ikkalasini birgalikda ishlatish.

## Main Ideas

### Uch darajali model

```
Future  ← eng mayda birlik; bir future boshqa future'lar daraxti
  ↑ boshqaradi runtime executor
Task    ← async operatsiyalar to'plami; future'lar orasida almashinadi
  ↑ boshqaradi OS
Thread  ← sinxron operatsiyalar to'plami; faqat thread'lar orasida concurrency
```

Task — thread'ga o'xshash, lekin OS emas runtime boshqaradi. Shuning uchun task ancha yengil.

### Qachon thread, qachon async?

```
CPU-bound ish (hisoblash, parallel data):
    → Thread — haqiqiy parallelism kerak
    → Har bir thread mustaqil CPU kuchidan foydalanadi

I/O-bound ish (tarmoq, fayllar, event queue):
    → Async — bitta thread'da yuzlab/minglab concurrent I/O
    → CPU kutib turadi, thread sarflash befoyda
```

### Thread + Async birgalikda

```rust
let (tx, mut rx) = trpl::channel();

// Thread: CPU-bound yoki blocking ish
thread::spawn(move || {
    for i in 1..11 {
        tx.send(i).unwrap();
        thread::sleep(Duration::from_secs(1));
    }
});

// Async: non-blocking consume
trpl::block_on(async {
    while let Some(message) = rx.recv().await {
        println!("{message}");
    }
});
```

Real dunyo: video encoding thread'da (CPU-bound) + UI yangilanishi async channel orqali (event-driven).

### Work stealing

Ko'pchilik runtime'lar (tokio) ichida task'lar thread'lar orasida avtomatik ko'chiriladi (work stealing). Bu uchun ham thread, ham task kerak.

`spawn_task`, `spawn_blocking` funksiyalari aslida multithreaded runtime ichida ishlaydi — foydalanuvchidan yashiringan.

### 17-bob xulosasi

Rust concurrencysi uchun uch asbob:
1. **Thread** — OS darajasi, CPU-bound parallelism
2. **Task** — runtime darajasi, async concurrent I/O
3. **Future** — tilning asosi, zero-cost abstraction

Uchala yondashuv birgalikda ishlaydi. Rust ikkalasini xavfsiz qo'llash uchun ownership + type system kafolatlarini beradi.

## Concepts

- [[threads]] — CPU-bound parallelism, OS thread
- [[async-task|async task]] — runtime-managed concurrent unit
- [[future|Future trait]] — eng mayda concurrency birligi
- [[cpu-bound|CPU-bound]] → thread afzal
- [[io-bound|I/O-bound]] → async afzal
- [[concurrency]] — uch darajali model

## Examples

- [[thread-async-combined]] — thread produce + async consume

## Exercises

1. I/O-bound va CPU-bound qismlarni ajratib, birini thread, ikkinchisini async bilan yozing.
2. `spawn_blocking` ni qachon ishlatish kerak? Tushuntiring.

## Review Questions

1. Future, task, thread — uchala darajani farqlang.
2. CPU-bound ish uchun nima uchun async yetarli emas?
3. Work stealing nima va nima uchun ham thread, ham task kerak?
4. Thread va async birgalikda qanday ishlaydi (Listing 17-25 misoli)?

## Related Pages

- [[wiki/chapters/16-fearless-concurrency|16. Fearless Concurrency]] — thread-based concurrency
- [[17-async-programming|17. Async Programming Intro]]
- [[wiki/sources/17-6-futures-tasks-and-threads|Source: 17.6]]
