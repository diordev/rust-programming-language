---
title: "3.4. Comments - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, comments]
source_count: 1
source_path: "raw/books/the_rust_programming_language/3.4. Comments - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch03-04-comments.html"
---

# 3.4. Comments - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/3.4. Comments - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch03-04-comments.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[comments]]ni tushuntiradi. Comments compiler tomonidan ignore qilinadi, lekin code o'qiyotgan odamlar uchun explanation beradi. Rust idiomatic line comment style `//` bilan boshlanadi va line oxirigacha davom etadi.

Multi-line explanation uchun har bir line boshiga `//` yoziladi. Comment code line oxirida ham bo'lishi mumkin, lekin ko'pincha comment code ustidagi alohida line sifatida yoziladi.

Documentation comments alohida tur bo'lib, Chapter 14da crate publishing mavzusida ko'riladi.

## Key Concepts

- [[comments]]
- [[documentation-comments|documentation comments]]
- [[readable-code|readable code]]

## Code Examples

```rust
// hello, world
```

```rust
fn main() {
    // I'm feeling lucky today
    let lucky_number = 7;
}
```

## Exercises or Practice Ideas

- Qisqa code snippetga faqat foydali joyda comment yozish.
- Obvious comment bilan meaningful comment farqini yozib chiqish.
- Chapter 14da documentation comments mavzusiga qaytish.

## Questions Raised

- Rust codebase'da qachon comment kerak, qachon code o'zi yetarli bo'lishi kerak?
- Documentation comments oddiy commentsdan qanday farq qiladi?

## Links To Update

- [[comments]]
- [[functions]]
