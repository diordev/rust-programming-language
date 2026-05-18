---
title: "Condvar"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, concurrency, sync, condition-variable]
source_count: 1
---

# Condvar

## Short Definition

`std::sync::Condvar` bir thread'ni kutish holatiga qo'yib, boshqa thread signal berganda uyg'otadigan primitiv.

## Why It Matters

Busy-wait yoki polling o'rniga state change'ni samarali kutish mumkin.

## Mental Model

Thread mutex bilan flag'ni tekshiradi, tayyor bo'lmasa uxlaydi, boshqa thread flag'ni o'zgartirib notify qiladi.

## Syntax and Examples

```rust
let pair = std::sync::Arc::new((std::sync::Mutex::new(false), std::sync::Condvar::new()));
```

`wait(guard)` guard'ni consume qiladi, mutex'ni bo'shatadi, keyin uyg'ongach yangi guard bilan qaytadi.

## Common Mistakes

- `wait` mutex'ni ushlab turadi deb o'ylash.
- `while` o'rniga bitta `if` bilan predicate tekshirish.

## Related Concepts

- [[mutex-t|Mutex<T>]]
- [[threads]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multithreading]]
