---
title: "Unit Type"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, types]
source_count: 2
---

# Unit Type

## Short Definition

`unit type` Rustdagi `()` type'i bo'lib, meaningful value yo'qligini bildiradi.

## Why It Matters

Semicolon expressionni statementga aylantiradi va ko'pincha return value `()` bo'lib qoladi.

## Mental Model

`()` "nothing useful to return" degan bitta valuega ega type.

[[unit-like-structs]] field saqlamasligi bilan unit typega o'xshaydi, lekin ular named custom type bo'lib, trait implementation uchun ishlatilishi mumkin.

## Syntax and Examples

```rust
fn log_message() {
    println!("done");
}
```

## Common Mistakes

- Final expressiondan keyin semicolon qo'yib, return type'ni tasodifan `()` qilish.
- Function body block'i value qaytarishini unutish.
- Unit-like struct va `()`ni bir xil type deb o'ylash.

## Related Concepts

- [[statements-and-expressions|statements and expressions]]
- [[functions]]
- [[compound-types|compound types]]
- [[unit-like-structs]]
- [[traits]]

## Sources

- [[3-2-data-types-the-rust-programming-language]]
- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
