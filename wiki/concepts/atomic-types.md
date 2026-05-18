---
title: "Atomic Types"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, concurrency, atomics]
source_count: 1
---

# Atomic Types

## Short Definition

`std::sync::atomic` ichidagi `AtomicBool`, `AtomicI32`, `AtomicUsize`, `AtomicPtr<T>` kabi typelar lock'siz atomik read/write va read-modify-write operatsiyalarini beradi.

## Why It Matters

Counter, flag, state bitmask, yoki pointer publish qilish kabi tor state uchun `Mutex<T>`dan yengilroq variant bo'lishi mumkin.

## Mental Model

Bir xotira katagi ustidagi operatsiya "uzilib ketmaydigan" bo'ladi. Lekin bu butun algoritm avtomatik to'g'ri degani emas.

## Syntax and Examples

```rust
use std::sync::atomic::{AtomicI32, Ordering};

let a = AtomicI32::new(0);
a.fetch_add(1, Ordering::Relaxed);
assert_eq!(a.load(Ordering::Relaxed), 1);
```

## Common Mistakes

- Atomik bo'lsa ham multi-variable invariant avtomatik saqlanadi deb o'ylash.
- Ordering'ni default detail deb e'tiborsiz qoldirish.

## Related Concepts

- [[atomic-memory-ordering]]
- [[compare-exchange]]
- [[data-race]]
- [[mutable-static]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multithreading]]
