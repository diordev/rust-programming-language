---
title: "3.1. Variables and Mutability - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, variables]
source_count: 1
source_path: "raw/books/the_rust_programming_language/3.1. Variables and Mutability.md"
source_url: "https://doc.rust-lang.org/stable/book/ch03-01-variables-and-mutability.html"
---

# 3.1. Variables and Mutability - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/3.1. Variables and Mutability.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch03-01-variables-and-mutability.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section Rustda variables default immutable ekanini batafsil tushuntiradi. `let x = 5;` bilan binding yaratilgandan keyin `x = 6;` qilish compile-time error beradi: `error[E0384]: cannot assign twice to immutable variable`. Compiler bu holatda `let mut x = 5;` qilishni taklif qiladi.

Immutability Rustning safety va concurrencyga yo'naltirilgan design nudge'laridan biri. Agar code bir joyda value o'zgarmaydi deb faraz qilsa, boshqa joy uni o'zgartirib yubormasligi compiler tomonidan kafolatlanadi. Bu reasoningni osonlashtiradi va ba'zi bugsni runtimega chiqmasdan oldin topadi.

Mutability kerak bo'lsa, variable nomidan oldin `mut` yoziladi:

```rust
let mut x = 5;
x = 6;
```

`mut` faqat permission emas, balki future readerga intent ham bildiradi: bu value keyin o'zgaradi.

Constants immutable variablesga o'xshaydi, lekin farqlari bor. Constant `const` keyword bilan e'lon qilinadi, `mut` ishlatib bo'lmaydi, type annotation majburiy, va value constant expression bo'lishi kerak. Constant har qanday scope'da, global scope'da ham e'lon qilinishi mumkin. Naming convention: all uppercase with underscores.

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

[[shadowing]] bir xil nomni yangi `let` binding bilan qayta ishlatishdir. Shadowing `mut`dan farq qiladi: `let` bilan yangi variable yaratiladi, shu sababli type ham o'zgarishi mumkin.

```rust
let spaces = "   ";
let spaces = spaces.len();
```

`mut` bilan buni qilib bo'lmaydi, chunki `spaces` avval `&str`, keyin `usize` bo'lishi kerak bo'ladi va Rust variable typeini mutate qilishga ruxsat bermaydi. Bu ham `E0308 mismatched types`ga olib keladi.

## Key Concepts

- [[variables-and-mutability|variables and mutability]]
- [[immutability]]
- [[mut]]
- [[constants]]
- [[shadowing]]
- [[e0384-cannot-assign-twice|E0384 cannot assign twice]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Code Examples

Mutable variable:

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

Constant:

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Shadowing:

```rust
fn main() {
    let x = 5;
    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```

## Exercises or Practice Ideas

- Immutable variablega qayta assignment qilib `E0384`ni ko'rish.
- `mut` qo'shib xatoni tuzatish.
- `const` bilan domain value e'lon qilish.
- Shadowing orqali string lengthni `usize` sifatida saqlash.

## Questions Raised

- Qachon `mut`, qachon [[shadowing]] yaxshiroq?
- Constant expression bilan runtime computed value chegarasi qayerdan o'tadi?
- Default immutability concurrency safetyga qanday yordam beradi?

## Links To Update

- [[variables-and-mutability|variables and mutability]]
- [[shadowing]]
- [[constants]]
- [[e0384-cannot-assign-twice|E0384 cannot assign twice]]
