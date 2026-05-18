---
title: "thread::Builder"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, threads, builder, concurrency]
source_count: 1
---

# thread::Builder

## Short Definition

`std::thread::Builder` yangi thread yaratishda default `spawn` bermaydigan sozlamalarni beruvchi builder.

## Why It Matters

Thread name debugging uchun, stack size esa deep recursion yoki katta stack frame'lar uchun kerak bo'lishi mumkin.

## Mental Model

`thread::spawn` bu shorthand. Maxsus operational detail kerak bo'lsa builder ishlatiladi.

## Syntax and Examples

```rust
let handle = std::thread::Builder::new()
    .name("worker-1".to_string())
    .stack_size(8 * 1024)
    .spawn(|| println!("{:?}", std::thread::current().name()))
    .unwrap();
```

## Common Mistakes

- `thread::spawn` har doim yetadi deb o'ylash.
- Stack size'ni performance tuning emas, muammo bo'lsa kerakli parametr sifatida ko'rmaslik.

## Related Concepts

- [[threads]]
- [[join-handle]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multithreading]]
