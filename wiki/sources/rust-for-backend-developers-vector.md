---
title: "Вектор - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, vector]
source_count: 1
---

# Вектор - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/14. Вектор.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/vector.html

## Detailed Summary

Bu source `Vec<T>`ni arrayning dynamic alternative'i sifatida tanishtiradi. Core idea oddiy: array length compile-time'da ma'lum bo'lishi kerak, vector esa runtime'da o'sishi va kamayishi mumkin.

Source avval amaliy API'ni ko'rsatadi: `Vec::new()`, `push`, indexing, va `Vec<T>` generic notation. Keyin eng foydali qismga o'tadi: memory layout. Vector variable'ning o'zi stackda faqat metadata saqlaydi, actual elements esa heap bufferda yashaydi.

Berilgan mental model uchta maydondan iborat: heap buffer boshiga pointer, `len`, va `capacity`. Bu model `with_capacity`ni ham tushuntiradi: oldindan kerakli buffer hajmini ajratib, bir necha `push` paytidagi qayta allocationlarni kamaytirish mumkin.

Source reallocation xavfini ham sodda ko'rsatadi: `len == capacity` bo'lganda yangi kattaroq buffer olinadi, eski elementlar ko'chiriladi, eski buffer tashlanadi. Bu keyin borrowing rules nega vector elementlariga reference turganda `push`ni cheklashini tushunishga yordam beradi.

Oxirida `vec![]` macro practical shortcut sifatida beriladi. Backend code uchun bu deyarli default constructor syntax.

## Key Concepts

- [[vector|Vec<T>]]
- [[vec-macro|vec! macro]]
- [[generics]]
- [[stack-and-heap|stack and heap]]
- [[borrowing]]
- [[vector-indexing|vector indexing]]

## Code Examples

```rust
let mut my_vec: Vec<i32> = Vec::new();
my_vec.push(1);
my_vec.push(2);
my_vec.push(3);
let third = my_vec[2];
```

```rust
let mut my_vec: Vec<i32> = Vec::with_capacity(3);
my_vec.push(1);
my_vec.push(2);
my_vec.push(3);
my_vec.push(4);
```

```rust
let my_vec = vec![1, 2, 3];
```

## Exercises or Practice Ideas

- `Vec::new()` va `Vec::with_capacity(100)`ni qaysi workloadlarda tanlashni yozing.
- `Vec<T>` stack metadata va heap buffer modelini diagrammasiz matn bilan tushuntirib bering.
- Reference active paytda vectorni mutate qilish nega xavfli bo'lishini tushuntiring.

## Questions Raised

- `capacity` strategiyasi stable API contractmi yoki implementation detailmi?
- `vec![]` bilan yaratilgan vector va `push` orqali to'ldirilgan vector orasida qaysi mental model foydaliroq?
- Qachon `Vec<T>` o'rniga fixed array yoki boshqa collection kerak bo'ladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-vector]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[vector|Vec<T>]]
- [[vec-macro|vec! macro]]
- [[stack-and-heap|stack and heap]]
