---
title: "E0308 mismatched types"
type: error
status: active
created: 2026-05-06
updated: 2026-05-09
tags: [rust, compiler-error, type-system]
source_count: 6
---

# E0308 mismatched types

## Symptom

Compiler `error[E0308]: mismatched types` chiqaradi. Bu bitta umumiy diagnostic: expected type bilan actual type mos kelmaganda paydo bo'ladi.

Guessing game chapterida `String` bilan numberni solishtirishga uringanda shunday xato chiqadi:

```text
expected `&String`, found `&{integer}`
```

## Cause

`guess` user inputdan kelgani uchun `String`, `secret_number` esa numeric type. `cmp` method bir xil yoki mos comparable type kutadi. Rust strong, static type systemga ega, shuning uchun `String` va integer to'g'ridan-to'g'ri solishtirilmaydi.

Chapter 3da shu diagnostic yana uch xil pattern bilan chiqadi:

- `mut` variable typeini `&str`dan `usize`ga o'zgartirishga urinish.
- Return type `i32` bo'lgan functionda final expressionga semicolon qo'yib `()` return qilish.
- `if` conditionga integer berish yoki `if`/`else` arms turli type qaytarishi.

Chapter 10.1da `Point<T>` bitta generic parameter ishlatgani uchun `x` va `y` bir xil concrete type bo'lishi kerak. `Point { x: 5, y: 4.0 }`da compiler `T`ni integer deb infer qiladi, keyin `y` uchun floating-point value kelgani sabab E0308 beradi.

Chapter 20.4da E0308 [[opaque-types|opaque type]] kontekstida chiqadi: ikki function ikkalasi ham `impl Fn(i32) -> i32` qaytarsa ham, har `impl Trait` return joyi alohida concrete type yaratadi. Ularni bitta `Vec` ichida saqlash "expected opaque type, found a different opaque type" xatosiga olib keladi.

## Fix Pattern

Input stringni numeric typega convert qilish:

```rust
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

Yaxshiroq recoverable handling:

```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```

Generic struct field typelari turli bo'lishi kerak bo'lsa, bir nechta type parameter ishlating:

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

let works = Point { x: 5, y: 4.0 };
```

Turli closure typelarini bitta collectionda saqlash kerak bo'lsa, common trait object typega o'ting:

```rust
fn a() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}

fn b(init: i32) -> Box<dyn Fn(i32) -> i32> {
    Box::new(move |x| x + init)
}

let handlers = vec![a(), b(123)];
```

## Minimal Example

Failing idea:

```rust
let guess = String::from("42");
let secret_number = 50;

match guess.cmp(&secret_number) {
    _ => {}
}
```

Fixed idea:

```rust
let guess = String::from("42");
let guess: u32 = guess.trim().parse().expect("Please type a number!");
let secret_number = 50;

match guess.cmp(&secret_number) {
    _ => {}
}
```

Generic struct mismatch:

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let wont_work = Point { x: 5, y: 4.0 };
}
```

## Related Concepts

- [[wiki/sources/2-programming-a-guessing-game]]
- [[3-1-variables-and-mutability]]
- [[3-3-functions]]
- [[3-5-control-flow]]
- [[10-1-generic-data-types]]
- [[wiki/sources/20-4-advanced-functions-and-closures]]
- [[result|Result]]
- [[shadowing]]
- [[type-inference|type inference]]
- [[match]]
- [[generic-structs|generic structs]]
- [[generic-type-parameters|generic type parameters]]
- [[opaque-types|opaque types]]
- [[returning-closures|returning closures]]
