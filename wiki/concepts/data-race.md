---
title: "Data Race"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, concurrency, memory-safety]
source_count: 4
---

# Data Race

## Short Definition

Data race bir data'ga bir vaqtning o'zida bir nechta pointer access qilganda, kamida bittasi write qilganda, va synchronization yo'q bo'lganda yuz beradi.

## Why It Matters

Data races undefined behaviorga olib keladi. Rust borrowing rules bir vaqtda aliasing va mutationni cheklab, data racesni compile time'da oldini oladi.

Safe Rustning asosiy kafolatlaridan biri - unsynchronized shared mutable accessni oddiy application codeida compile time'da cheklash.

## Mental Model

Rust rule:

- bitta mutable reference;
- yoki ko'p immutable references;
- lekin ikkalasi bir vaqtda emas.

## Syntax and Examples

Invalid:

```rust
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;
```

## Common Mistakes

- Multiple mutable aliases xavfsiz deb o'ylash.
- Immutable readers active bo'lgan paytda writer olishga urinish.

## Thread kontekstida data race

Threadlar orasida synchronization vositasisiz (masalan, `Mutex<T>` ishlatmasdan) shared data'ga bir vaqtda yozish — klassik data race. Rust bu holatni `Send` va `Sync` traitlari orqali compile time'da oldini oladi:

```rust
// Rc<T> Send emas → threadlar orasida o'tkazib bo'lmaydi
// Arc<T> Send + Sync → thread-safe reference counting
use std::sync::Arc;
let shared = Arc::new(5);
```

`Mutex<T>` esa shared mutable state'ga xavfsiz kirish uchun ishlatiladi (ch16.3).

Unsafe raw pointerlar va qo'lda qurilgan aliasing bu compile-time himoyani aylanib o'tishi mumkin. O'sha nuqtadan keyin data race xavfi to'liq author zimmasida.

## Related Concepts

- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[e0499-multiple-mutable-borrows|E0499 multiple mutable borrows]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
- [[threads]]
- [[send-trait|Send trait]]
- [[concurrency]]
- [[memory-safety|memory safety]]
- [[unsafe-rust|unsafe Rust]]

## Sources

- [[4-2-references-and-borrowing]]
- [[16-1-using-threads-to-run-code-simultaneously]]
- [[wiki/sources/rust-for-backend-developers-safe-rust]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
