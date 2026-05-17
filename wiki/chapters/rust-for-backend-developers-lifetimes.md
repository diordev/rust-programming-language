---
title: "Rust for Backend Developers: Lifetimes"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, lifetimes, chapter]
source_count: 1
---

# Rust for Backend Developers: Lifetimes

## Learning Goal

Lifetime annotation referencelar umrini uzaytirmasdan, ularning o'zaro munosabatini compilerga qanday bildirishi tushunilsin.

## Main Ideas

- Lifetime annotation relationni bildiradi, data umrini uzaytirmaydi.
- Reference return qiladigan functionlar ko'pincha explicit lifetime talab qiladi.
- `'static` string literal kabi program oxirigacha yashaydigan data uchun.
- Beginner stage'da ba'zan owned return yoki `.clone()` dizaynni soddalashtiradi.

## Concepts

- [[lifetimes]]
- [[static-lifetime]]
- [[dangling-reference|dangling reference]]
- [[borrow-checker]]
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]]

## Examples

```rust
fn take_longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

## Exercises

- Reference return qiladigan 2 ta function signature yozing.
- Owned `String` qaytarish va `&str` qaytarish tradeoffini yozing.

## Review Questions

- Lifetime annotation nimani bildiradi va nimani bildirmaydi?
- `'static` qachon haqiqiy yechim, qachon symptom?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-lifetimes]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[lifetimes]]
- [[static-lifetime]]
