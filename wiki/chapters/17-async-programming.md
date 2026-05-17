---
title: "17. Fundamentals of Asynchronous Programming"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 2
---

# 17. Fundamentals of Asynchronous Programming

## Learning Goal

[[async-await|async/await]] sintaksisini o'rganib, I/O-bound operatsiyalarni thread'larsiz concurrent bajarish modelini tushunish. 16-bobda o'rganilgan thread-based concurrency bilan solishtirish.

## Main Ideas

### Nima uchun async kerak?

Dasturlar ko'pincha uzoq kutish talab qiladigan operatsiyalarni bajaradi:

- **I/O-bound**: tarmoqdan ma'lumot yuklash, fayllardan o'qish — CPU ko'p vaqt bekor turadi
- **CPU-bound**: video render qilish, hisoblash — CPU/GPU to'liq band bo'ladi

I/O-bound holatlarda blocking API ishlatilsa, butun thread ma'lumot kelguncha to'xtaydi. Threadlar esa arzon emas — ko'p thread yaratish xotira va OS resurslarini sarflaydi.

**Async yechim**: runtime qaysi vazifani qachon davom ettirish kerakligini o'zi boshqaradi; kod "pauza bo'lishi mumkin bo'lgan joylar"ni ko'rsatadi va boshqa ishni kutayotganda o'sha nuqtalarga qaytiladi.

### Parallelism vs Concurrency (aniq ta'rif)

```
Concurrency:  [A1 → B1 → A2 → B2 → A3 → B3]   (bitta thread, navbat)
Parallelism:  [A1 → A2 → A3]                    (parallel threads)
              [B1 → B2 → B3]
```

| | Concurrency | Parallelism |
|---|---|---|
| Thread soni | Bitta ham bo'lishi mumkin | Ko'p core kerak |
| Ishlash uslubi | Navbat bilan almashinadi | Haqiqiy bir vaqtda |
| Async Rust | ✓ asosiy model | Runtime orqali mumkin |

Async Rust odatda **concurrent** ishlaydi. Runtime va hardware ga qarab parallelism ham bo'lishi mumkin.

### Blocking muammo va async yechim

```rust
// Blocking yondashuv (har bir yuklab olish uchun thread):
for url in urls {
    std::thread::spawn(|| download(url)); // resurs sarflanadi
}

// Async yondashuv (runtime boshqaradi):
for url in urls {
    tokio::spawn(async { fetch(url).await }); // yengil "task"
}
```

## Concepts

- [[async-await|async/await]] — asynchronous kodni ifodalash sintaksisi
- [[future|Future]] — lazy asinxron qiymat
- [[cpu-bound|CPU-bound]] — CPU resurslari bilan chegaralangan operatsiya
- [[io-bound|I/O-bound]] — kirish/chiqish tezligi bilan chegaralangan operatsiya
- [[concurrency]] — bir necha mustaqil ishni aralashtirib bajarish
- [[async-runtime|async runtime]] — async kodni boshqaruvchi executor

## Examples

Ushbu kirish bo'limida kod misol yo'q. Misollar [[17-1-futures-and-async-syntax|17.1]] dan boshlanadi.

## Exercises

1. I/O-bound va CPU-bound uchun haqiqiy hayotdan 2 tadan misol toping.
2. "Concurrency" va "parallelism"ning Rust'dagi farqini o'z so'zingiz bilan tushuntiring.
3. Blocking funksiya nima uchun async kontextda muammo? Tushuntiring.

## Review Questions

1. Nima uchun har bir I/O operatsiyasi uchun yangi thread yaratish optimal emas?
2. Rust async'i odatda concurrent ishlaydi deb aytiladi — bu nima ma'noni anglatadi?
3. Async va thread'li concurrencyning asosiy farqi nima?

## Related Pages

- [[wiki/chapters/16-fearless-concurrency|16. Fearless Concurrency]] — thread'li concurrency
- [[17-1-futures-and-async-syntax|17.1. Futures and the Async Syntax]] — birinchi amaliy async kod
- [[17-fundamentals-of-asynchronous-programming|Source: 17. Async Programming Intro]]
