---
title: "Smart Pointers"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-08
tags: [rust, smart-pointers, ownership, memory]
source_count: 3
---

# Smart Pointers

## Short Definition

Smart pointer pointer kabi ishlaydigan, lekin oddiy addressdan tashqari ownership, metadata, cleanup, yoki runtime checking kabi qo'shimcha behaviorga ega type.

## Why It Matters

Rustda references eng sodda pointer-like value bo'lib, data'ni borrow qiladi. Smart pointerlar esa ko'pincha data'ga ownership qiladi va shu ownership atrofida API beradi. `Box<T>`, `Rc<T>`, `RefCell<T>`, `String`, va `Vec<T>` kabi typelar Rust memory modelini amaliy code'da ishlatishga yordam beradi.

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

## Common Mistakes

- Smart pointer deganda faqat raw pointerni tushunish.
- Reference va smart pointer ownership jihatdan bir xil deb o'ylash.
- `Box<T>` shared ownership beradi deb o'ylash; shared ownership uchun `Rc<T>` keyingi bo'limlarda keladi.
- Smart pointerlar borrowing qoidalarini bekor qiladi deb o'ylash.

## Related Concepts

- [[ownership]]
- [[borrowing]]
- [[reference]]
- [[box-t|Box<T>]]
- [[deref-trait|Deref trait]]
- [[drop|Drop trait]]
- [[stack-and-heap|stack and heap]]

## Sources

- [[wiki/sources/0-2-introduction]]
- [[wiki/sources/15-smart-pointers]]
- [[15-1-using-box-t-to-point-to-data-on-the-heap]]
