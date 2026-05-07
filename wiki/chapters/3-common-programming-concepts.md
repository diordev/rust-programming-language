---
title: "3. Common Programming Concepts"
type: chapter
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, chapter, concepts]
source_count: 6
---

# 3. Common Programming Concepts

## Learning Goal

Rustda hamma programlarda kerak bo'ladigan asosiy building blocksni tushunish: variables, data types, functions, comments, `if`, `loop`, `while`, va `for`.

## Main Ideas

- Rust variables default immutable; `mut` o'zgarish intentini explicit qiladi.
- Constants `const` bilan e'lon qilinadi, type annotation majburiy, va value compile-time constant expression bo'lishi kerak.
- [[shadowing]] yangi binding yaratadi; shu sababli type o'zgarishi mumkin.
- Rust statically typed, lekin compiler ko'p hollarda type inference qiladi.
- Scalar types: integers, floating-point numbers, `bool`, `char`.
- Compound types: tuples va arrays.
- Functions `fn` bilan e'lon qilinadi; parameter typelari majburiy.
- Rust expression-based language: semicolon expressionni statementga aylantiradi.
- `if` condition faqat `bool`; `if` expression bo'lgani uchun `let` ichida value return qilishi mumkin.
- Loop constructs: `loop`, `while`, `for`. Collection iteration uchun `for` odatda eng xavfsiz va idiomatic.

## Concepts

- [[variables-and-mutability|variables and mutability]]
- [[constants]]
- [[shadowing]]
- [[data-types|data types]]
- [[scalar-types|scalar types]]
- [[compound-types|compound types]]
- [[functions]]
- [[statements-and-expressions|statements and expressions]]
- [[comments]]
- [[control-flow|control flow]]
- [[if-expressions|if expressions]]
- [[loop]]

## Examples

Immutable vs mutable:

```rust
let x = 5;
let mut y = 5;
y = 6;
```

Function return expression:

```rust
fn plus_one(x: i32) -> i32 {
    x + 1
}
```

For loop over array:

```rust
let a = [10, 20, 30, 40, 50];

for element in a {
    println!("the value is: {element}");
}
```

## Exercises

- [[chapter-3-practice]]

## Review Questions

- `mut` va [[shadowing]] orasidagi farq nima?
- Constants va immutable variables qanday farq qiladi?
- Type annotation qachon majburiy bo'ladi?
- Tuple va array qanday farq qiladi?
- Function return value nega final expression bo'lishi mumkin?
- `if` expression arms nima uchun bir xil type qaytarishi kerak?
- `for` loop nima uchun array indexing bilan `while`dan xavfsizroq?

## Related Pages

- [[3-common-programming-concepts-the-rust-programming-language]]
- [[3-1-variables-and-mutability-the-rust-programming-language]]
- [[3-2-data-types-the-rust-programming-language]]
- [[3-3-functions-the-rust-programming-language]]
- [[3-4-comments-the-rust-programming-language]]
- [[3-5-control-flow-the-rust-programming-language]]
- [[rust-learning-roadmap]]
