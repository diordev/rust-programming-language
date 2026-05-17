---
title: "17. Fundamentals of Asynchronous Programming: Async, Await, Futures, and Streams"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 1
---

# 17. Fundamentals of Asynchronous Programming

## Source

- URL: https://doc.rust-lang.org/stable/book/ch17-00-async-await.html
- Raw: `raw/books/the_rust_programming_language/17. Fundamentals of Asynchronous Programming Async, Await, Futures, and Streams.md`

## Detailed Summary

17-bob async programmingni kirish sifatida tushuntiradi. Bob 16-bobda o'rganilgan threadlar bilan paralleldagi concurrencyni yoritib, unga muqobil yondashuv — Rust'ning `async`/`await` mexanizmini taqdim etadi.

### CPU-bound va I/O-bound operatsiyalar

Bob ikkita turli xil ko'p vaqt talab qiladigan operatsiyani farqlaydi:

- **CPU-bound**: Video eksport kabi operatsiya — CPU/GPU kuchi bilan chegaralangan, hisoblash tezligiga bog'liq.
- **I/O-bound**: Fayl yuklab olish kabi operatsiya — tarmoq tezligi bilan chegaralangan, CPU ko'p vaqt kutadi.

Operatsion tizim har ikkala holatda ham dasturni ko'rinmas tarzda to'xtatib boshqasini ishlatadi — bu concurrency dastur darajasida emas, _butun jarayon_ darajasida yuz beradi.

### Blocking muammo

Ko'pgina tarmoq API'lari **blocking**: ma'lumot tayyor bo'lguncha butun thread to'xtaydi. Yechim sifatida har bir yuklab olish uchun alohida thread yaratish mumkin, ammo bu resurslar sarfiga olib keladi va kengaytirilishi qiyin.

Async yondashuv: runtime qaysi vazifani qachon bajarish kerakligini o'zi belgilaydi, kod esa "potentsial pauza nuqtalari" bilan yoziladi.

### Parallelism vs Concurrency

Bu ikki tushunchani aniq farqlash zarur:

| Tushuncha | Ma'nosi |
|-----------|---------|
| **Concurrency** | Bir kishi bir necha vazifani navbat bilan bajaradi — bitta vaqtda faqat bittasi |
| **Parallelism** | Bir necha kishi har biri bir vazifani bir vaqtda bajaradi |
| **Serial** | Vazifalar ketma-ket, biri boshqa tugagandan keyin |

Amalda bu ikkalasi aralashishi mumkin: qisman parallel, qisman serial. Async Rust odatda **concurrent** ishlaydi; runtime va hardware ga qarab parallelism ham qo'shilishi mumkin.

### Async Rust nima beradi

- `async` va `await` keywordslarini ishlatib, operatsiyalar qanday asynchronous bo'lishi mumkinligini ifodalash imkoni.
- Runtime (uchinchi tomon cratelari) asinxron operatsiyalarni boshqaradi.
- Bob 16 mavzulari — bir nechta yuklab olishni boshqarish, UI ni qulflamaslik — async bilan yanada qulay yechiladi.

## Key Concepts

- [[cpu-bound|CPU-bound]] — CPU/GPU resurslari bilan chegaralangan operatsiya
- [[io-bound|I/O-bound]] — kirish/chiqish tezligi bilan chegaralangan operatsiya
- [[async-await|async/await]] — asynchronous kodni ifodalash uchun Rust sintaksisi
- [[concurrency]] — mustaqil ishlash, aralashuv bilan
- [[async-runtime|async runtime]] — async kodni boshqaruvchi va bajoruvchi runtime

## Code Examples

Bob bu bo'limda kod misol keltirmaydi — keyingi bo'limlarga kirish sifatida ishlaydi.

## Exercises or Practice Ideas

1. CPU-bound va I/O-bound operatsiyalar sifatida haqiqiy hayotdan 3 tadan misol keltiring.
2. Concurrency va parallelism o'rtasidagi farqni diagramma yordamida tushuntiring.
3. `blocking` funksiya nima? Misolda ko'rsating.

## Questions Raised

- Rust'ning async runtime'i nima uchun standart kutubxonaga kirmagan? (`tokio`, `async-std` kabi uchinchi tomon cratelari nima uchun kerak?)
- `async` concurrency va `thread` concurrency qachon birinchisi afzalroq?

## Links To Update

- [[17-async-programming]] — yangi chapter page
- [[async-await]] — misollar qo'shilishi kerak
- [[concurrency]] — async muqobil qo'shilishi kerak
