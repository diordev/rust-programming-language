---
title: "Thread Pool"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, concurrency, threads, web-server]
source_count: 2
---

# Thread Pool

## Short Definition

Oldindan spawn qilingan, qayta ishlatiladigan worker threadlar to'plami. Yangi task kelganda har safar yangi thread yaratish o'rniga, ish shu pool ichidagi bo'sh workerga topshiriladi.

## Why It Matters

[[web-server]] uchun fixed-size thread pool ikki asosiy muammoni hal qiladi:

- sekin request sabab boshqa request'lar bekor turib qolmasligi;
- har request uchun cheksiz thread yaratib, resursni tugatib yubormaslik.

Bu throughput va resource control o'rtasidagi amaliy balansdir.

21.3 bilan yana bitta talab qo'shiladi: pool faqat request bajarishi emas, balki [[graceful-shutdown]] paytida toza tugashi ham kerak.

## Mental Model

Restorandagi navbatchi oshpazlar: oldindan 4 ishchi tayyor turadi. Buyurtma kelganda yangi oshpaz yollanmaydi; navbatdagi tayyor ishchi buyurtmani oladi. Agar ishchilar band bo'lsa, buyurtmalar queue'da kutadi.

## Syntax and Examples

### Ideal API

```rust
let pool = ThreadPool::new(4);

pool.execute(|| {
    handle_connection(stream);
});
```

### Job flow

1. `execute` closure'ni qabul qiladi
2. uni queue'ga yuboradi
3. worker thread queue'dan oladi
4. `job()` qilib bajaradi

### Shutdown lifecycle

1. Pool endi yangi job yubormaslik uchun `sender`ni yopadi.
2. Workerlar qolgan job'larni tugatadi yoki `recv()`dan `Err` oladi.
3. Har worker loop'dan chiqadi.
4. Pool `join()` bilan worker threadlar tugashini kutadi.

## Common Mistakes

- **Har request uchun `thread::spawn`ni pool deb o'ylash**: bu pool emas, cheksiz growth.
- **Pool size'ni nol qilish**: `0` worker ma'nosiz.
- **Pool size'ni doim CPU count bilan tenglashtirish**: I/O-heavy workload'lar uchun bu doim optimal emas.
- **Shutdown strategiyasini unutish**: ishlaydigan pool hali production-ready lifecycle degani emas.

## Related Concepts

- [[worker-thread]]
- [[job-queue]]
- [[threads]]
- [[concurrency]]
- [[channels]]
- [[web-server]]
- [[graceful-shutdown]]

## Sources

- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
