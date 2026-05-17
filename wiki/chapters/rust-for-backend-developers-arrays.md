---
title: "Rust for Backend Developers: Arrays"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, arrays, chapter]
source_count: 1
---

# Rust for Backend Developers: Arrays

## Learning Goal

`[T; N]` fixed-size modelini va qachon array, qachon `Vec<T>` ishlatishni ajratish.

## Main Ideas

- Array length compile-time'da ma'lum bo'ladi.
- Type ichiga element type bilan birga length ham kiradi.
- Arraylar `Debug` bilan oson chiqariladi, lekin `Display` default emas.

## Concepts

- [[array]]
- [[debug-formatting|Debug formatting]]
- [[vector|Vec<T>]]
- [[slices]]

## Examples

```rust
let arr: [u8; 4] = [192, 168, 0, 1];
println!("{arr:?}");
```

## Exercises

- IPv4 address uchun array, request body bytes uchun `Vec<u8>` tanlang va sababini yozing.
- Bir xil elementlarga ega, lekin turli uzunlikdagi arraylar nega turli type ekanini ko'rsating.

## Review Questions

- Array qaysi holatda vector'dan yaxshi?
- Nima uchun runtime size arrayning core modeliga zid?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-arrays]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[array]]
- [[vector|Vec<T>]]
