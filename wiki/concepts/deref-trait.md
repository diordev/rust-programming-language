---
title: "Deref Trait"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, traits, deref, smart-pointers]
source_count: 3
---

# Deref Trait

## Short Definition

`Deref` trait `*` dereference behaviorini custom qilishga va deref coercion'ga asos bo'lishga xizmat qiladi.

## Why It Matters

`Box<T>`, `String`, va ayrim wrapper typelar reference kutadigan API bilan ergonomic ishlaydi.

## Mental Model

`Deref` inner value'ni move qilmaydi; `&Self::Target` qaytaradi. Shuning uchun smart pointer yoki wrapper "referencega o'xshab" ishlashi mumkin.

Advance newtype source yana bir amaliy boundary ko'rsatadi: `FileWrapper(File)` uchun `Deref<Target = File>` yozilsa, wrapper ustida `metadata()` kabi method'lar ishlaydi. Bu qulay, lekin wrapperning alohida API surface'ini ham yumshatadi.

## Syntax and Examples

```rust
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```

## Common Mistakes

- `Deref`ni har qanday wrapper uchun avtomatik yozish.
- `Deref`ni ownership conversion deb o'ylash.
- Newtype uchun u default tavsiya deb hisoblash.

## Related Concepts

- [[smart-pointers]]
- [[box-t|Box<T>]]
- [[dereference-operator]]
- [[deref-coercions]]
- [[deref-mut-trait|DerefMut trait]]
- [[newtype-pattern|newtype pattern]]
- [[traits]]

## Sources

- [[15-2-treating-smart-pointers-like-regular-references]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
- [[wiki/sources/rust-for-backend-developers-newtype-pattern]]

