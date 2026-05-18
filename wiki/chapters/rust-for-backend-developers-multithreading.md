---
title: "Rust for Backend Developers: Multithreading"
type: chapter
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, backend, multithreading, chapter, advance]
source_count: 1
---

# Rust for Backend Developers: Multithreading

## Learning Goal

Backend kodda OS thread, ownership transfer, shared state synchronization, scoped borrowing, atomics, va channel coordination orasidagi boundary'larni ajratish.

## Main Ideas

- `thread::spawn` oddiy function call emas; u `FnOnce + Send + 'static` contract'ini talab qiladi.
- `JoinHandle` natija olish va thread tugashini kutish nuqtasi, `thread::Builder` esa thread name va stack size kabi operational sozlamalarni beradi.
- `Send` ownership transfer xavfsizligini, `Sync` esa `&T`ni bir nechta thread'da ishlatish xavfsizligini bildiradi.
- Shared mutable state uchun `Arc<Mutex<T>>` basic pattern, lekin `RwLock`, `Condvar`, va `Barrier` har biri boshqa coordination muammosini hal qiladi.
- `MutexGuard` lifetime'i amaliy xulqni keskin o'zgartiradi; compile bo'lgan kod lock scope sabab serial ishlashi mumkin.
- Scoped threads local variable'larni `move` va `Arc`siz borrow qilish imkonini beradi.
- Atomics counter va flag kabi tor state uchun qulay, lekin ordering noto'g'ri tanlansa "race-free" bo'lsa ham mantiq buzilishi mumkin.
- `channel` queue beradi, `sync_channel` esa backpressure ham beradi; shutdown ko'pincha sender close bilan boshqariladi.

## Concepts

- [[threads]]
- [[thread-builder]]
- [[join-handle]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[mutex-t|Mutex<T>]]
- [[poisoned-mutex]]
- [[rwlock|RwLock]]
- [[condvar|Condvar]]
- [[barrier|Barrier]]
- [[scoped-threads]]
- [[atomic-types]]
- [[atomic-memory-ordering]]
- [[compare-exchange]]
- [[thread-local-storage]]
- [[channels]]
- [[sync-channel]]

## Examples

```rust
let t1 = std::thread::spawn(|| 1);
let t2 = std::thread::spawn(|| 2);
let sum = t1.join().unwrap() + t2.join().unwrap();
```

```rust
std::thread::scope(|scope| {
    scope.spawn(|| println!("{s}"));
});
```

```rust
let counter = std::sync::atomic::AtomicI32::new(0);
counter.fetch_add(1, std::sync::atomic::Ordering::Relaxed);
```

```rust
let (tx, rx) = std::sync::mpsc::sync_channel::<i32>(3);
```

## Exercises

- `Rc<T>` vs `Arc<T>` va `Cell<T>` vs `Mutex<T>` juftliklarini thread-safety nuqtai nazaridan jadvalga tushiring.
- `Mutex`, `RwLock`, `Condvar`, `Barrier` uchun bittadan real backend scenario yozing.
- `compare_exchange` bilan lock-free increment loop yozib, `fetch_add` bilan solishtiring.
- `sync_channel(1)` bilan producer side blocking xulqini kuzating.

## Review Questions

- Nega `thread::spawn` local reference'larni oddiy borrow bilan qabul qilmaydi?
- `Send` va `Sync` nega ikki alohida marker trait?
- Qachon `Mutex<T>` yetarli, qachon `RwLock<T>` foydaliroq?
- `Relaxed` ordering nimani kafolatlaydi, nimani kafolatlamaydi?
- Channel close shutdown va explicit sentinel message o'rtasidagi farq nima?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-multithreading]]
- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/16-fearless-concurrency]]
- [[wiki/chapters/17-6-futures-tasks-and-threads]]
