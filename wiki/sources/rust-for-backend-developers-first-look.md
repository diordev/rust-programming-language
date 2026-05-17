---
title: "Первый взгляд - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, beginner, source]
source_count: 1
---

# Первый взгляд - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/1. intro/6. Первый взгляд.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/first-look.html

## Detailed Summary

Bu source Rustdagi minimal "Hello World" programni ko'rsatadi. Syntax C-like: function body curly braces bilan o'raladi, expression statement semicolon bilan tugaydi, program execution [[main-function|main function]]dan boshlanadi. Output uchun [[println-macro|println! macro]] ishlatiladi.

Source `rustc main.rs` orqali single-file programni compile qilishni ko'rsatadi. Compiler executable yaratadi: Windowsda `main.exe`, Linuxda `main`. Bu Cargo workflowidan oldingi eng sodda compiler flow bo'lib, `rustc` nima qilishini ko'rish uchun foydali.

Run command OSga qarab farqlanadi. Linuxda current directory executable `./main` bilan ishga tushiriladi. Windowsda source misolida `main` deb ko'rsatilgan; PowerShellda ko'pincha `.\main` ko'rinishi ishlatiladi. Wiki bu farqni existing [[hello-world]] example bilan bog'laydi.

## Key Concepts

- [[hello-world]]
- [[main-function]]
- [[println-macro|println! macro]]
- [[rustc]]
- [[semicolon]]
- [[binary-executable|binary executable]]

## Code Examples

```rust
// Bu comment
fn main() {
    println!("Hello world!");
}
```

Compile:

```shell
rustc main.rs
```

Linux run:

```shell
./main
Hello world!
```

Windows run:

```powershell
.\main
Hello world!
```

## Exercises or Practice Ideas

- `main.rs` yaratib, `rustc main.rs` bilan Cargo ishlatmasdan compile qiling.
- `println!`dagi textni o'zgartirib, executable qayta build qilinmaguncha output o'zgarmasligini kuzating.

## Questions Raised

- `rustc` bilan single-file compile qilish qachon foydali, qachon [[cargo|Cargo]]ga o'tish kerak?
- Macro call sintaksisidagi `!` yangi o'quvchi uchun nimani bildiradi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-first-look]]
- [[wiki/chapters/rust-for-backend-developers-1-intro]]
- [[hello-world]]
- [[rustc]]
- [[main-function]]
