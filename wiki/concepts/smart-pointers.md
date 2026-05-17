---
title: "Smart Pointers"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, smart-pointers, ownership, memory]
source_count: 4
---

# Smart Pointers

## Short Definition

Smart pointer pointer kabi ishlaydigan, lekin oddiy addressdan tashqari ownership, metadata, cleanup, yoki runtime checking kabi qo'shimcha behaviorga ega type.

## Why It Matters

Rustda references eng sodda pointer-like value bo'lib, data'ni borrow qiladi. Smart pointerlar esa ko'pincha data'ga ownership qiladi va shu ownership atrofida API beradi. `Box<T>`, `Rc<T>`, `Cell<T>`, `RefCell<T>`, `Arc<T>`, `String`, va `Vec<T>` kabi typelar Rust memory modelini amaliy code'da ishlatishga yordam beradi.

## Mental Model

Reference: "men value'ni borrow qilyapman."

Smart pointer: "men pointer kabi value'ga olib boraman, lekin qo'shimcha qoida ham olib yuraman."

Masalan `Box<T>` heapdagi value'ga owner bo'ladi; `Deref` sabab reference kabi ishlaydi; `Drop` sabab scope tugaganda cleanup qiladi.

## Syntax and Examples

```rust
let b = Box::new(5);
println!("b = {b}");
```

`Box<T>` stackda pointer metadata saqlaydi, heapda esa actual `T`.

```rust
use std::{cell::Cell, rc::Rc};

let shared = Rc::new(Cell::new(1));
Rc::clone(&shared).set(5);
```

## Common Mistakes

- Smart pointer deganda faqat raw pointerni tushunish.
- Reference va smart pointer ownership jihatdan bir xil deb o'ylash.
- `Box<T>` shared ownership beradi deb o'ylash; shared ownership uchun `Rc<T>` keyingi bo'limlarda keladi.
- `Box<T>` "zero-cost abstraction" degani heap allocationning o'zi bepul deb o'ylash.
- `Rc<T>` va `Arc<T>`ni bir xil cost bilan keladigan oddiy aliaslar deb o'ylash.
- Smart pointerlar borrowing qoidalarini bekor qiladi deb o'ylash.

## Related Concepts

- [[ownership]]
- [[borrowing]]
- [[reference]]
- [[box-t|Box<T>]]
- [[rc-t|Rc<T>]]
- [[cell-t|Cell<T>]]
- [[refcell-t|RefCell<T>]]
- [[arc-t|Arc<T>]]
- [[deref-trait|Deref trait]]
- [[drop|Drop trait]]
- [[stack-and-heap|stack and heap]]

## Sources

- [[wiki/sources/0-2-introduction]]
- [[wiki/sources/15-smart-pointers]]
- [[15-1-using-box-t-to-point-to-data-on-the-heap]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
