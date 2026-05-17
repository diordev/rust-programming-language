---
title: "Reference Counting"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, memory, ownership]
source_count: 2
---

# Reference Counting

## Short Definition

Bir xil heap allokatsiya nechta egasi (owner) borligini hisoblab boruvchi memory boshqaruv mexanizmi. Owner soni `1` dan `0` ga tushganda allokatsiya avtomatik bo'shatiladi.

## Why It Matters

Rust ownership modeli har qiymatga **bitta** owner talab qiladi. Lekin ba'zi data strukturalarida (graph, doubly-linked list, shared cache) bir xil qiymatga bir nechta egalik kerak — kompilyatsiya vaqtida qaysi biri eng oxirgi tugashi noma'lum. Reference counting buni runtime'da hisoblash orqali hal qiladi.

`drop()` har safar bittadan kamaytiradi; oxirgi `drop` aslida heap allokatsiyani bo'shatadi.

## Mental Model

Tasavvur qiling: kutubxonadagi kitobni nechta o'quvchi olib turibdi. Har kim olganda hisob `+1`, qaytarganda `-1`. Hisob `0` bo'lsa — kitob arxivga qaytariladi.

```
strong_count = 3   →  3 ta owner
strong_count = 2   →  bittasi drop bo'ldi
strong_count = 1   →  yana bittasi
strong_count = 0   →  heap bo'shatiladi
```

## Rustda Ikki Implementatsiya

| Tur | Thread-safety | Performance | Foydalanish |
|-----|---------------|-------------|-------------|
| [[rc-t\|Rc<T>]] | Single-threaded only | Tezroq (atomik bo'lmagan counter) | Bitta thread ichida shared ownership |
| [[arc-t\|Arc<T>]] | Thread-safe | Sekinroq (atomik counter) | Multi-thread ichida shared ownership |

`Rc<T>` `Send` va `Sync` emas — kompilyator uni boshqa thread'ga uzatishga ruxsat bermaydi.

## Syntax and Examples

```rust
use std::rc::Rc;

let a = Rc::new(5);
println!("count = {}", Rc::strong_count(&a)); // 1

let b = Rc::clone(&a);
println!("count = {}", Rc::strong_count(&a)); // 2

{
    let c = Rc::clone(&a);
    println!("count = {}", Rc::strong_count(&a)); // 3
} // c drop bo'ladi

println!("count = {}", Rc::strong_count(&a)); // 2
```

## Common Mistakes

**1. Rc<T>'ni thread'ga uzatish.** Compile xato (`Rc<T> cannot be sent between threads`). [[arc-t|Arc<T>]] ishlatish kerak.

**2. Reference cycle.** Ikki `Rc<T>` bir-biriga `Rc<RefCell<...>>` orqali murojaat qilsa, count hech qachon `0` ga tushmaydi → memory leak. Yechim: aylanani uzish uchun [[weak-t|Weak<T>]] ishlatish.

**3. Mutability.** `Rc<T>` faqat shared (immutable) reference beradi. Mutable kerak bo'lsa: `Rc<RefCell<T>>` (single-thread) yoki `Arc<Mutex<T>>` (multi-thread).

## Related Concepts

- [[rc-t|Rc<T>]] — single-threaded reference-counted smart pointer
- [[arc-t|Arc<T>]] — atomic, thread-safe variant
- [[weak-t|Weak<T>]] — referansga ega, lekin count'ga hisoblanmaydi (cycle break)
- [[ownership]] — Rust'ning asosiy modeli
- [[smart-pointers]] — umumiy oila
- [[memory-leak]] — reference cycle natijasi
- [[drop]] — count kamayganda chaqiriladi

## Sources

- [[15-4-rc-the-reference-counted-smart-pointer|15.4 Rc<T>]]
- [[16-3-shared-state-concurrency|16.3 Shared-State Concurrency]]
