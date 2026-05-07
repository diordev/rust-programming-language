---
title: "Copy Trait"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, ownership, trait]
source_count: 3
---

# Copy Trait

## Short Definition

`Copy` trait simple stack-only typelar assignmentda move emas, trivial copy bo'lishini bildiradi.

## Why It Matters

`i32`, `bool`, `char` kabi values assignment yoki function call'dan keyin ham valid qoladi. Bu ownership mental modelida `String` kabi move typesdan farq qiladi.

## Mental Model

```rust
let x = 5;
let y = x;
println!("x = {x}, y = {y}");
```

`x` valid qoladi, chunki integer `Copy`.

Struct update syntaxda `Copy` fields eski instance'dan yangi instance'ga copy qilinadi. Masalan `bool` va `u64` fields moved emas, copied bo'ladi.

[[hash-map|HashMap]] insertida ham shu farq ko'rinadi: `String` key/value mapga move bo'ladi, `i32` kabi score values esa copied bo'ladi.

## Syntax and Examples

Common Copy types:

- integer types, masalan `u32`;
- `bool`;
- floating-point types, masalan `f64`;
- `char`;
- faqat `Copy` elementlardan iborat tuples, masalan `(i32, i32)`.

Hash map example:

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
```

Bu yerda `"Blue"`dan yaratilgan `String` mapga move bo'ladi; `10` esa `i32` bo'lgani uchun copy qilinadi.

## Common Mistakes

- `String` `Copy` deb o'ylash.
- `Drop` implement qilgan type `Copy` ham bo'lishi mumkin deb o'ylash.
- Copy va clone'ni bir xil signal deb o'ylash; `clone()` explicit va potentially expensive.
- Struct update syntaxdagi hamma fieldlar move bo'ladi deb o'ylash; `Copy` fields copied bo'ladi.
- Hash map insertidagi hamma value move bo'ladi deb o'ylash; `Copy` values copied bo'ladi.

## Related Concepts

- [[ownership]]
- [[move-semantics|move semantics]]
- [[clone]]
- [[drop]]
- [[traits]]
- [[struct-update-syntax]]
- [[structs]]
- [[hash-map|HashMap]]

## Sources

- [[4-1-what-is-ownership-the-rust-programming-language]]
- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language]]
