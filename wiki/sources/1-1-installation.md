---
title: "1.1. Installation - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, setup]
source_count: 1
source_path: "raw/books/the_rust_programming_language/1.1. Installation.md"
source_url: "https://doc.rust-lang.org/stable/book/ch01-01-installation.html"
---

# 1.1. Installation - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/1.1. Installation.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch01-01-installation.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section Rustni o'rnatishning recommended yo'lini tushuntiradi. Rust [[rustup]] orqali o'rnatiladi; `rustup` Rust versions va associated toolsni boshqaradigan command line tool. Download uchun internet kerak.

Book latest stable Rust compilerni o'rnatishni tavsiya qiladi. Rust stability guarantees sababli kitobdagi compile bo'ladigan examplelar yangi stable Rust versiyalarida ham compile bo'lishi kerak. Output, ayniqsa error messages va warnings, versiyadan versiyaga biroz farq qilishi mumkin, chunki Rust diagnostics yaxshilanib boradi.

Command line notation tushuntiriladi: terminalga kiritiladigan commandlar `$` bilan ko'rsatiladi, lekin `$`ning o'zi typed qilinmaydi. Output line odatda `$`siz beriladi. PowerShell examplelari `>` bilan ko'rsatilishi mumkin.

Linux va macOS uchun install command:

```shell
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

Install muvaffaqiyatli bo'lsa, Rust installed ekanini bildiruvchi message chiqadi. Rust compiled outputlarni bitta executablega ulash uchun linker kerak bo'ladi. Linker errorlari bo'lsa, odatda C compiler o'rnatish kerak: macOSda `xcode-select --install`, Linuxda distro documentation bo'yicha GCC yoki Clang, Ubuntu uchun `build-essential`.

Windowsda Rust install sahifasi orqali o'rnatiladi va Visual Studio linker hamda native libraries uchun kerak bo'lishi mumkin.

Install tekshiruvi:

```shell
$ rustc --version
```

Output `rustc x.y.z (abcabcabc yyyy-mm-dd)` formatida bo'lsa, Rust to'g'ri o'rnatilgan. Aks holda `%PATH%`, PowerShellda `$env:Path`, Linux/macOSda `$PATH` tekshiriladi.

Rustni yangilash:

```shell
$ rustup update
```

Rust va `rustup`ni o'chirish:

```shell
$ rustup self uninstall
```

Local documentation `rustup doc` bilan browserda ochiladi. Standard librarydagi type yoki function nima qilishini bilmasangiz, API documentationdan foydalanish kerak.

Offline ishlash uchun book keyingi dependencylarni oldindan cache qilishni taklif qiladi:

```shell
$ cargo new get-dependencies
$ cd get-dependencies
$ cargo add rand@0.8.5 trpl@0.2.0
```

Shundan keyin keyingi Cargo commandlarda `--offline` ishlatib cached versionsdan foydalanish mumkin.

## Key Concepts

- [[rustup]]
- [[rustc]]
- [[linker]]
- [[c-compiler|C compiler]]
- [[path-environment-variable|PATH]]
- [[standard-library|standard library]]
- [[cargo|Cargo]]
- [[offline-builds|offline builds]]

## Code Examples

Bu source Rust code bermaydi, lekin setup commandlarini beradi:

```shell
$ rustc --version
$ rustup update
$ rustup doc
```

## Exercises or Practice Ideas

- `rustc --version` outputini yozib qo'yish.
- `rustup doc` orqali local documentationni ochib ko'rish.
- Agar offline ishlash kerak bo'lsa, `rand@0.8.5` va `trpl@0.2.0` dependencylarini oldindan cache qilish.
- Linker/C compiler borligini oddiy Rust compile orqali tekshirish.

## Questions Raised

- Rust stability guarantees aynan qaysi compatibilityni va'da qiladi?
- Linker errorlarini Rust beginner qanday tanishi va tuzatishi kerak?
- `rustup doc` bilan `rustup doc --book` farqi nimada?
- `--offline` workflow qachon foydali bo'ladi?

## Links To Update

- [[wiki/chapters/1-getting-started|Getting Started]]
- [[rustup]]
- [[rustc]]
- [[cargo|Cargo]]
- [[standard-library|standard library]]
