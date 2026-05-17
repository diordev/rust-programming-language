---
title: "AsRef Trait"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, traits, references, conversion]
source_count: 1
---

# AsRef Trait

## Short Definition

`AsRef<T>` trait'i `&self`dan `&T` ko'rinishidagi borrowed view olish uchun ishlatiladi.

## Why It Matters

Function argumentlarini `impl AsRef<T>` qilish API'ni flexible qiladi: caller turli wrapper yoki owner typelarni uzata oladi, function esa ichkarida `&T` bilan ishlaydi.

## Mental Model

`AsRef` reference conversion trait'i. U odatda data'ni qayta formatlamaydi yoki semantic equivalence contract bermaydi; shunchaki ichki data'ga borrowed access beradi.

## Syntax and Examples

```rust
impl AsRef<Product> for Shipment {
    fn as_ref(&self) -> &Product {
        &self.product
    }
}
```

## Common Mistakes

- `AsRef`ni `Borrow` bilan bir xil deb o'ylash.
- `AsRef` semantic `Eq`/`Ord`/`Hash` consistency va'da qiladi deb taxmin qilish.

## Related Concepts

- [[asmut-trait]]
- [[borrow-trait]]
- [[reference]]

## Sources

- [[wiki/sources/rust-for-backend-developers-common-traits]]

