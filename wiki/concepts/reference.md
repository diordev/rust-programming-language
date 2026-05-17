---
title: "Reference"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, ownership, borrowing]
source_count: 4
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

Basic dereference modeli:

```rust
let a: i32 = 5;
let ref_a: &i32 = &a;
println!("{}", *ref_a);
```

Reference pointerga o'xshaydi, lekin Rust compiler uning lifetime va aliasing qoidalarini tekshiradi.

Raw pointer bilan boundary shu yerda boshlanadi: reference compiler tomonidan tekshiriladi, raw pointer esa low-level adres bo'lib, dereference uchun `unsafe` talab qiladi.

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

- [[4-2-references-and-borrowing]]
- [[5-2-an-example-program-using-structs]]
- [[wiki/sources/rust-for-backend-developers-references]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
