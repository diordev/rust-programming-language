---
title: "19. Patterns and Matching"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, pattern-matching]
source_count: 1
---

# 19. Patterns and Matching

## Source

- URL: https://doc.rust-lang.org/stable/book/ch19-00-patterns.html
- Raw fayl: `raw/books/the_rust_programming_language/19. Patterns and Matching.md`

## Detailed Summary

Chapter 19 kirish qismi patterns tushunchasini kengroq kontekstda tanishtiradi. [[pattern-matching|Pattern matching]] — Rust'da type strukturasiga mos keluvchi maxsus sintaksis bo'lib, u oddiy va murakkab typelarda ishlaydi.

Pattern tarkibiy qismlari:

- **Literals** — aniq qiymatlar (`5`, `'a'`, `"hello"`)
- **Destructured arrays, enums, structs, tuples** — murakkab typelarni "yechish"
- **Variables** — qiymatni nom bilan bog'lash
- **Wildcards** — `_` orqali qiymatni ignore qilish
- **Placeholders** — `..` orqali qolganlarini o'tkazib yuborish

Misollar: `x`, `(a, 3)`, `Some(Color::Red)`. Bu patternlar data'ning "shaklini" ta'riflaydi. Dastur qiymatni patternlar bilan solishtiradi: mos kelsa belgilangan kod ishlaydi, mos kelmasa ishlamaydi.

Bob 6'dagi `match` misollari (tanga sortlash mashinasi) patternlarni allaqachon ishlatgan. Bu bob esa barcha valid joylari, refutable/irrefutable farqi va sintaksis turlarini yoritadi.

## Key Concepts

- [[pattern-matching|pattern matching]]
- [[match]]
- [[irrefutable-pattern|irrefutable pattern]]
- [[refutable-pattern|refutable pattern]]
- [[pattern-destructuring|pattern destructuring]]

## Code Examples

```rust
// Oddiy misollar
let x = 5;          // x — pattern (irrefutable)
let (a, 3) = ..;    // literal + variable pattern
Some(Color::Red)    // enum variant destructuring
```

## Exercises or Practice Ideas

- Qanday pattern komponentlarini eslaysiz? Ularni misol bilan yozing.
- `match` va `if let`dagi patternlarni solishtiring.

## Questions Raised

- Refutable va irrefutable patternlar qaysi kontekstlarda qo'llanadi? (19.2 da yoritiladi)
- Pattern sintaksisi qanday variantlari bor? (19.3 da yoritiladi)

## Links To Update

- [[wiki/chapters/19-patterns-and-matching]] — chapter sahifasi
- [[pattern-matching]] — source qo'shish
