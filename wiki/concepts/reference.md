---
title: "Reference"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, ownership, borrowing]
source_count: 2
---

# Reference

## Short Definition

Reference value'ga ownership olmasdan ishora qiladigan address-like value. Syntax: `&value`.

## Why It Matters

References functionlarga value'dan foydalanish imkonini beradi, lekin ownershipni transfer qilmaydi. Shu sababli caller original value'dan keyin ham foydalana oladi.

## Mental Model

Reference pointerga o'xshaydi, lekin Rust compiler reference valid value'ga ishora qilishini kafolatlaydi.

```rust
let s1 = String::from("hello");
let len = calculate_length(&s1);
```

Method receiver va struct parametersda references ownershipni saqlash uchun ishlatiladi:

```rust
fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```

## Syntax and Examples

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
}
```

## Common Mistakes

- Reference owner deb o'ylash; reference scope'dan chiqqanda pointed value dropped bo'lmaydi.
- Immutable reference orqali mutate qilishga urinish.
- Dangling reference qaytarishga urinish.
- Debug macro kabi ownership oladigan joylarda reference kerak bo'lishini unutish.

## Related Concepts

- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[dangling-reference|dangling reference]]
- [[ownership]]
- [[slices]]
- [[methods]]
- [[dbg-macro|dbg! macro]]

## Sources

- [[4-2-references-and-borrowing-the-rust-programming-language]]
- [[5-2-an-example-program-using-structs-the-rust-programming-language]]
