---
title: "1.2. Hello, World! - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, example]
source_count: 1
source_path: "raw/books/the_rust_programming_language/1.2. Hello, World!.md"
source_url: "https://doc.rust-lang.org/stable/book/ch01-02-hello-world.html"
---

# 1.2. Hello, World! - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/1.2. Hello, World!.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch01-02-hello-world.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section birinchi Rust programini qo'lda yozish, `rustc` bilan compile qilish, va executable file'ni ishga tushirishni o'rgatadi.

Rust code qayerda joylashishi Rust uchun qat'iy emas, lekin book mashqlar uchun home directory ichida `projects` folder ochishni tavsiya qiladi. `hello_world` project folderi yaratiladi. Rust source filelar `.rs` extension bilan tugaydi. Bir nechta so'zli filename uchun underscore convention ishlatiladi: `hello_world.rs`.

Minimal program:

```rust
fn main() {
    println!("Hello, world!");
}
```

`main` function har bir executable Rust programda birinchi ishga tushadigan function. Bu function argument olmaydi va return value bermaydi. Function body curly braces `{}` ichida bo'lishi shart; Rustda barcha function bodylar uchun braces kerak.

`println!` oddiy function emas, macro call. `!` belgisi macro chaqirilayotganini ko'rsatadi. Macro Rust syntaxini extend qilish uchun code generation qila oladi; detailed explanation keyinroq macros chapterda beriladi. `"Hello, world!"` string `println!`ga argument sifatida beriladi. Line oxiridagi semicolon expression tugaganini bildiradi.

Compile workflow:

```shell
$ rustc main.rs
$ ./main
Hello, world!
```

Windowsda executable `.\main` orqali ishlatiladi. Compile qilingandan keyin Rust binary executable yaratadi: Linux/macOSda `main`, Windowsda `main.exe`; Windowsda debugging information uchun `.pdb` file ham bo'lishi mumkin.

Rust ahead-of-time compiled language sifatida tushuntiriladi. Program compile qilingandan keyin executable boshqa odamda Rust installed bo'lmasa ham ishlashi mumkin. Bu Ruby/Python/JavaScript kabi dynamic languagesdan farq qiladi, chunki ularda source file ishlashi uchun tegishli runtime/interpreter kerak.

Section oxirida `rustc` oddiy programlar uchun yetarli, lekin project kattalashganda build options, sharing va project organization uchun [[cargo|Cargo]] kerak bo'lishi aytiladi.

## Key Concepts

- [[hello-world]]
- [[main-function|main function]]
- [[println-macro|println! macro]]
- [[macro]]
- [[semicolon]]
- [[rustc]]
- [[binary-executable|binary executable]]
- [[ahead-of-time-compilation|ahead-of-time compilation]]
- [[cargo|Cargo]]

## Code Examples

```rust
fn main() {
    println!("Hello, world!");
}
```

Compile va run:

```shell
$ rustc main.rs
$ ./main
```

## Exercises or Practice Ideas

- `main.rs` yaratib `Hello, world!`ni `rustc` orqali compile qilish.
- `println!`dagi stringni o'zgartirib qayta compile qilish.
- Semicolonni olib tashlab compiler response qanday bo'lishini kuzatish.
- `ls` yoki `dir` orqali source file va executable file farqini ko'rish.

## Questions Raised

- `println!` nima uchun function emas, macro sifatida ishlaydi?
- Rustda expression va statement farqini semicolon qanday ko'rsatadi?
- Ahead-of-time compilation deployment uchun qanday foyda beradi?
- Direct `rustc` workflow qaysi joyda Cargo workflowga almashadi?

## Links To Update

- [[wiki/chapters/1-getting-started|Getting Started]]
- [[hello-world]]
- [[rustc]]
- [[cargo|Cargo]]
- [[macro]]
- [[main-function|main function]]
