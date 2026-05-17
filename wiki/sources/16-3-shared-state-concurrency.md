---
title: "16.3. Shared-State Concurrency"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, threads, mutex, arc]
source_count: 1
source_path: "raw/books/the_rust_programming_language/16.3. Shared-State Concurrency.md"
---

# 16.3. Shared-State Concurrency

## Source

- **Fayl:** `raw/books/the_rust_programming_language/16.3. Shared-State Concurrency.md`
- **URL:** https://doc.rust-lang.org/stable/book/ch16-03-shared-state.html

## Detailed Summary

### Shared memory vs message passing

Channels = single ownership (yuborilgandan keyin asl scope egalik qilmaydi). Shared memory concurrency = **multiple ownership**: bir nechta thread bir xil xotira joyiga kirish huquqi. Ch15 smart pointers kabi — ko'p egalilik murakkablik qo'shadi, lekin Rust type system yordamida to'g'ri boshqarish mumkin.

### Mutex — mutual exclusion

Mutex bitta vaqtda faqat bitta threadga kirishni ruxsat beradi. Ikkita qoida:
1. Data ishlatishdan oldin **lock** olish kerak.
2. Ishlatib bo'lgach **unlock** qilish kerak.

Rust `MutexGuard` orqali ikkinchi qoidani avtomatlashtiradi: guard scope'dan chiqqanda lock qo'yiladi.

### `Mutex<T>` API

```rust
use std::sync::Mutex;

let m = Mutex::new(5);
{
    let mut num = m.lock().unwrap(); // MutexGuard<i32>
    *num = 6;
} // lock avtomatik qo'yildi
println!("m = {m:?}");
```

`.lock()` `LockResult<MutexGuard<T>>` qaytaradi. Agar lock tutayotgan thread panic qilsa — "poisoned mutex" — `Err` qaytaradi.

### Muammo: `Mutex<T>`'ni loop da ko'p threadga berish — E0382

Yolg'iz `Mutex<T>` faqat bitta egalikka ega. Loop'da `move` qilganda birinchi iteratsiyadan keyin yo'q.

### Xato urinish: `Rc<Mutex<T>>` — E0277

`Rc<T>` `Send` implement qilmaydi — reference count atomic emas. `thread::spawn` `Send` bound talab qiladi:
```
`Rc<Mutex<i32>>` cannot be sent between threads safely
```

### To'g'ri yechim: `Arc<Mutex<T>>`

`Arc<T>` — *Atomically Reference Counted*. `Rc<T>` bilan bir xil API, lekin reference count atomik. `Send + Sync` implement qiladi.

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
    });
    handles.push(handle);
}

for handle in handles { handle.join().unwrap(); }
println!("Result: {}", *counter.lock().unwrap()); // 10
```

Nima uchun `Arc<T>` standart emas? Atomic operatsiyalar overhead qo'shadi — single-threaded kodda keraksiz.

### `Mutex<T>` va interior mutability

`counter` immutable, lekin `Mutex<T>` orqali ichki qiymatni o'zgartirish mumkin. Xuddi `RefCell<T>` kabi interior mutability, lekin thread-safe versiyasi:

| Pattern | Single-threaded | Multi-threaded |
|---------|----------------|----------------|
| Ko'p egalilik | `Rc<T>` | `Arc<T>` |
| Interior mut | `RefCell<T>` | `Mutex<T>` |

### Deadlock xavfi

`Rc<T>` reference cycle kabi, `Mutex<T>` deadlock xatari bor. Rust bu xatardan himoya qilmaydi — dizayn masalasi. Standard library `Mutex<T>` va `MutexGuard` hujjatlarida strategiyalar keltirilgan.

## Key Concepts

- [[mutex-t|Mutex<T>]]
- [[arc-t|Arc<T>]]
- [[interior-mutability]]
- [[concurrency]]
- [[threads]]
- [[send-trait|Send trait]]
- [[rc-t|Rc<T>]] — thread-unsafe analog
- [[refcell-t|RefCell<T>]] — single-threaded analog

## Code Examples

- [[mutex-arc-counter]] — E0382 → Rc xatosi → Arc yechimi progressi

## Exercises or Practice Ideas

1. Single-threaded `Mutex<T>` ishlatib lock/unlock semantikasini tushuning.
2. Loop da `Mutex<T>` move qilishga urinib E0382 ni o'qing.
3. `Rc<Mutex<T>>` ishlatishga urinib E0277 ni o'qing va nima uchun `Send` yo'qligini tushuntiring.
4. `Arc<Mutex<T>>` bilan 10 thread counter dasturni yozib, natijani tekshiring.
5. Bir xil `Mutex` ni bir thread ichida ikki marta lock olishga urinib deadlock holatini hosil qiling.

## Questions Raised

- `RwLock<T>` nima, va `Mutex<T>`dan qanday farqlanadi?
- Atomic typlar (`AtomicUsize`, `AtomicBool`) qachon `Mutex<T>`dan yaxshiroq?

## Links To Update

- [[wiki/chapters/16-fearless-concurrency]]
- [[mutex-t|Mutex<T>]]
- [[arc-t|Arc<T>]]
- [[index]]
