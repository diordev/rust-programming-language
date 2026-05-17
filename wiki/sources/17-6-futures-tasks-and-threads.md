---
title: "17.6. Futures, Tasks, and Threads"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency, threads]
source_count: 1
---

# 17.6. Futures, Tasks, and Threads

## Source

- URL: https://doc.rust-lang.org/stable/book/ch17-06-futures-tasks-threads.html
- Raw: `raw/books/the_rust_programming_language/17.6. Futures, Tasks, and Threads.md`

## Detailed Summary

Bu yakunlovchi bo'lim 17-bobda o'rganilgan barcha narsalarni va 16-bobdagi thread'larni birlashtirib, Rust concurrency modelining to'liq manzarasini chizadi.

### Uch darajali concurrency modeli

Rust concurrencysi uch darajadan iborat:

```
Future  — eng mayda birlik; await nuqtalarida boshqaruv beradi
  ↑ boshqaradi
Task    — async operatsiyalar to'plami; future'lar orasida almashinadi
  ↑ boshqaradi
Thread  — OS tomonidan boshqariluvchi sinxron operatsiyalar to'plami
```

- **Future** — eng mayda concurrency birligi; bir future boshqa future'lar daraxtini ifodalashi mumkin
- **Task** — thread'ga o'xshash, lekin OS emas runtime boshqaradi; bir task ichida bir nechta future'lar almashinadi
- **Thread** — OS darajasida; faqat thread'lar orasida concurrency, ichida emas

### Thread vs Async qachon?

| Holat | Yechim |
|---|---|
| CPU-bound (ma'lumot qayta ishlash, har qism mustaqil) | **Thread** |
| I/O-bound (turli manbalardan xabarlar, noma'lum vaqtda) | **Async** |
| Ikkalasi ham kerak | Thread + Async birgalikda |

### Birgalikda ishlatish: Thread produce + Async consume

```rust
let (tx, mut rx) = trpl::channel();

// Thread: sinxron produce (CPU yoki blocking I/O)
thread::spawn(move || {
    for i in 1..11 {
        tx.send(i).unwrap();
        thread::sleep(Duration::from_secs(1));
    }
});

// Async: concurrent consume
trpl::block_on(async {
    while let Some(message) = rx.recv().await {
        println!("{message}");
    }
});
```

Real misol: video encoding thread (CPU-bound) → UI'ga async channel orqali xabar beradi.

### Work stealing

Ko'pchilik async runtime'lar (tokio kabi) ichida **work stealing** mexanizmi ishlaydi: task'lar zaruriyatga qarab thread'lar orasida ko'chiriladi. Bu runtime'ga ham thread'lar, ham task'lar kerak degani.

`spawn_blocking`, `spawn_task` funksiyalari aslida multithreaded runtime'da ishlaydi — bu foydalanuvchidan yashiringan.

### Xulosa

- Thread va async raqib emas — **bir-birini to'ldiruvchi** vositalar.
- Rust ikkalasini xavfsiz qo'llash uchun kerakli barcha vositalarni beradi.
- Keyingi: Chapter 21 veb-server loyihasida bu tushunchalar amalda qo'llaniladi.

## Key Concepts

- [[threads]] — OS-darajasidagi sinxron concurrency
- [[async-task|async task]] — runtime-darajasidagi asinxron concurrency birligi
- [[future|Future trait]] — eng mayda concurrency birligi
- [[cpu-bound|CPU-bound]] → thread afzal
- [[io-bound|I/O-bound]] → async afzal
- [[concurrency]] — uch darajali model

## Code Examples

- [[thread-async-combined]] — thread produce + async consume

## Exercises or Practice Ideas

1. Video encoding simulatsiyasi: CPU-bound ish uchun thread, progress update uchun async channel.
2. Thread va async task performance'ini I/O-bound ish bilan solishtiring.

## Questions Raised

- Work stealing qanday ishlaydi va nima uchun thread va task ikkalasi kerak?
- Embedded sistemada thread yo'q bo'lsa async qanday ishlaydi?

## Links To Update

- [[wiki/chapters/17-6-futures-tasks-and-threads]] — yangi chapter page
- [[concurrency]] — uch darajali model
