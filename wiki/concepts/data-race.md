---
title: "Data Race"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-18
tags: [rust, concurrency, memory-safety]
source_count: 5
---

# Data Race

## Short Definition

Data race bir data'ga bir vaqtning o'zida bir nechta pointer access qilganda, kamida bittasi write qilganda, va synchronization yo'q bo'lganda yuz beradi.

## Why It Matters

Data races undefined behaviorga olib keladi. Rust borrowing rules bir vaqtda aliasing va mutationni cheklab, data racesni compile time'da oldini oladi.

## Mental Model

Rust rule:

- bitta mutable reference;
- yoki ko'p immutable references;
- lekin ikkalasi bir vaqtda emas.

## Syntax and Examples

```rust
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;
```

## Common Mistakes

- Multiple mutable aliases xavfsiz deb o'ylash.
- Immutable readers active bo'lgan paytda writer olishga urinish.

## Thread kontekstida data race

Threadlar orasida synchronization vositasisiz shared data'ga bir vaqtda yozish — klassik data race. Rust bu holatni `Send` va `Sync` traitlari orqali compile time'da oldini oladi:

```rust
use std::sync::Arc;
let shared = Arc::new(5);
```

`Mutex<T>` esa shared mutable state'ga xavfsiz kirish uchun ishlatiladi.

Atomik typelar ham bitta location ustidagi unsynchronized read/write muammosini hal qiladi. Lekin bu butun algoritm to'g'ri degani emas: bir nechta atomik orasidagi visibility/order muammosi uchun [[atomic-memory-ordering]] hali ham muhim.

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
- [[atomic-types]]
- [[atomic-memory-ordering]]

## Sources

- [[4-2-references-and-borrowing]]
- [[16-1-using-threads-to-run-code-simultaneously]]
- [[wiki/sources/rust-for-backend-developers-safe-rust]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
- [[wiki/sources/rust-for-backend-developers-multithreading]]
