---
title: "Opaque Types"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, traits, impl-trait, type-system]
source_count: 1
---

# Opaque Types

## Short Definition

Opaque type - compiler biladigan, lekin caller nomini ko'ra olmaydigan concrete type. Rust'da return position `impl Trait` shunday type yaratadi.

## Why It Matters

`impl Trait` API'da concrete typeni yashirishga yordam beradi, lekin "shu traitni implement qilgan istalgan type" degani emas. Har bir `impl Trait` return joyi bitta maxsus concrete typega bog'lanadi. Bu closure qaytarishda va async kodda muhim.

## Mental Model

```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}
```

Compiler ichkarida closure uchun maxfiy type yaratadi. Caller uning nomini bilmaydi, faqat `Fn(i32) -> i32` sifatida ishlata oladi.

Muhim qoida:

> Distinct uses of `impl Trait` result in different opaque types.

Shuning uchun quyidagi ikki function bir xil ko'rinsa ham, return typelari bir xil emas:

```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}

fn returns_initialized_closure(init: i32) -> impl Fn(i32) -> i32 {
    move |x| x + init
}
```

## Syntax and Examples

### Ishlaydi: bitta opaque type

```rust
fn make_adder() -> impl Fn(i32) -> i32 {
    |x| x + 1
}
```

### Ishlamaydi: ikki opaque type bitta Vec ichida

```rust
let handlers = vec![returns_closure(), returns_initialized_closure(123)];
```

Compiler [[e0308-mismatched-types|E0308]] beradi: expected opaque type, found a different opaque type.

### Yechim: trait object

```rust
fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}

fn returns_initialized_closure(init: i32) -> Box<dyn Fn(i32) -> i32> {
    Box::new(move |x| x + init)
}
```

Endi ikkala function ham bir xil concrete return type qaytaradi: `Box<dyn Fn(i32) -> i32>`.

## Common Mistakes

- Return position `impl Trait`ni trait object deb o'ylash.
- `impl Trait` "har qanday implementor qaytishi mumkin" degani deb tushunish. Function body bitta concrete type qaytarishi kerak.
- Ikki alohida functiondagi `impl Fn(...)` bir xil type deb kutish.
- Branchlarda turli concrete type qaytarish: `if cond { TypeA } else { TypeB }` return `impl Trait` bilan ishlamaydi, agar `TypeA` va `TypeB` bir xil concrete type bo'lmasa.

## Related Concepts

- [[impl-trait|impl Trait]]
- [[returning-closures|returning closures]]
- [[closures]]
- [[trait-object|trait object]]
- [[dynamic-dispatch|dynamic dispatch]]
- [[e0308-mismatched-types|E0308 mismatched types]]
- [[future|Future trait]] - async bloklar ham alohida concrete future type yaratadi

## Sources

- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
