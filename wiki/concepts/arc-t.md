---
title: "Arc<T>"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-13
tags: [rust, concurrency, threads, arc, reference-counting, smart-pointers]
source_count: 2
---

# Arc<T>

## Short Definition

`Arc<T>` — *Atomically Reference Counted*. `Rc<T>` bilan bir xil API, lekin reference counting **atomik operatsiyalar** orqali bajariladi — threadlar orasida xavfsiz. `std::sync::Arc`.

## Why It Matters

`Rc<T>` single-threaded: reference count oddiy integer, atomic emas — ikki thread bir vaqtda count'ni o'zgartirsa, noto'g'ri hisob, memory leak yoki double-free. `Arc<T>` esa count'ni atomic tarzda boshqaradi — `Send + Sync` implement qiladi.

Nima uchun `Arc<T>` standart emas? **Performance**: atomic operatsiyalar oddiy integer operatsiyalardan sekinroq. Single-threaded kodda bu overhead keraksiz — shuning uchun `Rc<T>` va `Arc<T>` alohida turibdi.

## Mental Model

`Rc<T>` = oddiy hisoblagich. `Arc<T>` = atom bilan himoyalangan hisoblagich. API bir xil, lekin `Arc` thread-safe bo'lgani uchun biroz qimmatroq.

## Syntax and Examples

### Ko'p threadda shared ownership

```rust
use std::sync::Arc;
use std::thread;

let val = Arc::new(String::from("hello"));

let val2 = Arc::clone(&val); // reference count atomic tarzda oshadi
let handle = thread::spawn(move || {
    println!("spawned: {val2}");
});

println!("main: {val}");
handle.join().unwrap();
```

### `Arc<Mutex<T>>` — shared mutable state

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

`Arc::clone(&counter)` — reference count atomic oshadi. `Mutex<T>` ichki qiymatga xavfsiz kirish imkonini beradi.

### Thread pool receiver sharing

```rust
let (sender, receiver) = mpsc::channel::<Job>();
let receiver = Arc::new(Mutex::new(receiver));

for id in 0..size {
    workers.push(Worker::new(id, Arc::clone(&receiver)));
}
```

Bu yerda `Arc` bir nechta workerga bitta `Receiver<Job>` ownership'ini bo'lishishga yordam beradi. `Mutex` esa receiver'dan bir vaqtda faqat bitta worker o'qishini kafolatlaydi.

## `Rc<T>` vs `Arc<T>` taqqoslash

| | `Rc<T>` | `Arc<T>` |
|---|---|---|
| Thread-safety | Single-threaded | Multi-threaded |
| `Send` | ✗ | ✓ |
| `Sync` | ✗ | ✓ |
| Count operatsiyasi | Oddiy integer | Atomic |
| Performance | Tezroq | Biroz sekinroq |
| Import | `std::rc::Rc` | `std::sync::Arc` |

## Common Mistakes

- **`Arc<T>` bilan mutable kirish** — `Arc<T>` ichki qiymat *immutable*. Mutable kerak bo'lsa `Arc<Mutex<T>>` yoki `Arc<RwLock<T>>`.
- **`Rc<T>` bilan threadda ishlatishga urinish** — E0277: `Send` emas. `Arc<T>` ga almashtiring.
- **Performance uchun single-threaded kodda `Arc<T>`** — keraksiz overhead; `Rc<T>` ishlatish yetarli.

## Related Concepts

- [[mutex-t|Mutex<T>]] — `Arc<Mutex<T>>` eng keng tarqalgan kombinatsiya
- [[rc-t|Rc<T>]] — single-threaded analog
- [[send-trait|Send trait]] — `Arc<T>` Send implement qiladi
- [[sync-trait|Sync trait]] — `Arc<T>` Sync implement qiladi
- [[threads]]
- [[concurrency]]
- [[smart-pointers]]
- [[reference-counting]]
- [[thread-pool]]
- [[job-queue]]

## Sources

- [[16-3-shared-state-concurrency]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
