---
title: "Rust for Backend Developers: Ownership"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, ownership, chapter]
source_count: 1
---

# Rust for Backend Developers: Ownership

## Learning Goal

Ownership, move, borrowing, va borrow-checker qoidalari birga Rustning no-GC memory modelini qanday ishlatishini tushunish.

## Main Ideas

- Non-primitive owned values assignment va function call orqali move bo'ladi.
- Owner scope'dan chiqqanda cleanup/deterministic drop sodir bo'ladi.
- Borrowing ownershipni saqlab qoladi; real API'da `&str` ko'pincha `&String`dan yaxshiroq.
- Referential safety qoidasi: yo bitta `&mut T`, yo ko'p `&T`.
- `for n in arr` va `for n in &arr` ownership farqi muhim.

## Concepts

- [[ownership]]
- [[move-semantics|move semantics]]
- [[drop]]
- [[borrow-checker]]
- [[borrowing]]
- [[copy-trait|Copy trait]]

## Examples

```rust
let s1 = String::from("some string");
let s2 = s1;
```

```rust
fn len_of_string(s: &String) -> usize {
    s.len()
}
```

## Exercises

- `String` argument oladigan va `&str` argument oladigan functionlarni solishtiring.
- `for n in arr` va `for n in &arr` farqini `String` array bilan ko'rsating.

## Review Questions

- Nega move bo'lgandan keyin eski binding invalid bo'ladi?
- Borrow checker vector reallocation holatida nimani himoya qiladi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-ownership]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[ownership]]
- [[borrow-checker]]
