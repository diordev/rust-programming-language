---
title: "Box<T>"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, smart-pointers, heap, ownership]
source_count: 3
---

# Box<T>

## Short Definition

`Box<T>` value'ni heapda saqlaydigan owning smart pointer. Stackda fixed-size pointer metadata, heapda esa `T` value turadi.

## Why It Matters

`Box<T>` Rustda indirection kerak bo'lgan joyda sodda yechim beradi. U recursive typelarni finite size qilish, katta data ownership'ini pointer orqali ko'chirish, va trait objects bilan ishlash uchun asosiy tool. Backend traits source'da `Box<dyn Trait>` return-position polymorphism uchun birinchi amaliy misol sifatida chiqadi. Backend smart-pointer source yana bitta muhim caveatni qo'shadi: `Box<T>`ning "zero-cost abstraction"ligi allocation yo'q degani emas, stack representation sodda degani.

## Mental Model

`Box<T>` "men bu heap value'ning owneriman" deydi. Box move bo'lsa, heapdagi data odatda joyida qoladi; stackdagi pointer metadata move bo'ladi. Box scope'dan chiqqanda u ko'rsatayotgan heap data ham cleaned up bo'ladi.

```rust
let b = Box::new(5);
```

Bu yerda `b` stackda, `5` heapda.

## Syntax and Examples

Heap allocation:

```rust
fn main() {
    let b = Box::new(5);
    println!("b = {b}");
}
```

Recursive type:

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
```

## Common Mistakes

- `Box<T>` doim performance yutug'i beradi deb o'ylash. Heap allocationning o'zi costga ega; oddiy `i32` uchun box odatda kerak emas.
- `Box<T>` shared ownership beradi deb o'ylash; shared ownership uchun keyin `Rc<T>` ko'riladi.
- Recursive type ichida `Box<T>` nima uchun kerakligini "heapda bo'lgani uchun" deb soddalashtirib yuborish. Muhim nuqta: `Box<T>`ning stackdagi pointer size'i known.

## Related Concepts

- [[smart-pointers]]
- [[stack-and-heap|stack and heap]]
- [[ownership]]
- [[drop|Drop trait]]
- [[deref-trait|Deref trait]]
- [[recursive-types]]
- [[move-semantics|move semantics]]

## Sources

- [[15-1-using-box-t-to-point-to-data-on-the-heap]]
- [[wiki/sources/rust-for-backend-developers-traits]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
