---
title: "Borrow Trait"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, traits, borrowing, collections]
source_count: 1
---

# Borrow Trait

## Short Definition

`Borrow<T>` trait'i owner type'dan `&T` borrowed view beradi, lekin shu bilan birga owner va borrowed view orasida `Eq`/`Ord`/`Hash` consistency contractini talab qiladi.

## Why It Matters

Collectionlar va lookup API'lar uchun bu kuchli signal: borrowed ko'rinish bilan qilingan comparison yoki hash owner type bilan mos natija berishi kerak.

## Mental Model

`Borrow` tashqi ko'rinishda `AsRef`ga o'xshaydi, lekin semantik jihatdan ancha kuchli. Agar `x.borrow() == y.borrow()` bo'lsa, odatda `x == y` bilan ham mos kelishi kerak; shu narsa `Ord` va `Hash` uchun ham amal qiladi.

## Syntax and Examples

```rust
pub trait Borrow<Borrowed>
where
    Borrowed: ?Sized,
{
    fn borrow(&self) -> &Borrowed;
}
```

## Common Mistakes

- `Borrow`ni oddiy `AsRef` replacement deb ishlatish.
- Owner va borrowed view uchun `Eq`/`Ord`/`Hash` semantics mos kelmasligini e'tiborsiz qoldirish.

## Related Concepts

- [[asref-trait]]
- [[borrowmut-trait]]
- [[eq-trait]]
- [[ord-trait|Ord]]
- [[hash-trait]]

## Sources

- [[wiki/sources/rust-for-backend-developers-common-traits]]

