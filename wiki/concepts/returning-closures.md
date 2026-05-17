---
title: "Returning Closures"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-17
tags: [rust, closures, traits, impl-trait]
source_count: 2
---

# Returning Closures

## Short Definition

Functiondan closure qaytarish patterni. Bitta closure implementation qaytarilsa `impl Fn(...) -> ...`, turli closure typelarini bir xil return type ostida birlashtirish kerak bo'lsa `Box<dyn Fn(...) -> ...>` ishlatiladi.

## Why It Matters

Closure'lar ko'pincha callback, handler, adapter, yoki configured computation sifatida qaytariladi. Rust'da closure'ning concrete tipi odatda nomlanmaydi, shuning uchun return type tanlovi API design va performancega ta'sir qiladi.

## Mental Model

Closure - compiler yaratadigan maxfiy struct + `Fn*` trait implementation. Captured qiymatlar shu struct fieldlari kabi saqlanadi. Siz closure typeni nomlay olmaysiz, lekin trait orqali contract yozasiz:

- `impl Fn(i32) -> i32` - "aniq, lekin nomi yashirilgan bitta concrete type".
- `Box<dyn Fn(i32) -> i32>` - "heapdagi trait object; runtime dispatch orqali chaqiriladi".

Non-capturing callable uchun undan ham sodda variant bor: plain `fn(i32) -> i32` qaytarish. Lekin outer scope'dan qiymat ushlaydigan closure plain `fn` bo'la olmaydi.

## Syntax and Examples

### Bitta closure qaytarish

```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}

let f = returns_closure();
assert_eq!(f(5), 6);
```

### Non-capturing callable'ni `fn` sifatida qaytarish

```rust
fn create_inc() -> fn(i32) -> i32 {
    |x| x + 1
}
```

### Capture bilan closure qaytarish

```rust
fn make_adder(init: i32) -> impl Fn(i32) -> i32 {
    move |x| x + init
}

let add_10 = make_adder(10);
assert_eq!(add_10(5), 15);
```

`move` `init`ni closure ichiga ko'chiradi; aks holda returned closure local variable'ga reference saqlab qolishi mumkin bo'lardi.

### Turli closure typelari uchun trait object

```rust
fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}

fn returns_initialized_closure(init: i32) -> Box<dyn Fn(i32) -> i32> {
    Box::new(move |x| x + init)
}

let handlers = vec![returns_closure(), returns_initialized_closure(123)];
for handler in handlers {
    println!("{}", handler(5));
}
```

## Common Mistakes

- `impl Fn` qaytaradigan ikki function bir xil return type qaytaradi deb o'ylash.
- Capture qilgan closure qaytarishda `move`ni unutish.
- `Box<dyn Fn>` kerak joyda `impl Fn` ishlatib, heterogeneous collection yaratishga urinish.
- `Fn`, `FnMut`, `FnOnce` tanlovini capture behavior bilan bog'lamaslik.

## Related Concepts

- [[closures]]
- [[fn-traits|Fn traits]]
- [[impl-trait|impl Trait]]
- [[opaque-types|opaque types]]
- [[trait-object|trait object]]
- [[box-t|Box<T>]]
- [[move-semantics|move semantics]]
- [[boxed-closure-handlers]]

## Sources

- [[wiki/sources/rust-for-backend-developers-anonymous-functions]]
- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
