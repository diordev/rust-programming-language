---
title: "Указатели - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, pointers]
source_count: 1
---

# Указатели - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/24. Указатели.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/pointers.html

## Detailed Summary

Bu source raw pointerlarni Rustdagi safe reference modelidan tashqaridagi low-level escape hatch sifatida tanishtiradi. `*const T` immutable, `*mut T` mutable raw pointer. Pointer yaratish safe bo'lishi mumkin, lekin dereference yoki pointerni referencenga aylantirish unsafe boundaryga kiradi.

Source `unsafe` block va `unsafe fn`dan boshlaydi. Muhim tuzatish: source "unsafe fn body to'liq unsafe block" deb yozadi, lekin wiki bunu ehtiyotkor qayd qiladi. Rust 2024 modelida unsafe operation `unsafe fn` ichida ham explicit `unsafe { ... }` talab qilishi yoki hech bo'lmasa warning chiqarishi mumkin; shuning uchun unsafe scope'ni qo'lda tor saqlash kerak.

Pointer olishning uch yo'li ko'rsatiladi:

1. reference'dan cast orqali
2. zamonaviy `&raw const` / `&raw mut`
3. eski `addr_of!` / `addr_of_mut!`

Bu bo'lim ayniqsa foydali, chunki `&raw` operatorlari oldinroq `raw-borrow-operators` va `mutable static` materiallari bilan ulanadi.

Dereference `*ptr` Cga o'xshaydi, lekin Rustda unsafe. Source "reference'dan pointer qilish safe, lekin pointerni dereference qilish unsafe" boundarysini aniq ko'rsatadi.

Keyingi bo'lim borrow checker cheklovlarini raw pointerlar orqali chetlab o'tish mumkinligini ko'rsatadi. Doubly linked list va merge sort misollari beriladi. Asosiy takeaway: buni faqat low-level data structure yoki algorithm uchun, invariantlar to'liq nazorat qilinadigan joyda ishlatish kerak.

`let r1: &mut i32 = &mut a; let ptr = r1 as *mut i32; let r2 = ptr.as_mut().unwrap();` misoli orqali raw pointer yordamida ikkinchi mutable reference yasash mumkinligi ko'rsatiladi. Bu juda xavfli pattern bo'lgani uchun source o'zi ham test va Miri bilan tekshirishni tavsiya qiladi.

## Key Concepts

- [[raw-pointer]]
- [[raw-borrow-operators|raw borrow operators]]
- [[dereference-operator|dereference operator]]
- [[unsafe-rust|unsafe Rust]]
- [[unsafe-superpowers|unsafe superpowers]]
- [[reference]]
- [[mutable-reference|mutable reference]]
- [[borrowing]]
- [[data-race|data race]]
- [[undefined-behavior|undefined behavior]]
- [[miri|Miri]]

## Code Examples

```rust
let mut v: i32 = 5;
let const_ptr: *const i32 = &v as *const i32;
let mut_ptr: *mut i32 = &mut v as *mut i32;
```

```rust
let mut v: i32 = 5;
let const_ptr: *const i32 = &raw const v;
let mut_ptr: *mut i32 = &raw mut v;
```

```rust
let mut v: i32 = 5;
let const_ptr: *const i32 = std::ptr::addr_of!(v);
let mut_ptr: *mut i32 = std::ptr::addr_of_mut!(v);
```

```rust
let a = 5;
let ptr = (&a) as *const i32;
unsafe {
    println!("{}", *ptr);
}
```

```rust
unsafe {
    let r1: &mut i32 = &mut a;
    let ptr: *mut i32 = r1 as *mut i32;
    let r2: &mut i32 = ptr.as_mut().unwrap();
    inc(r1);
    inc(r2);
}
```

## Exercises or Practice Ideas

- `&raw const`, cast, va `addr_of!` bilan olingan pointerlarni yonma-yon yozing.
- Raw pointer dereference qaysi nuqtada unsafe bo'lishini bitta minimal misolda ajrating.
- Borrow checker nega doubly linked list yoki merge sort kabi joylarda to'g'ridan-to'g'ri yetmay qolishini yozing.

## Questions Raised

- Qachon raw pointer kerak, qachon safe abstraction yetarli?
- `unsafe fn` va `unsafe {}` scope'lari orasida qaysi boundary saqlanishi kerak?
- Raw pointerdan yangi reference yasashda invariantlarni qanday hujjatlashtirish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-pointers]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[raw-pointer]]
- [[unsafe-rust|unsafe Rust]]
- [[miri|Miri]]
