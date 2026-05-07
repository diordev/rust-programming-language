---
title: "Functions"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, functions]
source_count: 3
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

## Common Mistakes

- Parameter type annotationni unutish.
- Return expression oxiriga semicolon qo'yib yuborish.
- Function/variable names uchun snake_case conventionni buzish.
- Duplicate code'ni functionga extract qilmasdan copy-paste qilish.
- Function signature'da ownership/borrowingni keragidan og'ir tanlash.
- Generic type parameterni function name'dan keyin e'lon qilmasdan ishlatish.

## Related Concepts

- [[statements-and-expressions|statements and expressions]]
- [[return-values|return values]]
- [[unit-type|unit type]]
- [[function-extraction|function extraction]]
- [[generic-functions|generic functions]]
- [[generic-type-parameters|generic type parameters]]
- [[code-duplication|code duplication]]
- [[slices]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Sources

- [[3-3-functions-the-rust-programming-language]]
- [[10-generic-types-traits-and-lifetimes-the-rust-programming-language]]
- [[10-1-generic-data-types-the-rust-programming-language]]
