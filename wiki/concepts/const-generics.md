---
title: "Const Generics"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, generics, const]
source_count: 1
---

# Const Generics

## Short Definition

`const generics` generic parameter sifatida compile-time constant qabul qiladigan Rust mexanizmi. Syntax odatda `<const N: usize>` ko'rinishida bo'ladi.

## Why It Matters

Ba'zi typelar faqat data turi bilan emas, compile-time shape bilan ham farqlanadi. Array buning eng oddiy misoli: `[T; 3]` va `[T; 5]` boshqa-boshqa type. `const generics` shu shape'ni API contractning bir qismiga aylantiradi.

## Mental Model

Oddiy `T` generic "qaysi type?" degan savolga javob beradi. `const N: usize` esa "qaysi compile-time son?" degan savolga javob beradi. Ikkalasi birga ishlashi mumkin:

```rust
fn make_array<T: Copy, const SIZE: usize>(init_value: T) -> [T; SIZE]
```

Bu yerda `T` element type, `SIZE` esa array uzunligi. Ikkalasi ham compile time'da ma'lum bo'lishi kerak.

## Syntax and Examples

Array type:

```rust
let arr: [i32; 3] = [1, 2, 3];
```

Const generic helper:

```rust
fn make_array<T: Copy, const SIZE: usize>(init_value: T) -> [T; SIZE] {
    [init_value; SIZE]
}

let arr: [i32; 5] = make_array::<i32, 5>(1);
```

Const generic qiymatni body ichida ishlatish:

```rust
fn print_times<const QUANTITY: usize>(v: impl std::fmt::Display) {
    for _ in 0..QUANTITY {
        println!("{v}");
    }
}
```

## Common Mistakes

- `const generics` runtime value qabul qiladi deb o'ylash. Yo'q, compile-time constant kerak.
- `[init_value; SIZE]` uchun `T: Copy` bound nega kerakligini e'tiborsiz qoldirish.
- `[T; 3]` va `[T; 4]` bir xil type familyasi deb o'ylash.
- `turbofish` ishlatilgan joyda type argument va const argument tartibini chalkashtirish.

## Related Concepts

- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[trait-bounds|trait bounds]]
- [[turbofish]]
- [[array]]
- [[copy-trait|Copy trait]]
- [[type-inference|type inference]]

## Sources

- [[wiki/sources/rust-for-backend-developers-generics]]
