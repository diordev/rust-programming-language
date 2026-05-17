---
title: "Deref Trait"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, traits, deref, smart-pointers]
source_count: 2
---

# Deref Trait

## Short Definition

`Deref` trait `*` [[dereference-operator|dereference operator]] behaviorini custom qilishga imkon beradi. Smart pointer reference kabi ishlashi uchun ko'pincha `Deref` implement qiladi.

## Why It Matters

`Deref` tufayli `Box<T>`, `String`, va custom smart pointerlar reference kutadigan kod bilan ergonomic ishlaydi. Bu [[deref-coercions|deref coercions]] uchun ham asos.

## Mental Model

`*y` yozilganda, agar `y` oddiy reference emas, lekin `Deref` implement qilgan type bo'lsa, compiler buni conceptually `*(y.deref())` kabi ko'radi.

`deref` inner value'ni move qilmasligi uchun reference qaytaradi:

```rust
fn deref(&self) -> &Self::Target
```

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

let b = Box::new(1);
let r: &i32 = &b; // conceptually b.deref()
```

## Common Mistakes

- `Deref` method value qaytarishi kerak deb o'ylash.
- `Deref`ni har qanday wrapper type uchun avtomatik implement qilish. Bu API'da yashirin coercion va method lookup behaviorini oshiradi.
- `Deref` va [[drop|Drop trait]]ni chalkashtirish: biri access ergonomics, ikkinchisi cleanup.

## Related Concepts

- [[smart-pointers]]
- [[box-t|Box<T>]]
- [[dereference-operator]]
- [[deref-coercions]]
- [[deref-mut-trait|DerefMut trait]]
- [[traits]]
- [[trait-implementations|trait implementations]]

## Sources

- [[15-2-treating-smart-pointers-like-regular-references]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
