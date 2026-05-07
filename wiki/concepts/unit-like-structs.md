---
title: "Unit-Like Structs"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, structs, traits]
source_count: 1
---

# Unit-Like Structs

## Short Definition

Unit-like struct hech qanday field saqlamaydigan struct type bo'lib, `()` [[unit-type|unit type]]ga o'xshab ishlaydi.

## Why It Matters

Data saqlash kerak bo'lmasa ham, biror type uchun behavior yoki [[traits]] implementation kerak bo'lishi mumkin.

## Mental Model

Type bor, lekin instance ichida data yo'q. Shuning uchun definition va instantiation juda ixcham.

## Syntax and Examples

```rust
struct AlwaysEqual;

let subject = AlwaysEqual;
```

## Common Mistakes

- Unit-like structni impossible value deb o'ylash.
- Datasiz type ham trait implement qilishi mumkinligini unutish.

## Related Concepts

- [[structs]]
- [[unit-type|unit type]]
- [[traits]]

## Sources

- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
