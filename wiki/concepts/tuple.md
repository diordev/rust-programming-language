---
title: "Tuple"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, types]
source_count: 3
---

# Tuple

## Short Definition

Tuple fixed lengthga ega compound type bo'lib, turli typelardagi valuesni bitta groupga jamlaydi.

## Why It Matters

Tuple oddiy grouping va multiple return values uchun qulay.

## Mental Model

Tuple elementlari position orqali aniqlanadi: `.0`, `.1`, yoki destructuring.

[[structs]] ham related valuesni group qiladi, lekin fields nomlanadi. [[tuple-structs]] esa tuple-like storage bilan named distinct type yaratadi.

## Syntax and Examples

```rust
let point: (i32, i32) = (10, 20);
let (x, y) = point;
```

Multiple return values:

```rust
fn split_pair() -> (i32, i32) {
    (1, 2)
}
```

## Common Mistakes

- Tuple elementlariga field name bilan murojaat qilishga urinish.
- Bir elementli tuple uchun trailing comma kerakligini unutish.
- Tuple yetarli bo'lmagan domain data uchun named struct ishlatmaslik.

## Related Concepts

- [[compound-types|compound types]]
- [[unit-type|unit type]]
- [[copy-trait|Copy trait]]
- [[structs]]
- [[tuple-structs]]
- [[functions]]
- [[slices]]

## Sources

- [[3-2-data-types]]
- [[wiki/sources/5-using-structs-to-structure-related-data]]
- [[wiki/sources/rust-for-backend-developers-tuples]]
