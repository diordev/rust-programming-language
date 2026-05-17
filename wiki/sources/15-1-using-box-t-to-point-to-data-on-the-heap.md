---
title: "15.1. Using Box<T> to Point to Data on the Heap"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, box, heap, recursive-types]
source_count: 1
---

# 15.1. Using Box<T> to Point to Data on the Heap

## Source

- Manba: The Rust Programming Language, 15.1
- URL: https://doc.rust-lang.org/stable/book/ch15-01-box.html
- Raw fayl: `raw/books/the_rust_programming_language/15.1. Using Box<T> to Point to Data on the Heap.md`

## Detailed Summary

`Box<T>` eng sodda smart pointerlardan biri. U qiymatni [[stack-and-heap|heap]]da saqlaydi, stackda esa heap data'ga pointer turadi.

```rust
fn main() {
    let b = Box::new(5);
    println!("b = {b}");
}
```

Bu misolda `5` heapda joylashadi, `b` esa stackdagi box value. Scope tugaganda box ham, u ko'rsatayotgan heap data ham [[drop|dropped]] bo'ladi.

Kitob `Box<T>`ni uch asosiy holatda foydali deb beradi:

- Compile time'da size'i noma'lum bo'lgan type'ni size talab qiladigan joyda ishlatish.
- Juda katta data ownership'ini ko'chirganda stackda katta copy bo'lishini oldini olish.
- Concrete type emas, ma'lum traitni implement qilgan value'ga ownership qilish kerak bo'lganda.

15.1 asosiy urg'uni [[recursive-types|recursive types]]ga beradi. Rust har bir type size'ini compile time'da bilishi kerak. Shuning uchun to'g'ridan-to'g'ri recursive enum ishlamaydi:

```rust
enum List {
    Cons(i32, List),
    Nil,
}
```

`Cons(i32, List)` ichida yana `List`, uning ichida yana `List` bo'lgani uchun compiler type size'ini infinite deb ko'radi va [[e0072-recursive-type-has-infinite-size|E0072]] beradi.

Yechim - indirection qo'shish. `Box<T>` pointer size'i fixed bo'lgani uchun recursive cycle uziladi:

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use crate::List::{Cons, Nil};

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
}
```

Bu modelda `Cons` stackda `i32` va box pointerini saqlaydi; keyingi `List` heapda turadi. Logical recursion saqlanadi, lekin physical layout compiler uchun finite bo'ladi.

## Key Concepts

- [[box-t|Box<T>]]
- [[stack-and-heap|stack and heap]]
- [[recursive-types]]
- [[smart-pointers]]
- [[ownership]]
- [[drop|Drop trait]]
- [[deref-trait|Deref trait]]
- [[e0072-recursive-type-has-infinite-size|E0072 recursive type has infinite size]]

## Code Examples

Heap allocation:

```rust
fn main() {
    let b = Box::new(5);
    println!("b = {b}");
}
```

Recursive cons list:

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
```

## Exercises or Practice Ideas

- `List` enumini avval `Cons(i32, List)` bilan yozib, compiler errorni o'qing.
- Keyin `Box<List>`ga almashtirib, size muammosi qanday hal bo'lganini chizing.
- `Vec<T>` nima uchun cons listdan ko'ra Rustda ko'proq default list ekanini tushuntiring.

## Questions Raised

- `Box<T>` heap allocationdan boshqa behavior bermasa, nega u smart pointer hisoblanadi?
- Recursive type uchun `Box<T>`, `Rc<T>`, yoki `&T` tanlovi nimaga bogliq?
- `Box<T>` ownership transfer qilinganda heapdagi data ko'chadimi yoki faqat pointer metadata ko'chadimi?

## Links To Update

- [[wiki/chapters/15-smart-pointers]]
- [[box-t]]
- [[recursive-types]]
- [[box-cons-list]]
- [[e0072-recursive-type-has-infinite-size]]
