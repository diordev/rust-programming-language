---
title: "Stack and Heap"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-08
tags: [rust, memory, heap, box]
source_count: 3
---

# Stack and Heap

## Short Definition

Stack va heap runtime memoryning ikki qismi. Stack fixed-size data'ni LIFO tartibda saqlaydi; heap dynamic size yoki runtime allocation kerak bo'lgan data uchun ishlatiladi.

## Why It Matters

Ownership asosan heap data'ni kim ishlatayotgani va qachon cleanup qilinishini boshqaradi. Stack/heap farqi `String`, move, clone, va Copy mental modelini tushuntiradi.

## Mental Model

- Stack: known fixed-size values, push/pop amallari juda tez.
- Heap: allocator joy qidiradi, pointer qaytaradi, access pointer orqali.
- Pointer stackda turishi mumkin, pointed data heapda turadi.

## Syntax and Examples

```rust
let x = 5; // stack-only integer
let s = String::from("hello"); // stack metadata + heap text data
```

`String` stackda pointer, length, capacity saqlaydi; actual text heapda turadi.

Common collections, masalan [[vector|Vec<T>]] va [[string-type|String]], runtime'da grow/shrink qilishi uchun heap-backed data ishlatadi.

[[box-t|Box<T>]] ham value'ni heapda saqlaydi, stackda esa fixed-size pointer metadata turadi:

```rust
let b = Box::new(5);
```

## Common Mistakes

- `String` assignment heap data'ni automatic copy qiladi deb o'ylash.
- Heap allocation har doim free qilinishi kerakligini unutish.
- Stack-only Copy types bilan heap-backed move typesni bir xil deb o'ylash.

## Related Concepts

- [[ownership]]
- [[string-type|String]]
- [[collections]]
- [[vector|Vec<T>]]
- [[box-t|Box<T>]]
- [[smart-pointers]]
- [[move-semantics|move semantics]]
- [[copy-trait|Copy trait]]
- [[drop]]

## Sources

- [[4-1-what-is-ownership]]
- [[wiki/sources/8-common-collections]]
- [[15-1-using-box-t-to-point-to-data-on-the-heap]]
