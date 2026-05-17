---
title: "From Trait"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, traits, conversion, error-handling]
source_count: 2
---

# From Trait

## Short Definition

`From<T>` trait bir type'dan boshqa type'ga canonical ownership conversionni belgilaydi.

## Why It Matters

Bu explicit constructor-like conversion uchun eng toza yo'l. Shuningdek `?` operator error propagation paytida `From::from`dan foydalanishi mumkin.

## Mental Model

`From<X> for Y` degani: `X`dan `Y` yasashning rasmiy yo'li bor. Odatda aynan `From` implement qilinadi. `Into<Y> for X` esa undan avtomatik keladi, shu sabab `Into`ni qo'lda implement qilish default emas.

## Syntax and Examples

```rust
impl From<[u8; 4]> for Ip4Addr {
    fn from(value: [u8; 4]) -> Self {
        let [a, b, c, d] = value;
        Ip4Addr(a, b, c, d)
    }
}
```

Error conversion:

```rust
impl From<std::io::Error> for OurError {
    fn from(error: std::io::Error) -> OurError {
        OurError::Io(error)
    }
}
```

## Common Mistakes

- `Into`ni qo'lda implement qilish default yo'l deb o'ylash.
- `From` har qanday potentially lossy yoki surprising conversion uchun ham to'g'ri deb taxmin qilish.
- `?` hamma error typelarni `From`siz convert qiladi deb o'ylash.

## Related Concepts

- [[into-trait]]
- [[traits]]
- [[question-mark-operator|question mark operator]]
- [[error-propagation|error propagation]]
- [[result|Result<T, E>]]

## Sources

- [[9-2-recoverable-errors-with-result]]
- [[wiki/sources/rust-for-backend-developers-common-traits]]
