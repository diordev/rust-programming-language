---
title: "Rust for Backend Developers: Anonymous Functions"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, closures, chapter]
source_count: 1
---

# Rust for Backend Developers: Anonymous Functions

## Learning Goal

Anonymous function, closure, function pointer, higher-order function, va `Fn`/`FnMut`/`FnOnce` orasidagi farqni bitta amaliy modelga tushirish.

## Main Ideas

- Capture qilmaydigan anonymous function ko'pincha `fn(...) -> ...` [[function-pointers|function pointer]] sifatida ishlaydi.
- Closure outer scope'dan qiymat ushlasa, u oddiy function pointer emas; compiler generated object bo'ladi va `Fn*` traitlardan birini implement qiladi.
- `Fn`, `FnMut`, `FnOnce` classification capture usulidan emas, captured value'ga body qanday muomala qilayotganidan keladi.
- `move` ownershipni closure ichiga o'tkazadi; returned closure yoki thread boundary'larda bu muhim.
- `impl Fn(...)` bitta concrete closure typeni yashiradi; turli closure branchlari kerak bo'lsa `Box<dyn Fn(...)>` ishlatiladi.

## Concepts

- [[closures]]
- [[function-pointers|function pointers]]
- [[higher-order-functions|higher-order functions]]
- [[fn-traits|Fn traits]]
- [[returning-closures|returning closures]]
- [[opaque-types|opaque types]]
- [[impl-trait|impl Trait]]
- [[trait-object|trait object]]
- [[move-semantics|move semantics]]

## Examples

```rust
let inc: fn(i32) -> i32 = |x| x + 1;
```

```rust
fn make_adder(step: i32) -> impl Fn(i32) -> i32 {
    move |x| x + step
}
```

## Exercises

- `fn` pointer qabul qiladigan va `F: Fn` qabul qiladigan ikki API yozib solishtiring.
- `FnMut` stateful counter closure yozing.
- Ikki xil closure'ni `Box<dyn Fn>` orqali bitta `Vec`ga yig'ing.

## Review Questions

- Nega capturing closure `fn` pointerga teng emas?
- `Fn`, `FnMut`, `FnOnce` qaysi signalni beradi?
- `impl Fn` va `Box<dyn Fn>` orasidagi asosiy farq nima?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-anonymous-functions]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[closures]]
- [[function-pointers|function pointers]]
- [[fn-traits|Fn traits]]
