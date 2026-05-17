---
title: "Ссылки - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, references]
source_count: 1
---

# Ссылки - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/12. Ссылки.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/references.html

## Detailed Summary

Bu source Rustdagi reference'ni ownershipni ko'chirmasdan boshqa value'ga ishora qiluvchi semantic handle sifatida tanishtiradi. Syntax `&value`, type esa `&T`.

Source oddiy immutable reference misolini beradi va `*ref_a` bilan dereference ko'rsatadi. Shu joydan ikki qatlam ko'rinadi: kod darajasida reference pointerga o'xshaydi, lekin Rust modelida u raw pointer emas; compiler reference valid value'ga ishora qilayotganini tekshiradi.

Mutable reference uchun `&mut` ishlatiladi va bu faqat mutable bindingdan olinadi. Misolda `*ref_a = 99;` orqali original value'ni reference orqali o'zgartirish ko'rsatiladi.

Source C pointerlari bilan qisqa taqqoslash qiladi: Rust reference ko'pincha alohida runtime obyekt emas, balki compiler boshqaradigan semantic relation sifatida qaraladi. Bu soddalashtirilgan model, lekin o'quvchi uchun foydali. Amaliy nuqta esa aniq: Rust compiler lifetime'larni kuzatadi va dead owner'ga ishora qiladigan reference'ni compile qildirmaydi.

## Key Concepts

- [[reference]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[dereference-operator|dereference operator]]
- [[ownership]]
- [[lifetimes]]
- [[dangling-reference|dangling reference]]

## Code Examples

```rust
let a: i32 = 5;
let ref_a: &i32 = &a;
println!("Value in a is {}", *ref_a);
```

```rust
let mut a: i32 = 5;
let ref_a: &mut i32 = &mut a;
*ref_a = 99;
println!("Value in a is {}", *ref_a);
```

## Exercises or Practice Ideas

- Immutable va mutable reference uchun ikkita alohida function yozing.
- `*` dereference operatori kerak bo'ladigan va automatic deref yetarli bo'ladigan holatlarni toping.
- Dangling reference'ga urinish bilan compiler nimani to'xtatishini tekshiring.

## Questions Raised

- Reference qachon real pointerga o'xshab ko'riladi, qachon purely semantic model sifatida tushuntirish yetadi?
- `&T` va `&mut T` qoidalari lifetime bilan qanday birlashadi?
- Dereference qachon explicit, qachon compiler tomonidan avtomatik qilinadi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-references]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[reference]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
