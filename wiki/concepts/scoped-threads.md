---
title: "Scoped Threads"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, threads, lifetimes, concurrency]
source_count: 1
---

# Scoped Threads

## Short Definition

`std::thread::scope` ichida yaratilgan thread'lar parent scope tugashidan oldin majburiy tugaydi va local qiymatlarni reference bilan borrow qila oladi.

## Why It Matters

`Arc` va `move` bilan ownership ko'chirmasdan, qisqa umrli parallel ishlarni sodda yozish mumkin.

## Mental Model

Ordinary `spawn` "detached until joined". Scoped thread esa "shu block ichida yashashi shart" modelida.

## Syntax and Examples

```rust
let s = String::from("Hello");

std::thread::scope(|scope| {
    scope.spawn(|| println!("{s}"));
});
```

## Common Mistakes

- `.join()` bor bo'lsa ordinary `spawn` ham local borrow'ni qabul qiladi deb o'ylash.
- Scoped thread'ni background task uchun ishlatishga urinish.

## Related Concepts

- [[threads]]
- [[lifetimes]]
- [[join-handle]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multithreading]]
