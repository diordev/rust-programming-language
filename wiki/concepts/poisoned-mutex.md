---
title: "Poisoned Mutex"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, concurrency, mutex, poison]
source_count: 1
---

# Poisoned Mutex

## Short Definition

Lock ushlab turgan thread panic qilsa, `Mutex<T>` poisoned deb belgilanadi va keyingi `.lock()` `PoisonError` qaytaradi.

## Why It Matters

Bu state ehtimol yarim yangilangan bo'lishi mumkinligini signal qiladi.

## Mental Model

Rust "endi bu qiymatni ishlatma" demaydi; "endi ehtiyot bo'l, qaror sendan" deydi.

## Syntax and Examples

```rust
let attempt = mutex.lock();
if let Err(err) = attempt {
    let guard = err.into_inner();
    drop(guard);
    mutex.clear_poison();
}
```

## Common Mistakes

- Poison bo'lsa value'ni umuman olib bo'lmaydi deb o'ylash.
- `unwrap()` bilan signalni ko'rmay yurish.

## Related Concepts

- [[mutex-t|Mutex<T>]]
- [[threads]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multithreading]]
