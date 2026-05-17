---
title: "Unit Type"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, types]
source_count: 4
---

# Unit Type

## Short Definition

`unit type` Rustdagi `()` type'i bo'lib, meaningful value yo'qligini bildiradi.

## Why It Matters

Semicolon expressionni statementga aylantiradi va ko'pincha return value `()` bo'lib qoladi.

## Mental Model

`()` "nothing useful to return" degan bitta valuega ega type.

[[unit-like-structs]] field saqlamasligi bilan unit typega o'xshaydi, lekin ular named custom type bo'lib, trait implementation uchun ishlatilishi mumkin.

Backend beginner source unit'ni `void` bilan chalg'itmaslikka alohida urg'u beradi: `void` bo'sh to'plam modeli bo'lsa, `()` bitta haqiqiy qiymatga ega singleton type.

## Syntax and Examples

```rust
fn log_message() {
    println!("done");
}
```

```rust
let unit_value: () = ();
```

`if` branch oxiridagi semicolon ham ko'pincha `()`ga olib keladi:

```rust
let v: () = if condition { 1; } else { 2; };
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

- [[3-2-data-types]]
- [[5-1-defining-and-instantiating-structs]]
- [[wiki/sources/rust-for-backend-developers-primitive-types]]
- [[wiki/sources/rust-for-backend-developers-if]]
