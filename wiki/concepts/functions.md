---
title: "Functions"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, functions]
source_count: 5
---

# Functions

## Short Definition

Function reusable code block. Rustda function `fn` keyword bilan e'lon qilinadi.

## Why It Matters

Rust code functions atrofida tashkil qilinadi. `main` executable program entry pointi, boshqa functions esa logicni bo'laklarga ajratadi.

## Mental Model

```rust
fn function_name(parameter: Type) -> ReturnType {
    expression
}
```

Parameter typelari majburiy. Return type `->` bilan beriladi. Return value odatda bodydagi final expression.

Chapter 10 functionlarni duplicationni kamaytirish abstractioni sifatida ishlatadi. Repeated logic alohida function body'ga extract qilinadi, inputs va output esa signature'da ko'rsatiladi.

Chapter 10.1 [[generic-functions|generic functions]]ni ko'rsatadi: type parameter function nomidan keyin `<>` ichida e'lon qilinadi, masalan `fn largest<T>(list: &[T]) -> &T`.

Chapter 20.4 functionni value sifatida ham ko'rsatadi: named function `fn(i32) -> i32` kabi [[function-pointers|function pointer]]ga coerce bo'lishi va boshqa functionga argument sifatida berilishi mumkin.

Backend beginner source functionlarning yana bir nechta amaliy qirrasini qo'shadi: inner helper functions, `const fn`, va function ichidagi `static mut`ning unsafe tabiati.

## Syntax and Examples

```rust
fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```

Extracted function:

```rust
fn largest(list: &[i32]) -> &i32 {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

Generic function:

```rust
fn largest<T: std::cmp::PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

Function pointer:

```rust
fn add_one(x: i32) -> i32 { x + 1 }

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}
```

Inner helper:

```rust
fn outer(index: usize) -> u32 {
    fn inner(x0: u32, x1: u32) -> u32 {
        x0 + x1
    }
    inner(index as u32, 1)
}
```

Const function:

```rust
const fn double(num: f32) -> f32 {
    num * 2.0
}
```

## Common Mistakes

- Parameter type annotationni unutish.
- Return expression oxiriga semicolon qo'yib yuborish.
- Function/variable names uchun snake_case conventionni buzish.
- Duplicate code'ni functionga extract qilmasdan copy-paste qilish.
- Function signature'da ownership/borrowingni keragidan og'ir tanlash.
- Generic type parameterni function name'dan keyin e'lon qilmasdan ishlatish.
- `fn` function pointer va `Fn` closure traitini bir xil deb o'ylash.

## Related Concepts

- [[statements-and-expressions|statements and expressions]]
- [[return-values|return values]]
- [[unit-type|unit type]]
- [[function-extraction|function extraction]]
- [[function-pointers|function pointers]]
- [[generic-functions|generic functions]]
- [[generic-type-parameters|generic type parameters]]
- [[code-duplication|code duplication]]
- [[slices]]
- [[e0308-mismatched-types|E0308 mismatched types]]
- [[static-items]]
- [[unsafe-rust|unsafe Rust]]
- [[never-type|never type (!)]]

## Sources

- [[3-3-functions]]
- [[wiki/sources/10-generic-types-traits-and-lifetimes]]
- [[10-1-generic-data-types]]
- [[wiki/sources/20-4-advanced-functions-and-closures]]
- [[wiki/sources/rust-for-backend-developers-functions]]
