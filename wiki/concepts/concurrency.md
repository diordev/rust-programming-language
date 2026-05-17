---
title: "Concurrency"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-13
tags: [rust, concurrency]
source_count: 8
---

# Concurrency

## Short Definition

Dasturning turli qismlari **mustaqil** (concurrent) yoki **bir vaqtda** (parallel) bajariladigan execution modeli. Rust bu ikkalasini qo'llab-quvvatlaydi va ownership + type system orqali ko'plab xatolarni **compile time'da** tutadi.

## Why It Matters

Ko'p protsessorli hisoblash kuchidan foydalanish uchun concurrency zarur. Ammo an'anaviy tillarda race conditions, deadlocks, va hard-to-reproduce buglar keng tarqalgan. Rust **fearless concurrency** falsafasi: noto'g'ri concurrent kod kompilyatsiyadan o'tmaydi — runtime debug o'rniga compiler xato ko'rsatadi.

## Mental Model

Ownership va borrowing qoidalari threadlar orasida ham to'liq amal qiladi:
- Bitta mutable referens yoki ko'p immutable referenslar — shu qoida threadlarda ham.
- [[data-race|data race]] = compile time xato, runtime emas.
- Thread'ga o'tkaziladigan qiymat `Send` bo'lishi kerak; shared referens `Sync`.

Erlang kabi tillar faqat message-passing'ni qo'llaydi. Rust moslashuvchan: message-passing (channels), shared state (`Mutex<T>`), va boshqa yondashuvlar hammasini qamraydi.

Amaliy server design'larida concurrency faqat "ko'proq thread" degani emas. [[thread-pool]] kabi fixed-size yondashuvlar throughput'ni oshirib, shu bilan birga resource usage'ni nazorat qiladi.

## Syntax and Examples

```rust
use std::thread;
use std::time::Duration;

// Asosiy thread yaratish
let handle = thread::spawn(|| {
    println!("spawned thread ishlayapti");
    thread::sleep(Duration::from_millis(1));
});

handle.join().unwrap(); // spawned tugashini kut
```

Qarang: [[threads]] va [[thread-spawn-basic]].

## Common Mistakes

- **Concurrent va parallel'ni bir xil deb ishlatish** — concurrent = mustaqil (overlap mumkin), parallel = bir vaqtda.
- **`move` siz closure thread'da** — E0373 chiqadi; `thread::spawn(move || ...)` kerak.
- **`join` ni unutish** — main thread tugashi bilan spawned threadlar ham to'xtaydi.
- **Deadlock** — ikki thread bir-birini kutib qolsa, hech biri davom etolmaydi.
- **Har request uchun cheksiz thread yaratish** — concurrency bersa ham operational jihatdan xavfli.
- **Lock lifetime'ni e'tiborsiz qoldirish** — compile bo'lgan kod ham amalda serial ishlashi mumkin. Qarang: [[mutexguard-lifetime]].

## Related Concepts

- [[ownership]]
- [[borrowing]]
- [[data-race|data race]]
- [[threads]]
- [[join-handle]]
- [[move-closures-threads]]
- [[channels]]
- [[message-passing]]
- [[mutex-t|Mutex<T>]]
- [[arc-t|Arc<T>]]
- [[thread-pool]]
- [[worker-thread]]
- [[job-queue]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[async-await|async/await]]
- [[future|Future trait]]
- [[async-runtime|async runtime]]
- [[io-bound|I/O-bound]]
- [[cpu-bound|CPU-bound]]

## Sources

- [[wiki/sources/0-2-introduction]]
- [[wiki/sources/16-fearless-concurrency]]
- [[16-1-using-threads-to-run-code-simultaneously]]
- [[16-2-transfer-data-between-threads-with-message-passing]]
- [[16-3-shared-state-concurrency]]
- [[16-4-extensible-concurrency-with-send-and-sync]]
- [[17-fundamentals-of-asynchronous-programming]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
