---
title: "15.2. Treating Smart Pointers Like Regular References"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, deref, references]
source_count: 1
---

# 15.2. Treating Smart Pointers Like Regular References

## Source

- Manba: The Rust Programming Language, 15.2
- URL: https://doc.rust-lang.org/stable/book/ch15-02-deref.html
- Raw fayl: `raw/books/the_rust_programming_language/15.2. Treating Smart Pointers Like Regular References.md`

## Detailed Summary

15.2 [[deref-trait|Deref trait]] smart pointerlarni oddiy [[reference|references]] kabi ishlatishga qanday yordam berishini tushuntiradi. Oddiy reference uchun `*` [[dereference-operator|dereference operator]] value'ga yetib boradi:

```rust
let x = 5;
let y = &x;

assert_eq!(5, x);
assert_eq!(5, *y);
```

`assert_eq!(5, y)` ishlamaydi, chunki `i32` bilan `&i32` bir xil type emas. Bu [[e0277-trait-bound-not-satisfied|E0277]] ko'rinishida chiqishi mumkin.

`Box<T>` ham `*` bilan reference kabi ishlaydi:

```rust
let x = 5;
let y = Box::new(x);

assert_eq!(5, x);
assert_eq!(5, *y);
```

Keyin bob `MyBox<T>` nomli tuple struct yasaydi:

```rust
struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}
```

Bu type hali dereference qilinmaydi. `*y` yozilsa [[e0614-type-cannot-be-dereferenced|E0614]] chiqadi. `Deref` implement qilingandan keyin compiler `*y`ni `*(y.deref())` kabi tushunadi:

```rust
use std::ops::Deref;

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```

`deref` value'ni emas, reference qaytaradi. Agar value qaytarsa, inner value `self`dan move bo'lib ketardi.

Bolimning ikkinchi katta mavzusi [[deref-coercions|deref coercions]]. Function yoki method parameteri `&str` kutsa, Rust `&MyBox<String>`ni compile time'da `&String`, keyin `&str`ga coerce qila oladi:

```rust
fn hello(name: &str) {
    println!("Hello, {name}!");
}

let m = MyBox::new(String::from("Rust"));
hello(&m);
```

Deref coercion runtime cost keltirmaydi: compiler kerakli `Deref::deref` chaqiruvlarini compile time'da joylaydi.

Mutable references uchun [[deref-mut-trait|DerefMut trait]] ishlatiladi. Rust uchta coercion holatini qo'llaydi:

1. `&T` -> `&U` agar `T: Deref<Target = U>`
2. `&mut T` -> `&mut U` agar `T: DerefMut<Target = U>`
3. `&mut T` -> `&U` agar `T: Deref<Target = U>`

Ammo `&T` -> `&mut U` yo'q: immutable reference'dan mutable reference yasash borrowing qoidalarini buzishi mumkin.

## Key Concepts

- [[dereference-operator]]
- [[deref-trait|Deref trait]]
- [[deref-mut-trait|DerefMut trait]]
- [[deref-coercions]]
- [[box-t|Box<T>]]
- [[reference]]
- [[smart-pointers]]
- [[e0614-type-cannot-be-dereferenced|E0614 type cannot be dereferenced]]

## Code Examples

Custom `MyBox<T>`:

```rust
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```

Deref coercion:

```rust
fn hello(name: &str) {
    println!("Hello, {name}!");
}

let m = MyBox::new(String::from("Rust"));
hello(&m);
```

## Exercises or Practice Ideas

- `MyBox<T>`ni `Deref` implementationsiz compile qilib, E0614 errorni o'qing.
- `hello(&m)` o'rniga `hello(&(*m)[..])` yozib ko'ring; deref coercion qanday boilerplateni yashirayotganini tushuntiring.
- Mutable reference coercion uch qoidasi uchun kichik jadval tuzing.

## Questions Raised

- Nega `Deref::deref` `&Self::Target` qaytaradi, `Self::Target` emas?
- Deref coercion borrowing rulesni qanday saqlaydi?
- Qachon custom smart pointer uchun `Deref` implement qilish API'ni yaxshilaydi, qachon yashirin behaviorni ko'paytiradi?

## Links To Update

- [[wiki/chapters/15-smart-pointers]]
- [[deref-trait]]
- [[deref-mut-trait]]
- [[deref-coercions]]
- [[dereference-operator]]
- [[mybox-deref]]
- [[e0614-type-cannot-be-dereferenced]]
