---
title: "Into Trait"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, traits, conversion]
source_count: 1
---

# Into Trait

## Short Definition

`Into<T>` trait'i value'ni boshqa type'ga ownership bilan convert qilishni bildiradi.

## Why It Matters

Function argumentini `impl Into<T>` qilish caller'ga ergonomikroq API beradi: turli compatible type'lar qabul qilinadi, function ichi esa bitta target type bilan ishlaydi.

## Mental Model

`From<X> for Y` conversionning canonical tomoni. `Into<Y> for X` esa caller tomondan ko'rinadigan tomon. Odatda `Into`ni qo'lda implement qilinmaydi, chunki `From` implement qilinganda `Into` avtomatik olinadi.

## Syntax and Examples

```rust
fn ping(addr: impl Into<Ip4Addr>) {
    let addr: Ip4Addr = addr.into();
}
```

## Common Mistakes

- `Into`ni qo'lda implement qilish default yo'l deb o'ylash.
- `impl Into<T>` argument oldim degani har qanday semantic conversionni qabul qilishim kerak deb o'ylash.

## Related Concepts

- [[from-trait]]
- [[traits]]
- [[ownership]]

## Sources

- [[wiki/sources/rust-for-backend-developers-common-traits]]

