---
title: "Arc<T>"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-18
tags: [rust, concurrency, threads, arc, reference-counting, smart-pointers]
source_count: 5
---

# Arc<T>

## Short Definition

`Arc<T>` — *Atomically Reference Counted*. `Rc<T>` bilan bir xil API, lekin reference counting **atomik operatsiyalar** orqali bajariladi — threadlar orasida xavfsiz. `std::sync::Arc`.

## Why It Matters

`Rc<T>` single-threaded: reference count oddiy integer, atomic emas. `Arc<T>` esa count'ni atomic tarzda boshqaradi — `Send + Sync` implement qiladi.

Nima uchun `Arc<T>` standart emas? **Performance**: atomic operatsiyalar oddiy integer operatsiyalardan sekinroq.

## Mental Model

`Rc<T>` = oddiy hisoblagich. `Arc<T>` = atom bilan himoyalangan hisoblagich. API bir xil, lekin `Arc` thread-safe bo'lgani uchun biroz qimmatroq.

Muhim practical nuance: agar thread'lar parent scope'dan chiqmaydigan qisqa ish bo'lsa, [[scoped-threads]] sabab `Arc<T>` umuman kerak bo'lmasligi mumkin.

Global session registry kabi pattern'da `Arc<Mutex<UserSession>>` yoki `Arc<RwLock<T>>` object-level sharing beradi: tashqi registry lock tez bo'shatiladi, keyin individual object lock alohida olinadi.

## Syntax and Examples

```rust
use std::sync::Arc;

let val = Arc::new(String::from("hello"));
let val2 = Arc::clone(&val);
let handle = std::thread::spawn(move || {
    println!("spawned: {val2}");
});
handle.join().unwrap();
```

```rust
use std::sync::{Arc, Mutex};

let counter = Arc::new(Mutex::new(0));
```

## `Rc<T>` vs `Arc<T>` taqqoslash

| | `Rc<T>` | `Arc<T>` |
|---|---|---|
| Thread-safety | Single-threaded | Multi-threaded |
| `Send` | ✗ | ✓ |
| `Sync` | ✗ | ✓ |
| Count operatsiyasi | Oddiy integer | Atomic |
| Performance | Tezroq | Biroz sekinroq |

## Common Mistakes

- **`Arc<T>` bilan mutable kirish** — mutable kerak bo'lsa `Arc<Mutex<T>>` yoki `Arc<RwLock<T>>`.
- **`Rc<T>` bilan threadda ishlatishga urinish**.
- **Single-threaded kodda odat bo'yicha `Arc<T>` ishlatish**.

## Related Concepts

- [[mutex-t|Mutex<T>]]
- [[rc-t|Rc<T>]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[threads]]
- [[concurrency]]
- [[smart-pointers]]
- [[reference-counting]]
- [[thread-pool]]
- [[job-queue]]
- [[scoped-threads]]
- [[global-state]]

## Sources

- [[16-3-shared-state-concurrency]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
- [[wiki/sources/rust-for-backend-developers-multithreading]]
- [[wiki/sources/rust-for-backend-developers-global-data]]
