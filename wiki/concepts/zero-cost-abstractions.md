---
title: "Zero-Cost Abstractions"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, performance]
source_count: 6
---

# Zero-Cost Abstractions

## Short Definition

`zero-cost abstractions` yuqori darajadagi abstraction runtime'da qo'shimcha majburiy xarajat keltirmasligi kerak degan Rust design maqsadi.

## Why It Matters

Rust safe va expressive code yozishga intiladi, lekin systems programming uchun performance nazoratini ham saqlaydi.

## Mental Model

Abstraction compile time'da tekshiriladi va optimizatsiya qilinadi; runtime'da qo'lda yozilgan past darajadagi codega yaqin natija kutiladi.

Chapter 10 generics, traits, va lifetimes orqali abstractionni chuqurlashtiradi. Bu abstractions code reuse beradi, lekin Rust ularni compile-time checking bilan birlashtiradi.

Chapter 10.1 [[monomorphization]]ni generics performance modeli sifatida tushuntiradi. Compiler generic code'ni ishlatilgan concrete typelar bo'yicha specialized codega aylantiradi, shuning uchun generic `Option<T>` yoki `largest<T>` runtime'da hand-written concrete versions kabi ishlaydi.

Chapter 10.2 [[trait-bounds|trait bounds]] compile-time behavior checking berishini ko'rsatadi. Generic function required method yoki operationni runtime'da tekshirmaydi; compiler concrete type kerakli traitni implement qilganini oldindan tekshiradi.

Chapter 13.4 [[iterators]] va [[closures]] uchun benchmark o'lchadi: "The Adventures of Sherlock Holmes" kitobida "the" so'zini qidirish — for loop va iterator versiyasi deyarli bir xil tezlikda (~19.6ms vs ~19.2ms). Compiler iterator chain'ni **loop unrolling** va **bounds check elimination** bilan optimizatsiya qiladi, natijada qo'l bilan yozilgan assemblydek assembly chiqadi. Bjarne Stroustrup zero-overhead qoidasi: "What you don't use, you don't pay for. What you do use, you couldn't hand code any better."

## Syntax and Examples

```rust
let total: i32 = [1, 2, 3].iter().sum();
```

## Common Mistakes

- "Zero-cost" har qanday abstraction har doim bepul degani deb o'ylash.
- Performance claimni benchmark qilmasdan qabul qilish.

## Related Concepts

- [[traits]]
- [[generics]]
- [[monomorphization]]
- [[trait-bounds|trait bounds]]
- [[generic-functions|generic functions]]
- [[abstraction]]
- [[iterators]]
- [[closures]]

## Sources

- [[0-2-introduction-the-rust-programming-language]]
- [[10-generic-types-traits-and-lifetimes-the-rust-programming-language]]
- [[10-1-generic-data-types-the-rust-programming-language]]
- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
- [[13-functional-language-features-the-rust-programming-language]]
- [[13-4-performance-in-loops-vs-iterators-the-rust-programming-language]]
