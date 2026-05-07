---
title: "vec! macro"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, collections, macros]
source_count: 1
---

# vec! macro

## Short Definition

`vec!` macro initial valuesdan yangi [[vector|Vec<T>]] yaratadi.

## Why It Matters

`vec!` boilerplateni kamaytiradi va compilerga element type'ini initial valuesdan infer qilishga yordam beradi.

## Mental Model

`vec![1, 2, 3]` "shu elementlar bilan vector yarat" degani. Elementlar bir xil concrete type'ga mos bo'lishi kerak.

## Syntax and Examples

```rust
let v = vec![1, 2, 3];
```

Bu odatda `Vec<i32>` sifatida inferred bo'ladi, chunki integer default type'i `i32`.

## Common Mistakes

- `vec!`ni type emas deb unutish; u macro, natijasi `Vec<T>`.
- Bir `vec![]` ichiga mos kelmaydigan har xil typelarni joylashtirish.

## Related Concepts

- [[vector|Vec<T>]]
- [[macro]]
- [[type-inference|type inference]]
- [[collections]]

## Sources

- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
