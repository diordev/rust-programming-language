---
title: "Self Type"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, methods, types]
source_count: 1
---

# Self Type

## Short Definition

`Self` `impl` block qaysi type uchun yozilgan bo'lsa, o'sha type'ning aliasi.

## Why It Matters

`Self` method receiver shorthandlarida va associated function return/bodylarida type nomini takrorlamaslikka yordam beradi.

## Mental Model

`impl Rectangle` ichida `Self` degani `Rectangle`.

## Syntax and Examples

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}
```

## Common Mistakes

- `Self` va `self`ni aralashtirish: `Self` type, `self` value/parameter.

## Related Concepts

- [[impl-block|impl block]]
- [[self-parameter|self parameter]]
- [[associated-functions]]

## Sources

- [[5-3-methods-the-rust-programming-language]]
