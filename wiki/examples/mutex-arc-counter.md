---
title: "Mutex<T> + Arc<T> counter"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, mutex, arc, threads]
source_count: 1
---

# Mutex<T> + Arc<T> counter

10 ta thread parallel ravishda bitta hisoblagichni oshiradi.

## Xato yondashuv 1: Mutex ni loop da move qilish (E0382)

```rust
use std::sync::Mutex;
use std::thread;

fn main() {
    let counter = Mutex::new(0);
    let mut handles = vec![];

    for _ in 0..10 {
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num += 1;
        }); // E0382: counter loop'ning birinchi iteratsiyasida move bo'lgan
        handles.push(handle);
    }
}
```

`counter` birinchi iteratsiyada thread'ga move bo'ladi — keyingilarda yo'q.

## Xato yondashuv 2: Rc<Mutex<T>> (E0277)

```rust
use std::rc::Rc;
use std::sync::Mutex;
use std::thread;

fn main() {
    let counter = Rc::new(Mutex::new(0));
    for _ in 0..10 {
        let counter = Rc::clone(&counter);
        thread::spawn(move || {
            // E0277: `Rc<Mutex<i32>>` cannot be sent between threads safely
            // `Rc<T>` does not implement `Send`
            *counter.lock().unwrap() += 1;
        });
    }
}
```

`Rc<T>` `Send` implement qilmaydi — reference count atomic emas.

## To'g'ri yondashuv: Arc<Mutex<T>> (Listing 16-15)

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter); // atomic reference count++
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap(); // MutexGuard oldi
            *num += 1;
        }); // MutexGuard drop → lock avtomatik qo'yildi
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap()); // 10
}
```

**Chiqish:** `Result: 10`

## Pattern xulosa

| Muammo | Yechim |
|--------|--------|
| Shared ownership | `Arc::clone()` |
| Mutable kirish | `mutex.lock()` |
| Lock qo'yish | Avtomatik (`MutexGuard` drop) |
| Thread-safety | `Arc<T>: Send + Sync` |

## Related

- [[mutex-t|Mutex<T>]]
- [[arc-t|Arc<T>]]
- [[threads]]
- [[16-3-shared-state-concurrency]]
