---
title: "self Parameter"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, methods, ownership]
source_count: 1
---

# self Parameter

## Short Definition

`self` method chaqirilayotgan instance'ni bildiradigan maxsus birinchi parameter.

## Why It Matters

`self`, `&self`, va `&mut self` tanlovi method instance ownershipini oladimi, immutable borrow qiladimi, yoki mutable borrow qiladimi, aniq ko'rsatadi.

## Mental Model

`&self` `self: &Self` shorthand. `Self` `impl` qilinayotgan type aliasi.

## Syntax and Examples

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

Receiver forms:

- `&self`: read-only borrow;
- `&mut self`: mutable borrow;
- `self`: ownershipni consume qilish.

## Common Mistakes

- Har doim `self` ownershipni oladi deb o'ylash.
- Mutating method uchun `&mut self` kerakligini unutish.

## Related Concepts

- [[methods]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[self-type|Self type]]

## Sources

- [[5-3-methods-the-rust-programming-language]]
