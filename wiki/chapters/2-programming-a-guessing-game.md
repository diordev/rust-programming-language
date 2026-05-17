---
title: "2. Programming a Guessing Game"
type: chapter
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, chapter, project]
source_count: 1
---

# 2. Programming a Guessing Game

## Learning Goal

Rustning birinchi project chapteri orqali input, variables, `Result`, external crate, `match`, `loop`, va simple error handlingni amaliy program ichida ko'rish.

## Main Ideas

- Project [[cargo|Cargo]] bilan yaratiladi: `cargo new guessing_game`.
- User input `std::io::stdin().read_line(&mut guess)` orqali olinadi.
- Rust variables default immutable; input buffer uchun `let mut guess = String::new()` kerak.
- `read_line` va `parse` `Result` qaytaradi; errorni e'tiborsiz qoldirish compiler warning yoki runtime panicga olib kelishi mumkin.
- [[rand]] crate standard librarydan tashqi functionality qo'shadi.
- `Cargo.toml` dependency constraintlarni saqlaydi; `Cargo.lock` exact versions bilan reproducible build beradi.
- `cmp` method `Ordering::{Less, Greater, Equal}` qaytaradi.
- `match` variantlarga qarab behavior tanlaydi va barcha casesni ko'rib chiqishga majbur qiladi.
- `loop`, `break`, va `continue` interactive CLI workflow uchun asosiy control flowni beradi.

## Concepts

- [[variables-and-mutability|variables and mutability]]
- [[shadowing]]
- [[result|Result]]
- [[match]]
- [[loop]]
- [[crate]]
- [[dependencies]]
- [[semver|SemVer]]
- [[cargo-lock|Cargo.lock]]
- [[reference]]
- [[mutable-reference|mutable reference]]
- [[ordering|Ordering]]
- [[pattern-matching|pattern matching]]

## Examples

- [[guessing-game]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Exercises

- Final guessing game code'ni `cargo run` bilan ishga tushirish.
- Invalid input berib, program crash qilmasdan qayta prompt berishini tekshirish.
- Guess count qo'shish.
- Secret range'ni o'zgartirish.
- `expect` ishlatilgan parse version bilan `match` ishlatilgan parse version farqini yozib chiqish.

## Review Questions

- `read_line` nima uchun `&mut String` oladi?
- `Result`ning `Ok` va `Err` variantlari nimani anglatadi?
- `guess.trim().parse()` chainidagi har bir method nima qiladi?
- `shadowing` bilan `mut` orasidagi farq nima?
- `Cargo.lock` reproducible buildga qanday yordam beradi?
- `match guess.cmp(&secret_number)`dagi har bir arm qaysi holatni bildiradi?

## Related Pages

- [[wiki/sources/2-programming-a-guessing-game]]
- [[guessing-game]]
- [[rand]]
- [[cargo|Cargo]]
- [[e0308-mismatched-types|E0308 mismatched types]]
- [[rust-learning-roadmap]]
