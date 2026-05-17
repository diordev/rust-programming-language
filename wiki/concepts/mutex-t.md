---
title: "Mutex<T>"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-13
tags: [rust, concurrency, threads, mutex, interior-mutability]
source_count: 2
---

# Mutex<T>

## Short Definition

`Mutex<T>` — *mutual exclusion* (o'zaro chiqarib qo'yish). Bir vaqtda faqat bitta thread mutex tomonidan saqlanadigan ma'lumotga kirish huquqini oladi. `std::sync::Mutex` — thread-safe interior mutability vositasi.

## Why It Matters

Shared mutable state xavfli: bir nechta thread bir xil ma'lumotni o'gartirishga urindi — data race. Mutex esa bir vaqtda faqat bitta threadga kirish imkonini beradi va Rust ushbu qoidani *type system* orqali kafolatlaydi: lock olmay turib ichki qiymatga kira olmaysiz.

## Mental Model

Konferensiya mikrofoni: gaplashmoqchi bo'lgan har bir kishi avval so'raydi, olgach gapiradi, tugagach keyingiga beradi. Agar birttasi mikrofon berish yodidan chiqsa — hamma kutib qoladi (**deadlock**). Rust `MutexGuard` drop bo'lganda avtomatik ravishda lock'ni qo'yib beradi — unutish imkoni yo'q.

## Syntax and Examples

### Oddiy ishlatilish (single-threaded)

```rust
use std::sync::Mutex;

let m = Mutex::new(5);

{
    let mut num = m.lock().unwrap(); // MutexGuard<i32> qaytaradi
    *num = 6;
} // MutexGuard scope'dan chiqdi → lock avtomatik qo'yildi

println!("m = {m:?}"); // Mutex { data: 6, ... }
```

### Ko'p threadda: `Arc<Mutex<T>>`

```rust
use std::sync::{Arc, Mutex};
use std::thread;

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let counter = Arc::clone(&counter);
    let handle = thread::spawn(move || {
        let mut num = counter.lock().unwrap();
        *num += 1;
    }); // num scope'dan chiqdi → lock avtomatik qo'yildi
    handles.push(handle);
}

for handle in handles {
    handle.join().unwrap();
}

println!("Result: {}", *counter.lock().unwrap()); // 10
```

## `MutexGuard<T>`

`.lock()` `LockResult<MutexGuard<T>>` qaytaradi. `MutexGuard<T>`:
- `Deref` implement qiladi → ichki qiymatga `*guard` orqali kirish.
- `Drop` implement qiladi → scope'dan chiqganda lock **avtomatik** qo'yiladi.

Agar lock tutayotgan thread panic qilsa, `.lock()` `Err` qaytaradi — "poisoned mutex".

### Lock lifetime nuance

Thread pool misolida quyidagi farq juda muhim:

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

## Interior Mutability

`Mutex<T>` immutable ko'rinishda turib mutable kirish imkonini beradi — xuddi `RefCell<T>` kabi, lekin thread-safe:

| | Single-threaded | Multi-threaded |
|---|---|---|
| Ichki mutabillik | `RefCell<T>` | `Mutex<T>` |
| Ko'p egalilik | `Rc<T>` | `Arc<T>` |
| Kombinatsiya | `Rc<RefCell<T>>` | `Arc<Mutex<T>>` |

## Deadlock xavfi

`Mutex<T>` deadlock xataridan himoya qilmaydi. Masalan, bitta thread bir mutex'ni ushlab ikkinchisini kutsa, ikkinchi thread esa birinchisini — ikkisi bir-birini abadiy kutadi. Rust deadlock'ni compile time'da aniqlamaydi.

## Common Mistakes

- **`Rc<Mutex<T>>` threadda ishlatish** — E0277: `Rc<T>` `Send` emas. O'rniga `Arc<Mutex<T>>`.
- **Lock ni scope'dan chiqmasdan yangi lock olishga urinish** — deadlock.
- **`unwrap()` bilan poisoned mutex'ni e'tiborsiz qoldirish** — panic bo'lgan thread mutex'ni "zaharlab" ketadi.
- **`MutexGuard`'ni uzoq ushlab turish** — boshqa threadlar bloklanadi; faqat kerakli ishni bajargach darhol drop qiling.
- **Temporary lifetime'larni noto'g'ri taxmin qilish** — `while let` yoki `match` ichidagi guard kutilganidan uzoq yashashi mumkin.

## Related Concepts

- [[arc-t|Arc<T>]] — thread-safe ko'p egalilik
- [[interior-mutability]] — `Mutex<T>` vs `RefCell<T>`
- [[concurrency]]
- [[threads]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[rc-t|Rc<T>]] — single-threaded alternativ
- [[refcell-t|RefCell<T>]] — single-threaded alternativ
- [[data-race]]
- [[mutexguard-lifetime]]
- [[thread-pool]]

## Sources

- [[16-3-shared-state-concurrency]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
