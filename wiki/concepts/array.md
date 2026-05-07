---
title: "Array"
type: concept
status: needs-review
created: 2026-05-06
updated: 2026-05-06
tags: [rust, types]
source_count: 2
---

# Array

## Short Definition

Array bir xil typedagi fixed length elementlar collectioni.

## Why It Matters

Array stackda saqlanishi mumkin va length compile time'da ma'lum bo'ladi. Dynamic length kerak bo'lsa odatda [[vector|Vec<T>]] ishlatiladi.

## Mental Model

Array type'i element type va lengthdan iborat: `[T; N]`.

## Syntax and Examples

```rust
let values: [i32; 3] = [1, 2, 3];
let first = values[0];
```

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

- [[3-2-data-types-the-rust-programming-language]]
- [[8-common-collections-the-rust-programming-language]]
