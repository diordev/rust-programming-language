---
title: "derive Attribute"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, attributes, traits]
source_count: 1
---

# derive Attribute

## Short Definition

`#[derive(...)]` compilerga type uchun ayrim standard trait implementationsni avtomatik yaratishni so'raydigan attribute.

## Why It Matters

`#[derive(Debug)]` custom structni [[debug-formatting|Debug formatting]] bilan chiqarishga ruxsat beradi.

## Mental Model

Derive "bu traitning default mechanical implementationini men uchun generate qil" degan signal. Bu explicit opt in, Rust custom type behaviorini taxmin qilmaydi.

## Syntax and Examples

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
```

## Common Mistakes

- Derive hamma traitlar uchun mavjud deb o'ylash.
- Derived behavior custom behavior bilan bir xil bo'ladi deb kutish.

## Related Concepts

- [[debug-trait|Debug trait]]
- [[traits]]
- [[structs]]

## Sources

- [[5-2-an-example-program-using-structs-the-rust-programming-language]]
