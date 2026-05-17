---
title: "Self Type"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, methods, types]
source_count: 2
---

# Self Type

## Short Definition

`Self` `impl` block qaysi type uchun yozilgan bo'lsa, o'sha type'ning aliasi.

## Why It Matters

`Self` method receiver shorthandlarida va associated function return/bodylarida type nomini takrorlamaslikka yordam beradi.

## Mental Model

`impl Rectangle` ichida `Self` degani `Rectangle`. Trait definition ichida esa `Self` hali noma'lum future implementor type'ni bildiradi: masalan `fn make_default() -> Self;`.

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

- [[5-3-methods]]
- [[wiki/sources/rust-for-backend-developers-traits]]
