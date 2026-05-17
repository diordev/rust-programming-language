---
title: "Массивы - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, arrays]
source_count: 1
---

# Массивы - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/13. Массивы.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/arrays.html

## Detailed Summary

Bu source Rust arraylarini fixed-size contiguous sequence sifatida beradi. Type shakli `[T; N]`: element type va elementlar soni birga type'ning o'ziga kiradi.

Asosiy cheklov compile-time size. Array uzunligini runtime'da inputdan olib belgilab bo'lmaydi. Shu sabab arraylar general dynamic list uchun emas, shape'i aniq bo'lgan ma'lumotlar uchun foydali. Source IPv4 address misolini beradi: `[u8; 4]`.

Indexing noldan boshlanadi. Mutation uchun array binding `mut` bo'lishi kerak. Printingda muhim detail bor: array `Display` emas, shuning uchun `println!("{arr:?}")` kabi `Debug` formatting ishlatiladi.

Source "function ichida e'lon qilingan array stackda bo'ladi" degan sodda mental modelni ham beradi. Wiki buni ehtiyotkor yozadi: local fixed-size array odatda local stack frame bilan bog'liq deb o'ylash mumkin, lekin umumiy storage placement contextga bog'liq bo'lishi mumkin.

## Key Concepts

- [[array]]
- [[scalar-types|scalar types]]
- [[debug-formatting|Debug formatting]]
- [[display-formatting|Display formatting]]
- [[mutable-reference|mutable reference]]
- [[vector|Vec<T>]]

## Code Examples

```rust
let arr: [i32; 3] = [1, 2, 3];
```

```rust
let arr: [u8; 4] = [192, 168, 0, 1];
println!("Array is {arr:?}");
println!("{}", arr[0]);
```

```rust
let mut arr = [1, 2, 3];
arr[1] = 55;
println!("Array is {arr:?}");
```

## Exercises or Practice Ideas

- `[u8; 4]` bilan IPv4 address, `Vec<u8>` bilan variable-length payload uchun ikki model yozing.
- Bir xil elementlar va turli uzunlikdagi ikki array nima uchun turli typelar ekanini ko'rsating.
- Arrayni `{:?}` va `{}` bilan chiqarishga urinish farqini tekshiring.

## Questions Raised

- Qachon array o'rniga `Vec<T>` yoki slice parameter yaxshiroq?
- Fixed-size type signature API design'ni qayerda soddalashtiradi?
- Local array storage haqidagi stack mental modeli qaysi joygacha foydali?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-arrays]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[array]]
- [[vector|Vec<T>]]
- [[slices]]
