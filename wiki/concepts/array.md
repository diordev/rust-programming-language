---
title: "Array"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, types]
source_count: 3
---

# Array

## Short Definition

Array bir xil typedagi fixed length elementlar collectioni.

## Why It Matters

Array stackda saqlanishi mumkin va length compile time'da ma'lum bo'ladi. Dynamic length kerak bo'lsa odatda [[vector|Vec<T>]] ishlatiladi.

## Mental Model

Array type'i element type va lengthdan iborat: `[T; N]`.

Bu yerda `N` type'ning bir qismi. Shuning uchun `[u8; 4]` va `[u8; 8]` turli typelar hisoblanadi.

## Syntax and Examples

```rust
let values: [i32; 3] = [1, 2, 3];
let first = values[0];
```

```rust
let ip: [u8; 4] = [192, 168, 0, 1];
println!("{ip:?}");
```

Local fixed-size arraylar uchun "odatda stack frame ichida turadi" mental modeli foydali, lekin storage placementni mutlaq qoida deb qabul qilmaslik kerak.

## Common Mistakes

- Runtime o'zgaradigan length uchun array tanlash.
- Invalid index panicga olib kelishini unutish.

## Related Concepts

- [[compound-types|compound types]]
- [[slices]]
- [[vector|Vec<T>]]
- [[collections]]
- [[integer-overflow|integer overflow]]

## Sources

- [[3-2-data-types]]
- [[wiki/sources/8-common-collections]]
- [[wiki/sources/rust-for-backend-developers-arrays]]
