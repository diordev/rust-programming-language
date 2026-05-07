---
title: "Compound Types"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, types]
source_count: 1
---

# Compound Types

## Short Definition

Compound types bir nechta valueni bitta type ichida guruhlaydi. Rustning primitive compound types: tuples va arrays.

## Why It Matters

Ko'p Rust programlarda related valuesni birga saqlash yoki fixed-size collection bilan ishlash kerak bo'ladi.

## Mental Model

- Tuple: fixed length, element typelari turlicha bo'lishi mumkin.
- Array: fixed length, barcha elementlar bir xil type.
- Unit `()`: empty value va empty return type.

## Syntax and Examples

Tuple:

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, y, z) = tup;
let first = tup.0;
```

Array:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
let repeated = [3; 5];
let first = a[0];
```

## Common Mistakes

- Array length o'zgaradi deb o'ylash.
- Out-of-bounds indexing runtime panic berishini unutish.
- Tuple indexlari 0dan boshlanishini unutish.

## Related Concepts

- [[data-types|data types]]
- [[tuple]]
- [[array]]
- [[unit-type|unit type]]
- [[memory-safety|memory safety]]

## Sources

- [[3-2-data-types-the-rust-programming-language]]
