---
title: "Tuple Structs"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, structs, types]
source_count: 2
---

# Tuple Structs

## Short Definition

Tuple struct named struct type bo'lib, fields nomlanmaydi, faqat types beriladi.

## Why It Matters

Tuple struct bir xil field typelarga ega valuesni distinct domain typega aylantiradi. Masalan `Color(i32, i32, i32)` va `Point(i32, i32, i32)` Rust uchun boshqa-boshqa types.

## Mental Model

Regular tuple position-based. Tuple struct ham position-based, lekin type name qo'shib meaning va type safety beradi. Backend source RGB misoli bilan `.0`, `.1`, `.2` access va method qo'shish birga ishlashini ko'rsatadi.

## Syntax and Examples

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);

let Point(x, y, z) = origin;
```

## Common Mistakes

- Field types bir xil bo'lsa tuple structs interchangeable bo'ladi deb o'ylash.
- Destructuringda tuple struct type name kerakligini unutish.

## Related Concepts

- [[structs]]
- [[tuple]]
- [[domain-modeling|domain modeling]]
- [[type-checking|type checking]]

## Sources

- [[5-1-defining-and-instantiating-structs]]
- [[wiki/sources/rust-for-backend-developers-structs]]
