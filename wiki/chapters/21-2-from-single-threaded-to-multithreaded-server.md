---
title: "21.2. From Single-Threaded to Multithreaded Server"
type: chapter
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, web-server, thread-pool, concurrency]
source_count: 1
---

# 21.2. From Single-Threaded to Multithreaded Server

## Learning Goal

Single-threaded server bottleneck'ini ko'rib, cheksiz `thread::spawn` yondashuvidan fixed-size [[thread-pool]] arxitekturasiga o'tish. Shu jarayonda `FnOnce + Send + 'static`, [[channels]], [[arc-t|Arc<T>]], [[mutex-t|Mutex<T>]], va temporary lifetime'larning amaliy ahamiyatini tushunish.

## Main Ideas

- `*/sleep*` request server throughput muammosini ko'zga ko'rinadigan qiladi.
- Har request uchun alohida `thread::spawn` concurrency beradi, lekin resursni nazoratsiz ishlatadi.
- Fixed-size [[thread-pool]] DoS riskini kamaytiradi va queue orqali backpressure beradi.
- Desired API avval yozilib, implementation compiler xatolari bo'yicha quriladi: [[compiler-driven-development]].
- `execute` job'lari `FnOnce() + Send + 'static` bound'lariga ega bo'ladi.
- `mpsc::Receiver<Job>` std channel modelida single-consumer; bir nechta worker uchun [[arc-t|Arc<Mutex<Receiver<Job>>>]] kerak.
- `MutexGuard` lifetime'i noto'g'ri scope'da qolsa concurrency amalda serial bo'lib qolishi mumkin.

## Evolution Path

| Qadam | Mavzu | Nega kerak |
|------|-------|------------|
| 21-10 | `*/sleep*` request | bottleneck'ni ko'rsatish |
| 21-11 | `thread::spawn` per request | muammoning naive concurrent yechimi |
| 21-12 | `ThreadPool::new` + `execute` | ergonomik target API |
| 21-13..21-15 | `ThreadPool`, `Worker`, `assert!` | internal structure |
| 21-16..21-18 | channel + `Arc<Mutex<Receiver<Job>>>` | job distribution |
| 21-19..21-20 | `Job` alias + worker loop | ishlaydigan fixed pool |
| 21-21 | `while let` pitfall | lock lifetime nuance |

## Core Design

### Thread-per-request

```rust
thread::spawn(move || {
    handle_connection(stream);
});
```

Bu ishlaydi, lekin cheksiz thread growth xavfi bor.

### Ideal pool API

```rust
let pool = ThreadPool::new(4);

pool.execute(|| {
    handle_connection(stream);
});
```

### Job representation

```rust
type Job = Box<dyn FnOnce() + Send + 'static>;
```

### Shared receiver

```rust
let receiver = Arc::new(Mutex::new(receiver));
```

## Concepts

- [[thread-pool]]
- [[worker-thread]]
- [[job-queue]]
- [[compiler-driven-development]]
- [[mutexguard-lifetime]]
- [[channels]]
- [[fn-traits|Fn traits]]
- [[send-trait|Send trait]]
- [[static-lifetime|static lifetime]]
- [[type-alias]]
- [[arc-t|Arc<T>]]
- [[mutex-t|Mutex<T>]]
- [[web-server]]

## Examples

- [[slow-sleep-request|Listing 21-10]]
- [[per-request-thread-spawn|Listing 21-11]]
- [[threadpool-new-and-execute|Listing 21-12]]
- [[arc-mutex-mpsc-receiver|Listing 21-18]]
- [[job-boxed-fnonce-alias|Listings 21-19 and 21-20]]
- [[while-let-mutexguard-pitfall|Listing 21-21]]

## Exercises

1. `ThreadPool::build(size) -> Result<_, _>` variantini tasavvur qilib, `new` bilan tradeoff'ini yozing.
2. `Job` alias nega `Fn()` emas, `FnOnce()` ekanini misol bilan tushuntiring.
3. `Receiver<Job>`ni `Arc`siz va `Mutex`siz ulashish nega mumkin emasligini ikki alohida sabab bilan yozing.
4. `while let` varianti nega compile bo'lib turib ham concurrency'ni sindiradi?
5. 4 worker va 10 ta `*/sleep*` request bo'lsa queue qanday ishlashini vaqt chizig'i bilan chizing.
6. `async` versiyada qaysi qismlar executor/task modeliga ko'chishini yozing.

## Review Questions

1. Single-threaded bottleneck qanday ko'rsatildi?
2. Nega per-request `thread::spawn` final yechim emas?
3. Fixed-size thread pool DoS riskini qanday kamaytiradi?
4. `ThreadPool::execute` nega `FnOnce() + Send + 'static` talab qiladi?
5. `assert!(size > 0)` qaysi invalid holatni ushlaydi?
6. `Worker` struct nega faqat `JoinHandle<()>`dan yaxshiroq abstraction?
7. `mpsc` channel modelida nega `Receiver` clone qilinmaydi?
8. `Arc<Mutex<Receiver<Job>>>` ichida `Arc` va `Mutex`ning rollari alohida nima?
9. `let job = receiver.lock().unwrap().recv().unwrap();` va `while let Ok(job) = ...` orasidagi muhim lifetime farqi nima?
10. Bu bo'lim compiler-driven developmentni qanday namoyish qiladi?

## Related Pages

- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|Source: 21.2]]
- [[wiki/chapters/21-final-project-building-a-multithreaded-web-server|Chapter 21]]
- [[wiki/chapters/21-1-building-a-single-threaded-web-server|Chapter 21.1]]
- [[wiki/chapters/16-fearless-concurrency|Chapter 16]]
- [[17-async-programming|Chapter 17]]
