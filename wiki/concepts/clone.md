---
title: "clone"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, ownership]
source_count: 1
---

# clone

## Short Definition

`clone()` value'ning explicit copy'sini yaratadigan method. Heap-backed typelarda bu deep copy bo'lishi va costly bo'lishi mumkin.

## Why It Matters

Rust automatic deep copy qilmaydi. Agar `String` kabi value'ning mustaqil copy'si kerak bo'lsa, `clone()` bilan intent va possible cost aniq ko'rsatiladi.

## Mental Model

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {s1}, s2 = {s2}");
```

Bu yerda `s1` ham, `s2` ham valid qoladi.

## Syntax and Examples

```rust
let copied_heap_data = original.clone();
```

## Common Mistakes

- Move kerak bo'lgan joyda odat bo'yicha `clone()` ishlatish.
- `clone()`ni free operation deb o'ylash.
- `Copy` typelarda `clone()`ni ownership mental modelini tushunmasdan ishlatish.

## Related Concepts

- [[move-vs-clone]]
- [[move-semantics|move semantics]]
- [[copy-trait|Copy trait]]
- [[string-type|String]]

## Sources

- [[4-1-what-is-ownership-the-rust-programming-language]]
