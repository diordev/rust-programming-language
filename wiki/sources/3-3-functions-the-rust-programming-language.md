---
title: "3.3. Functions - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, functions]
source_count: 1
source_path: "raw/books/the_rust_programming_language/3.3. Functions - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch03-03-how-functions-work.html"
---

# 3.3. Functions - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/3.3. Functions - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch03-03-how-functions-work.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section Rustdagi [[functions]]ni tushuntiradi. Function declaration `fn` keyword, function name, parentheses, va body braces bilan yoziladi. Rust convention: function va variable names uchun snake_case ishlatiladi.

`main` ko'p executable programlarda entry point. Boshqa functions source code'da `main`dan oldin yoki keyin yozilishi mumkin; Rust uchun muhim narsa function caller ko'ra oladigan scope'da defined bo'lishi.

Parameters function signature qismidir va har bir parameter type annotationga ega bo'lishi shart:

```rust
fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```

Multiple parameters comma bilan ajratiladi. Parameter va argument terminlari farqlanadi: parameter function definitiondagi variable, argument call paytida berilgan concrete value.

Rust expression-based language. Function body statementsdan iborat bo'ladi va optional final expression bilan tugashi mumkin. Statement action bajaradi va value return qilmaydi. Expression valuega evaluate bo'ladi. `let y = 6;` statement; `6`, `5 + 6`, function call, macro call, block expression value bo'lishi mumkin.

`let x = (let y = 6);` compile bo'lmaydi, chunki `let` statement value return qilmaydi. Block expression:

```rust
let y = {
    let x = 3;
    x + 1
};
```

Bu yerda `x + 1` oxirida semicolon yo'q, shuning uchun block `4` return qiladi. Semicolon expressionni statementga aylantiradi va return value yo'qoladi.

Return value type `->` bilan ko'rsatiladi:

```rust
fn five() -> i32 {
    5
}
```

Rustda function return value odatda function bodydagi final expression. `return` keyword early return uchun mavjud, lekin ko'p Rust code final expressionga tayanadi. Agar `fn plus_one(x: i32) -> i32 { x + 1; }` deb semicolon qo'yilsa, function `()` return qilgan hisoblanadi va `E0308 mismatched types` chiqadi.

## Key Concepts

- [[functions]]
- [[parameters-and-arguments|parameters and arguments]]
- [[statements-and-expressions|statements and expressions]]
- [[return-values|return values]]
- [[unit-type|unit type]]
- [[snake-case|snake case]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Code Examples

Function with parameters:

```rust
fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

Block expression:

```rust
let y = {
    let x = 3;
    x + 1
};
```

Return value:

```rust
fn plus_one(x: i32) -> i32 {
    x + 1
}
```

## Exercises or Practice Ideas

- `another_function`ni `main`dan oldin va keyin yozib ko'rish.
- Parameter typeni olib tashlab compiler errorni ko'rish.
- `plus_one`dagi final expressionga semicolon qo'shib `E0308`ni ko'rish.
- `return` keyword bilan explicit early return example yozish.

## Questions Raised

- Rust nima uchun function parameter typelarini majburiy qiladi?
- Statement va expression farqi `if`, `loop`, function returnlarda qanday ko'rinadi?
- Qachon explicit `return`, qachon final expression ishlatish ma'qul?

## Links To Update

- [[functions]]
- [[statements-and-expressions|statements and expressions]]
- [[e0308-mismatched-types|E0308 mismatched types]]
