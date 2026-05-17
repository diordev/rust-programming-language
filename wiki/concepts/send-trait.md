---
title: "Send trait"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, concurrency, threads, traits, marker, unsafe]
source_count: 5
---

# Send trait

## Short Definition

`Send` — `std::marker` marker trait. Type'ning **ownership**'ini threadlar orasida xavfsiz o'tkazish mumkinligini ko'rsatadi. `thread::spawn` closuresi `Send` bound talab qiladi.

## Why It Matters

Rust `Send`ni compile time'da tekshiradi: agar type `Send` implement qilmasa, uni `thread::spawn`ga bera olmaysiz. Bu thread-safety'ni runtime xato o'rniga kompilyatsiya vaqtida kafolatlaydi.

## Mental Model

"Bu qiymatni boshqa threadga topshirish xavfsiz" degan kafolat. `Rc<T>` xavfli — reference count atomic emas, ikki thread bir vaqtda o'zgartirishi mumkin. `Arc<T>` xavfsiz — atomic. Rust shu farqni `Send` trait orqali ifodalaydi.

## Kimlar `Send`

- **Deyarli barcha primitive type'lar**: `i32`, `f64`, `bool`, `String`, `Vec<T>` (T: Send bo'lsa), va hokazo.
- **`Arc<T>`** (T: Send + Sync bo'lsa) — `Rc<T>`dan farqli.
- **`Mutex<T>`** (T: Send bo'lsa).
- `Send` type'lardan tashkil topgan type'lar **avtomatik** `Send`.

## Kimlar `Send` emas

- **`Rc<T>`** — reference count atomic emas; `thread::spawn`ga berish kompilyatsiya xatosi.
- **Raw pointerlar** (`*const T`, `*mut T`) — ch20 da ko'riladi.

## Syntax and Examples

```rust
use std::thread;

let x = 5i32; // i32: Send
let handle = thread::spawn(move || println!("{x}"));
handle.join().unwrap();
```

```rust
type Job = Box<dyn FnOnce() + Send + 'static>;
```

Thread pool job'i `Send` bo'lishi shart, chunki closure `execute` chaqirilgan threaddan worker threadga uzatiladi.

```rust
use std::rc::Rc;
use std::thread;

let rc = Rc::new(5);
// XATO: `Rc<i32>` cannot be sent between threads safely
// let handle = thread::spawn(move || println!("{rc}"));
```

## `Send` vs `Sync`

| Trait | Kafolat |
|-------|---------|
| `Send` | Ownership o'tkazish xavfsiz |
| `Sync` | Immutable reference (`&T`) orqali kirish xavfsiz |
| Munosabat | `T: Sync` ↔ `&T: Send` |

## Manual Implementation

`Send` — hech qanday metodsiz marker trait. `Send` type'lardan tashkil topgan composite type'lar avtomatik `Send`. Qo'lda implement talab qilsa — **unsafe Rust** (noto'g'ri implement thread-safety kafolatlarini buzadi). Backend traits source bu nuqtani juda foydali ko'rsatadi: `unsafe trait` bo'lishi uchun trait metodlari unsafe bo'lishi shart emas.

```rust
struct MyType { ptr: *mut u8 }   // *mut u8: !Send

unsafe impl Send for MyType {}   // dasturchi va'da beradi
```

Bu [[unsafe-trait|unsafe trait]] implementatsiyasining klassik misoli.

## Related Concepts

- [[sync-trait|Sync trait]]
- [[threads]]
- [[concurrency]]
- [[arc-t|Arc<T>]] — `Send + Sync`
- [[rc-t|Rc<T>]] — `Send` emas
- [[mutex-t|Mutex<T>]] — `Send`
- [[move-closures-threads]]
- [[thread-pool]]
- [[unsafe-rust]]
- [[unsafe-trait|unsafe trait]] — `Send` ni qo'lda implement qilish
- [[raw-pointer|raw pointer]] — `*const`/`*mut` ni `!Send`

## Sources

- [[wiki/sources/16-fearless-concurrency]]
- [[16-4-extensible-concurrency-with-send-and-sync]]
- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/rust-for-backend-developers-traits]]
