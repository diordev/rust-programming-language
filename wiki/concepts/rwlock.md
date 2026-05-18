---
title: "RwLock"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, concurrency, sync, lock]
source_count: 2
---

# RwLock

## Short Definition

`std::sync::RwLock<T>` bir vaqtning o'zida ko'p reader yoki bitta writer beradigan lock.

## Why It Matters

Read-heavy workload'da `Mutex<T>`dan ko'ra yaxshi parallelism berishi mumkin.

Global session registry kabi workload'da map ko'p o'qilib, kam yozilsa `RwLock<HashMap<...>>` tabiiy tanlov bo'lishi mumkin.

## Mental Model

Eshik ikki holatda bo'ladi: ko'p kishi faqat ko'rishi mumkin yoki bitta kishi ichkarida narsani o'zgartiradi.

## Syntax and Examples

```rust
let lock = std::sync::RwLock::new(5);
let read = lock.read().unwrap();
drop(read);
let mut write = lock.write().unwrap();
*write = 10;
```

## Common Mistakes

- Har doim `Mutex<T>`dan yaxshiroq deb o'ylash.
- Writer starvation va poison semanticsni e'tiborsiz qoldirish.
- Ichki value'ni olib bo'lgach lock scope'ini qisqartirmaslik.

## Related Concepts

- [[mutex-t|Mutex<T>]]
- [[arc-t|Arc<T>]]
- [[poisoned-mutex]]
- [[global-state]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multithreading]]
- [[wiki/sources/rust-for-backend-developers-global-data]]
