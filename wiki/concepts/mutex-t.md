---
title: "Mutex<T>"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-18
tags: [rust, concurrency, threads, mutex, interior-mutability]
source_count: 4
---

# Mutex<T>

## Short Definition

`Mutex<T>` — *mutual exclusion*. Bir vaqtda faqat bitta thread mutex tomonidan saqlanadigan ma'lumotga kirish huquqini oladi. `std::sync::Mutex` — thread-safe interior mutability vositasi.

## Why It Matters

Shared mutable state xavfli: bir nechta thread bir xil ma'lumotni o'zgartirishga urindi — data race. Mutex esa bir vaqtda faqat bitta threadga kirish imkonini beradi va Rust ushbu qoidani *type system* orqali kafolatlaydi: lock olmay turib ichki qiymatga kira olmaysiz.

## Mental Model

Konferensiya mikrofoni: gaplashmoqchi bo'lgan har bir kishi avval so'raydi, olgach gapiradi, tugagach keyingiga beradi. Rust `MutexGuard` drop bo'lganda avtomatik ravishda lock'ni qo'yib beradi.

## Syntax and Examples

### Oddiy ishlatilish

```rust
use std::sync::Mutex;

let m = Mutex::new(5);

{
    let mut num = m.lock().unwrap();
    *num = 6;
}
```

### Ko'p threadda: `Arc<Mutex<T>>`

```rust
use std::sync::{Arc, Mutex};

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let counter = Arc::clone(&counter);
    handles.push(std::thread::spawn(move || {
        let mut num = counter.lock().unwrap();
        *num += 1;
    }));
}

for handle in handles {
    handle.join().unwrap();
}
```

## `MutexGuard<T>`

`.lock()` `LockResult<MutexGuard<T>>` qaytaradi. `MutexGuard<T>`:
- `Deref` implement qiladi.
- `Drop` implement qiladi.

Agar lock tutayotgan thread panic qilsa, `.lock()` `Err` qaytaradi — "poisoned mutex".

### Lock lifetime nuance

```rust
let job = receiver.lock().unwrap().recv().unwrap();
job();
```

Bu yerda `MutexGuard` statement oxirida drop bo'ladi, demak `job()` lock'siz bajariladi.

```rust
while let Ok(job) = receiver.lock().unwrap().recv() {
    job();
}
```

Bu variant compile bo'ladi, lekin `MutexGuard` loop body oxirigacha yashaydi. Natijada `job()` ishlayotgan paytda ham boshqa workerlar lock ola olmaydi. Qarang: [[mutexguard-lifetime]].

### Poison recovery

`PoisonError` mutlaq blok emas. Zarur bo'lsa `err.into_inner()` bilan guard'ni olish, state'ni audit qilish, va `clear_poison()` bilan flag'ni tushirish mumkin.

## Interior Mutability

`Mutex<T>` immutable ko'rinishda turib mutable kirish imkonini beradi — xuddi `RefCell<T>` kabi, lekin thread-safe:

| | Single-threaded | Multi-threaded |
|---|---|---|
| Ichki mutabillik | `RefCell<T>` | `Mutex<T>` |
| Ko'p egalilik | `Rc<T>` | `Arc<T>` |
| Kombinatsiya | `Rc<RefCell<T>>` | `Arc<Mutex<T>>` |

## Deadlock xavfi

`Mutex<T>` deadlock xataridan himoya qilmaydi.

## Common Mistakes

- **`Rc<Mutex<T>>` threadda ishlatish** — E0277.
- **Lock ni scope'dan chiqmasdan yangi lock olishga urinish**.
- **`unwrap()` bilan poisoned mutex'ni e'tiborsiz qoldirish**.
- **`MutexGuard`'ni uzoq ushlab turish**.
- **Temporary lifetime'larni noto'g'ri taxmin qilish**.
- **Global state ichidagi `Mutex<T>`ni uzoq computation yoki I/O davomida ushlab turish**.

## Related Concepts

- [[arc-t|Arc<T>]]
- [[interior-mutability]]
- [[concurrency]]
- [[threads]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[rc-t|Rc<T>]]
- [[refcell-t|RefCell<T>]]
- [[data-race]]
- [[mutexguard-lifetime]]
- [[poisoned-mutex]]
- [[thread-pool]]
- [[global-state]]

## Sources

- [[16-3-shared-state-concurrency]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/rust-for-backend-developers-multithreading]]
- [[wiki/sources/rust-for-backend-developers-global-data]]
