---
title: "Function Pointers"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-17
tags: [rust, functions, closures, ffi]
source_count: 3
---

# Function Pointers

## Short Definition

`fn(...) -> ...` lowercase `fn` bilan yoziladigan concrete type. U oddiy function manzilini value sifatida saqlaydi yoki argument sifatida uzatadi.

## Why It Matters

Function pointer Rust'da functionni callback sifatida uzatishga imkon beradi. Lekin ko'p Rust API'larida `fn` pointer o'rniga `F: Fn(...)` trait bound yaxshi, chunki u named functionlar bilan birga capturing closure'larni ham qabul qiladi.

## Mental Model

`fn(i32) -> i32` - "i32 olib, i32 qaytaradigan function pointer" degani. U closure emas, lekin uchala closure traitni implement qiladi:

- `FnOnce`
- `FnMut`
- `Fn`

Shuning uchun function pointer closure kutilgan joyga kirishi mumkin. Teskarisi doim ham to'g'ri emas: capturing closure `fn` pointerga aylana olmaydi, chunki capture qilingan environmentni saqlashi kerak.

Non-capturing anonymous function ham shu familyaga koerce bo'lishi mumkin:

```rust
let inc: fn(i32) -> i32 = |x| x + 1;
```

Backend beginner generics source `fn() -> R` shaklini yana bir foydali joyda ishlatadi: outer function generic parameter bilan "qandaydir displayable qiymat qaytaradigan function pointer"ni qabul qilish mumkin. Bu named function yoki non-capturing callable uchun ishlaydi; capturing closure kerak bo'lsa odatda `F: Fn...` boundga o'tiladi.

## Syntax and Examples

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

fn main() {
    let answer = do_twice(add_one, 5);
    assert_eq!(answer, 12);
}
```

Moslashuvchanroq Rust API:

```rust
fn do_twice<F>(f: F, arg: i32) -> i32
where
    F: Fn(i32) -> i32,
{
    f(arg) + f(arg)
}
```

Named function:

```rust
fn add_one(x: i32) -> i32 { x + 1 }
assert_eq!(do_twice(add_one, 5), 12);
```

Capturing closure:

```rust
let amount = 3;
assert_eq!(do_twice(|x| x + amount, 5), 16);
```

Generic return type bilan function pointer:

```rust
fn print_produced<R: std::fmt::Display>(f: fn() -> R) {
    println!("{}", f());
}
```

## Common Mistakes

- `fn` va `Fn` bir xil deb o'ylash. `fn` type, `Fn` trait.
- Capturing closure `fn` pointer sifatida o'tadi deb kutish.
- Public Rust API'da keraksiz `fn` ishlatib, closure ishlatish imkonini yopib qo'yish.
- FFI callback contextida closure capture bo'lishini kutish; C odatda closure environmentni bilmaydi.
- `fn() -> R` yozilganda capturing closure ham bemalol kiradi deb o'ylash.

## Related Concepts

- [[functions]]
- [[closures]]
- [[fn-traits|Fn traits]]
- [[returning-closures|returning closures]]
- [[ffi|FFI]]
- [[function-pointer-do-twice]]
- [[trait-bounds|trait bounds]]

## Sources

- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
- [[wiki/sources/rust-for-backend-developers-anonymous-functions]]
- [[wiki/sources/rust-for-backend-developers-generics]]
