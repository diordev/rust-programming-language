---
title: "Rust for Backend Developers: Smart Pointers"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, smart-pointers, chapter]
source_count: 1
---

# Rust for Backend Developers: Smart Pointers

## Learning Goal

Heap va ownership ustidagi pointer-like abstractionlarni ajratish: `Box<T>`, `Deref`, `Rc<T>`, `Cell<T>`, `RefCell<T>`, va `Arc<T>` qaysi muammoni qaysi cost bilan hal qilishini tushunish.

## Main Ideas

- Smart pointer oddiy address emas; ownership, cleanup, reference counting, yoki runtime borrow checking kabi qo'shimcha behavior olib yuradi.
- `Box<T>` heap indirection va recursive type sizing uchun eng sodda owning pointer.
- `Deref` va `DerefMut` smart pointer'ni reference kabi ishlatish ergonomicsini beradi, lekin ownership qoidalarini bekor qilmaydi.
- `Rc<T>` shared ownership beradi, lekin faqat single-threaded.
- `Cell<T>` whole-value replacement uchun, `RefCell<T>` esa borrow-based mutationni runtime'da tekshirish uchun.
- `Rc<Cell<T>>` va `Rc<RefCell<T>>` bir xil emas; birinchisi replacement modeli, ikkinchisi runtime borrow modeli.
- `Arc<T>` `Rc<T>`ning thread-safe versiyasi, default replacement emas.

## Concepts

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

## Examples

```rust
enum List<T> {
    Nil,
    Elem(T, Box<List<T>>),
}
```

```rust
let mut b = Box::new(1);
*b = 2;
```

```rust
use std::{cell::RefCell, rc::Rc};

let shared = Rc::new(RefCell::new(1));
*shared.borrow_mut() += 1;
```

```rust
use std::sync::Arc;

let a = Arc::new(String::from("hello"));
let b = Arc::clone(&a);
```

## Exercises

- `Box<List<T>>`siz recursive enum yozib, compiler nima uchun rad etishini ko'rsating.
- `Rc<T>`, `Rc<Cell<T>>`, va `Rc<RefCell<T>>` uchun ruxsat etilgan mutation modelini taqqoslang.
- `Rc<T>` va `Arc<T>` orasidagi performance va safety tradeoffini yozing.
- `Deref` tufayli smart pointer reference kutadigan functionga qanday uzatilishini tekshiring.

## Review Questions

- `Box<T>`ning "zero-cost abstraction" degani nimani anglatadi, nimani anglatmaydi?
- `Rc<T>` nega `DerefMut` bermaydi?
- `Cell<T>` va `RefCell<T>`ning asosiy amaliy farqi nima?
- `RefCell<T>` borrow qoidalarini bekor qiladimi?
- `Rc<T>` va `Arc<T>` qaysi boundary bilan ajraladi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[smart-pointers]]
- [[box-t|Box<T>]]
- [[recursive-types]]
- [[deref-trait|Deref trait]]
- [[deref-mut-trait|DerefMut trait]]
- [[rc-t|Rc<T>]]
- [[cell-t|Cell<T>]]
- [[refcell-t|RefCell<T>]]
- [[interior-mutability|interior mutability]]
- [[arc-t|Arc<T>]]
