---
title: "Marker Trait"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, traits, type-system]
source_count: 1
---

# Marker Trait

## Short Definition

`marker trait` methodsiz yoki deyarli methodsiz semantic signal beradigan trait.

## Why It Matters

Bunday trait type-system va generic code'ga "shu type ma'lum property'ga ega" degan axborot beradi. Behaviorning o'zi ko'pincha trait methodlari bilan emas, compiler yoki boshqa API qarorlari bilan ko'rinadi.

## Mental Model

Marker trait "bu value bilan qanday ishlash mumkin?" degan savolga qisqa belgi beradi. `Copy` bunga klassik misol: yangi method qo'shmaydi, lekin assignment/paytda move o'rniga implicit copy semantics tanlanadi. `Send` va `Sync` ham marker-like semantikaga ega, lekin ular alohida auto trait mexanizmi bilan ham bog'langan.

## Syntax and Examples

```rust
trait Copy: Clone {}
```

```rust
#[derive(Copy, Clone)]
struct Point {
    x: i32,
    y: i32,
}
```

## Common Mistakes

- Marker trait = auto trait deb o'ylash.
- Marker traitning effecti yo'q, chunki unda method yo'q deb o'ylash.
- Trait bildirayotgan invariant yoki property'ni tushunmasdan implement qilish.

## Related Concepts

- [[copy-trait|Copy trait]]
- [[clone]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[traits]]

## Sources

- [[wiki/sources/rust-for-backend-developers-auto-derive-traits]]
