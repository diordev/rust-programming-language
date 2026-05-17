---
title: "Умные указатели - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, smart-pointers]
source_count: 1
---

# Умные указатели - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/37. Умные указатели.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/smart-pointers.html

## Detailed Summary

Bu source smart pointer'larni oddiy address container emas, heap + ownership + runtime rule toolkit sifatida ochadi. Kirishdagi asosiy kontrast to'g'ri: oldingi boblarda `Vec` va `String` heap bilan ishlagan bo'lsa ham, ular heap detail'ini yashirib kelgan edi; bu yerda esa heap va indirection explicit bo'ladi.

`Box<T>` birinchi pointer sifatida beriladi. Durable model: `Box<T>` stack'da pointer metadata saqlaydi, heap'da esa `T` value turadi; `Box` heapdagi data'ning owneri. Source `zero cost abstraction` iborasini ishlatadi, lekin wiki'da bu ehtiyotkor yoziladi: bu "heap allocation tekin" degani emas. Gap stack representation deyarli pointer darajasida sodda ekanida; allocation va indirection costi baribir mavjud.

`Box<T>`ning amaliy motivatsiyasi recursive type problem. `enum List<T> { Elem(T, List<T>), Nil }` infinite size bo'lgani uchun yiqiladi, `Box<List<T>>` esa known-size indirection beradi. Bu yerda asosiy nuance "heapga qo'yish" emas, compile-time layout'ni finit qilish.

Keyingi qatlam `Deref` va `DerefMut`. Source `Box<T>`ni oddiy reference kabi ishlatish mumkinligini `*b = 2` va `increment(&mut b)` bilan ko'rsatadi. Durable signal: dereference operator smart pointerga "maxsus muomala" qilmaydi; compiler `Deref`/`DerefMut` orqali reference-like accessni tarjima qiladi. Bu ergonomics ownershipni bekor qilmaydi, faqat access syntax'ini yumshatadi.

`Rc<T>` bo'limi ownership modelini kengaytiradi. Oddiy Rust ownership bitta ownerni kutadi; `Rc<T>` esa shared ownership beradi. Source bu yerda ichki struktura haqida foydali mental model beradi: heapdagi data pointeri va reference count pointeri. `clone()` deep copy emas, count increment. Eng muhim durable caveat: `Rc<T>` single-threaded.

`Cell<T>` bo'limi ko'p joyda noto'g'ri tushuntiriladigan farqni ochadi. `Rc<T>` mutable dereference bermaydi, chunki shared ownership bilan odatiy mutable borrow modeli xavfli. `Cell<T>` esa borrow bermasdan, whole-value replacement qiladi. `set`, `replace`, va `get` (`Copy` bo'lsa) uning asosiy API'si. Bu `RefCell<T>` bilan bir narsa emas; `Cell<T>` value'ni joyida mutatsiya qilish emas, butun value'ni almashtirish modeli.

Source `Cell<T>` uchun "atomar va xavfsiz" degan ifodani ishlatadi. Wiki'da bu yerda aniqlik saqlanadi: gap thread-safety emas, single-threaded interior mutation paytida invalid reference bermaslik haqida. `Cell<T>` data race protection bermaydi.

`Rc<Cell<T>>` kombinatsiyasi source'da shared ownership + replaceable payload pattern sifatida chiqadi. Bu `Rc<RefCell<T>>`dan torroq, lekin ko'pincha soddaroq model.

`RefCell<T>` esa runtime borrow checking'ga o'tadi. Bu yerda muhim durable takeaway: Rust borrow qoidalari yo'qolmaydi; faqat compile time'dan runtime'ga ko'chadi. `borrow()` va `borrow_mut()` conflict qilsa compile error emas, panic bo'ladi. Demak `RefCell<T>` compile-time rad etilgan har qanday pattern uchun emas, qoidaga rioya qilinishiga dasturchi o'zi javob beradigan holatlar uchun.

Source `Rc<RefCell<T>>` orqali shared mutable linked structure misolini beradi. Bu smart pointer stack'ining ma'nosini juda aniq ko'rsatadi: `Rc` ownershipni bo'ladi, `RefCell` mutationni runtime tekshiradi.

Oxirgi bo'lim `Arc<T>`. Bu yerda source ataylab chuqurlashmaydi va to'g'ri signal beradi: `Rc<T>`ning thread-safe analogi. Wiki'da shu boundary saqlanadi: `Rc<T>` single-threaded reference counting, `Arc<T>` esa thread-safe reference counting; default tanlov emas, kerak joydagi tanlov.

## Key Concepts

- [[smart-pointers]]
- [[box-t|Box<T>]]
- [[recursive-types]]
- [[deref-trait|Deref trait]]
- [[deref-mut-trait|DerefMut trait]]
- [[dereference-operator|dereference operator]]
- [[rc-t|Rc<T>]]
- [[cell-t|Cell<T>]]
- [[refcell-t|RefCell<T>]]
- [[interior-mutability|interior mutability]]
- [[arc-t|Arc<T>]]

## Code Examples

```rust
#[derive(Debug)]
enum List<T> {
    Nil,
    Elem(T, Box<List<T>>),
}
```

```rust
fn increment(v: &mut i32) {
    *v += 1;
}

let mut b = Box::new(1);
*b = 2;
increment(&mut b);
```

```rust
use std::rc::Rc;

let rc1 = Rc::new("Hello".to_string());
let rc2 = Rc::clone(&rc1);
```

```rust
use std::{cell::Cell, rc::Rc};

let shared = Rc::new(Cell::new(1));
Rc::clone(&shared).set(5);
```

```rust
use std::{cell::RefCell, rc::Rc};

let shared = Rc::new(RefCell::new(1));
*shared.borrow_mut() += 10;
```

## Exercises or Practice Ideas

- Recursive `List<T>`ni avval `Box<T>`siz yozib yiqiting, keyin `Box<List<T>>` bilan to'g'rilang.
- `Rc<T>`, `Rc<Cell<T>>`, va `Rc<RefCell<T>>` uchun qaysi operatsiyalar ruxsat etilishi va qaysi biri yo'qligini bir jadvalga tushiring.
- `Cell<T>` bilan `RefCell<T>`ni bir xil domainda ishlatib, qachon whole-value replacement yetishini ko'rsating.
- `Rc<T>`ni threadga yuborib compiler xatosini ko'ring, keyin `Arc<T>`ga almashtiring.
- `Deref` va `DerefMut` tufayli `Box<T>` reference kutadigan API bilan qanday ishlashini kichik helper function bilan tekshiring.

## Questions Raised

- Qachon `Box<T>` faqat indirection vositasi, qachon trait object yoki ownership transport vositasi bo'ladi?
- `Cell<T>` va `RefCell<T>` orasida eng amaliy tanlov mezoni nima?
- `Rc<T>` + `RefCell<T>` kuchli pattern bo'lsa ham, qaysi joyda u design smell bo'lib qoladi?
- `Arc<T>`ni `Rc<T>` o'rniga default ishlatish nima uchun noto'g'ri optimization direction?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-smart-pointers]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[smart-pointers]]
- [[box-t|Box<T>]]
- [[recursive-types]]
- [[deref-trait|Deref trait]]
- [[deref-mut-trait|DerefMut trait]]
- [[dereference-operator|dereference operator]]
- [[rc-t|Rc<T>]]
- [[cell-t|Cell<T>]]
- [[refcell-t|RefCell<T>]]
- [[interior-mutability|interior mutability]]
- [[arc-t|Arc<T>]]
