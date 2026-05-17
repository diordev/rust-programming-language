---
title: "3.5. Control Flow - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, control-flow]
source_count: 1
source_path: "raw/books/the_rust_programming_language/3.5. Control Flow.md"
source_url: "https://doc.rust-lang.org/stable/book/ch03-05-control-flow.html"
---

# 3.5. Control Flow - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/3.5. Control Flow.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch03-05-control-flow.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section Rustdagi [[control-flow|control flow]]ni tushuntiradi: conditionga qarab branch qilish va code'ni takrorlash. Eng asosiy constructs: `if` expressions va loops.

`if` condition `bool` bo'lishi shart. Rust Ruby/JavaScript kabi non-Boolean qiymatlarni avtomatik true/false ga convert qilmaydi. `if number { ... }` compile bo'lmaydi va `E0308 expected bool, found integer` beradi. Explicit condition kerak: `if number != 0 { ... }`.

`else if` bir nechta conditionni ketma-ket tekshiradi. Birinchi `true` condition topilganda shu branch bajariladi va qolganlari tekshirilmaydi. Juda ko'p `else if` code'ni chigallashtirsa, keyinroq [[match]] yaxshiroq variant bo'lishi mumkin.

`if` expression bo'lgani uchun `let` right side'da ishlatiladi:

```rust
let number = if condition { 5 } else { 6 };
```

Bunda barcha arms bir xil type return qilishi kerak. `if condition { 5 } else { "six" }` compile bo'lmaydi, chunki variable bitta concrete typega ega bo'lishi kerak.

Rustda uchta loop bor: [[loop]], [[while-loop|while]], va [[for-loop|for]]. `loop` infinite loop yaratadi va `break` bilan tugatiladi. `continue` current iterationni tashlab keyingisiga o'tadi. `break value` loopdan value qaytarishi mumkin:

```rust
let result = loop {
    counter += 1;
    if counter == 10 {
        break counter * 2;
    }
};
```

Nested loops uchun loop labels ishlatiladi: label single quote bilan boshlanadi, masalan `'counting_up: loop { ... }`; `break 'counting_up` outer loopdan chiqadi.

`while` condition true bo'lgancha loop qiladi. `loop + if + break` patternini soddalashtiradi. Collection indexing bilan `while` ishlatish xatoga moyil, chunki index va length condition mos kelmasa panic bo'lishi mumkin.

`for` collection elementlari bo'ylab yurish uchun eng common Rust loop construct. Array ustida `for element in a { ... }` bounds mistakesni kamaytiradi va concise. Counted loop uchun range ishlatiladi: `(1..4).rev()` countdown beradi.

Chapter summary practice ideas beradi: Fahrenheit/Celsius conversion, nth Fibonacci number, va "The Twelve Days of Christmas" lyricsni repetitiondan foydalanib chiqarish.

## Key Concepts

- [[control-flow|control flow]]
- [[if-expressions|if expressions]]
- [[boolean-type|bool]]
- [[loop]]
- [[while-loop|while loop]]
- [[for-loop|for loop]]
- [[loop-labels|loop labels]]
- [[range]]
- [[break]]
- [[continue]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Code Examples

`if` in `let`:

```rust
let condition = true;
let number = if condition { 5 } else { 6 };
```

Loop returning value:

```rust
let mut counter = 0;

let result = loop {
    counter += 1;

    if counter == 10 {
        break counter * 2;
    }
};
```

For loop over array:

```rust
let a = [10, 20, 30, 40, 50];

for element in a {
    println!("the value is: {element}");
}
```

## Exercises or Practice Ideas

- Fahrenheit/Celsius converter.
- nth Fibonacci number generator.
- "The Twelve Days of Christmas" lyrics generator.
- `if` arms type mismatch qilib `E0308`ni ko'rish.
- `while` array indexing va `for` array iterationni solishtirish.

## Questions Raised

- Rustda `if` nima uchun faqat `bool` condition oladi?
- `if` expressionning arms type consistency requirementi qanday mental model beradi?
- `for` loop nima uchun collection iterationda `while` indexingdan xavfsizroq?
- `loop`dan value qaytarish qachon foydali?

## Links To Update

- [[wiki/chapters/3-common-programming-concepts|3. Common Programming Concepts]]
- [[control-flow|control flow]]
- [[if-expressions|if expressions]]
- [[loop]]
- [[chapter-3-practice|Chapter 3 practice]]
