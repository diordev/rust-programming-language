---
title: "Sync trait"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, concurrency, threads, traits, marker, unsafe]
source_count: 5
---

# Sync trait

## Short Definition

`Sync` — `std::marker` marker trait. Type'ga bir nechta thread **immutable referens** (`&T`) orqali xavfsiz kirishi mumkinligini ko'rsatadi. Munosabat: `T: Sync` ↔ `&T: Send`.

## Why It Matters

Shared-state concurrency uchun: bir nechta thread bir xil qiymatga bir vaqtda o'qiy oladi — ammo bu xavfsiz faqat type `Sync` bo'lsa. Rust bu kafolatni compile time'da tekshiradi.

## Mental Model

"Bu qiymatga bir nechta thread bir vaqtda ko'z tashlay oladi" degan kafolat. `Mutex<T>` `Sync` — ichki qiymatga birin-ketin lock orqali kiradi. `RefCell<T>` `Sync` emas — runtime borrow checker thread-safe emas.

Common-traits source bu traitni ham preview qiladi: `Sync`ning to'liq amaliy oqibatlari multithreading qismida ochiladi, lekin marker contractni oldindan bilish foydali.

## Kimlar `Sync`

- **Deyarli barcha primitive type'lar**: `i32`, `f64`, `bool`, va hokazo.
- **`Arc<T>`** (T: Send + Sync bo'lsa).
- **`Mutex<T>`** (T: Send bo'lsa) — `lock()` orqali mutual exclusion kafolati.
- `Sync` type'lardan tashkil topgan type'lar **avtomatik** `Sync`.

## Kimlar `Sync` emas

- **`Rc<T>`** — reference count atomic emas; xuddi `Send` emas sababi bilan.
- **`RefCell<T>`** va `Cell<T>` oilasi — runtime borrow checking thread-safe emas.
- **`UnsafeCell<T>`** — interior mutability'ning asosi; `Sync` emas.

## Syntax and Examples

```rust
use std::sync::{Arc, Mutex};
use std::thread;

// Arc<Mutex<T>>: T: Send → Mutex<T>: Send + Sync → Arc<Mutex<T>>: Send + Sync
let shared = Arc::new(Mutex::new(0));
let shared2 = Arc::clone(&shared);

let handle = thread::spawn(move || {
    *shared2.lock().unwrap() += 1;
});
handle.join().unwrap();
```

## `Send` vs `Sync`

| Trait | Kafolat | Xavfsiz bo'lmagan misol |
|-------|---------|------------------------|
| `Send` | Ownership o'tkazish | `Rc<T>` (count atomic emas) |
| `Sync` | `&T` orqali kirish | `RefCell<T>` (borrow check thread-safe emas) |

Munosabat: `T: Sync` ↔ `&T: Send`

## Manual Implementation

Hech qanday metodsiz marker trait. Composite type'lar avtomatik `Sync`. Qo'lda implement — **unsafe Rust** talab qiladi. Backend traits source bu patternni `unsafe trait`ning klassik motivatsiyasi sifatida keltiradi: xavf method syntaxida emas, implementor invariantlarida.

```rust
struct MyType { ptr: *mut u8 }
unsafe impl Sync for MyType {}   // dasturchi va'da beradi
```

[[unsafe-trait|Unsafe trait]] implementatsiyasining klassik misoli.

## Related Concepts

- [[send-trait|Send trait]]
- [[threads]]
- [[concurrency]]
- [[arc-t|Arc<T>]] — `Send + Sync`
- [[rc-t|Rc<T>]] — `Sync` emas
- [[mutex-t|Mutex<T>]] — `Sync`
- [[refcell-t|RefCell<T>]] — `Sync` emas
- [[interior-mutability]]
- [[unsafe-rust]]
- [[unsafe-trait|unsafe trait]]
- [[raw-pointer|raw pointer]]

## Sources

- [[wiki/sources/16-fearless-concurrency]]
- [[16-4-extensible-concurrency-with-send-and-sync]]
- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/rust-for-backend-developers-traits]]
- [[wiki/sources/rust-for-backend-developers-common-traits]]
