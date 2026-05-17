---
title: "Copy Trait"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, ownership, trait]
source_count: 5
---

# Copy Trait

## Short Definition

`Copy` trait simple value'lar assignmentda move emas, implicit copy bo'lishini bildiradigan [[marker-trait|marker trait]].

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

Appendix C nuqtai nazaridan, `Copy` derive bo'lishi uchun type'ning barcha qismlari `Copy` bo'lishi kerak. Yana muhim shart: `Copy` type albatta [[clone]]ni ham implement qiladi.

Source materiallarda ba'zan "compiler `clone()` chaqiradi" degan simplification uchraydi. Durable model uchun bunu literal method call deb emas, implicit copy semantics deb tushunish to'g'riroq.

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

Derived copy:

```rust
#[derive(Copy, Clone)]
struct Point {
    x: i32,
    y: i32,
}
```

## Common Mistakes

- `String` `Copy` deb o'ylash.
- `Drop` implement qilgan type `Copy` ham bo'lishi mumkin deb o'ylash.
- Copy va clone'ni bir xil signal deb o'ylash; `clone()` explicit va potentially expensive.
- Struct update syntaxdagi hamma fieldlar move bo'ladi deb o'ylash; `Copy` fields copied bo'ladi.
- Hash map insertidagi hamma value move bo'ladi deb o'ylash; `Copy` values copied bo'ladi.
- `Copy`ni `Clone`siz derive qilish mumkin deb o'ylash.
- `Copy` marker trait ekanini unutib, undan alohida behavior methodi kutish.

## Related Concepts

- [[ownership]]
- [[move-semantics|move semantics]]
- [[clone]]
- [[drop]]
- [[marker-trait|marker trait]]
- [[traits]]
- [[struct-update-syntax]]
- [[structs]]
- [[hash-map|HashMap]]
- [[derive-attribute]]

## Sources

- [[4-1-what-is-ownership]]
- [[5-1-defining-and-instantiating-structs]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[wiki/sources/rust-for-backend-developers-auto-derive-traits]]
- [[wiki/sources/22-3-c-derivable-traits|22.3]]
