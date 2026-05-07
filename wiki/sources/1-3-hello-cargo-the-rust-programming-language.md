---
title: "1.3. Hello, Cargo! - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, cargo]
source_count: 1
source_path: "raw/books/the_rust_programming_language/1.3. Hello, Cargo! - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch01-03-hello-cargo.html"
---

# 1.3. Hello, Cargo! - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/1.3. Hello, Cargo! - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch01-03-hello-cargo.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[cargo|Cargo]]ni Rustning build system va package manageri sifatida tanishtiradi. Cargo code build qilish, dependencylarni download qilish, va dependency librariesni build qilish kabi ishlarni boshqaradi.

Oddiy `Hello, world!` programida dependencies yo'q, lekin project murakkablashsa dependency qo'shish va build processni boshqarish uchun Cargo kerak bo'ladi. Rust projectlarining katta qismi Cargo ishlatgani uchun bookning qolgan qismi ham Cargo workflowga tayanadi.

Cargo borligini tekshirish:

```shell
$ cargo --version
```

Yangi project yaratish:

```shell
$ cargo new hello_cargo
$ cd hello_cargo
```

`cargo new` project directory, `Cargo.toml`, `src/main.rs`, `.gitignore`, va default holatda Git repository yaratadi. Agar existing Git repository ichida `cargo new` ishlatilsa, Git files generatsiya qilinmaydi. `--vcs` flag version control behaviorni sozlaydi.

`Cargo.toml` TOML formatidagi Cargo configuration file. Minimal generated file:

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2024"

[dependencies]
```

`[package]` section package metadata beradi: name, version, Rust edition. `[dependencies]` section project ishlatadigan crates ro'yxati uchun. Rustda code packages `crates` deb ataladi.

Cargo convention: source files `src/` ichida bo'ladi, top-level directory esa README, license, config va boshqa non-code project files uchun. Oldin Cargo'siz boshlangan projectni Cargo projectga o'tkazish uchun code `src/`ga ko'chiriladi va `Cargo.toml` yaratiladi; buni `cargo init` osonlashtiradi.

Build va run workflow:

```shell
$ cargo build
$ ./target/debug/hello_cargo
```

Default build debug profile bo'ladi va binary `target/debug/` ichida yaratiladi. Birinchi `cargo build` `Cargo.lock` file ham yaratadi; u dependencylarning exact versionsini saqlaydi va qo'lda edit qilinmaydi.

`cargo run` compile va runni bitta commandga birlashtiradi:

```shell
$ cargo run
```

Source o'zgarmagan bo'lsa, Cargo qayta compile qilmaydi, existing binaryni ishga tushiradi. Source o'zgarsa, oldin rebuild qiladi.

`cargo check` code compile bo'lishini tez tekshiradi, lekin executable yaratmaydi. U development paytida tez feedback olish uchun `cargo build`dan foydaliroq bo'lishi mumkin.

Release build:

```shell
$ cargo build --release
```

Bu optimized binaryni `target/release/` ichida yaratadi. Compile vaqti uzunroq, lekin runtime tezroq. Benchmark qilganda release build executable bilan o'lchash kerak.

Section summarysi Chapter 1da o'rganilgan narsalarni jamlaydi: Rustni `rustup` bilan o'rnatish, update qilish, local docs ochish, `rustc` bilan `Hello, world!` yozish/run qilish, va Cargo conventionlari bilan yangi project yaratish/run qilish. Keyingi tabiiy qadam Chapter 2dagi guessing game project yoki oldin common programming concepts uchun Chapter 3.

## Key Concepts

- [[cargo|Cargo]]
- [[package-manager|package manager]]
- [[build-system|build system]]
- [[cargo-toml|Cargo.toml]]
- [[crate]]
- [[dependencies]]
- [[cargo-lock|Cargo.lock]]
- [[debug-build|debug build]]
- [[release-build|release build]]
- [[cargo-check|cargo check]]
- [[cargo-run|cargo run]]

## Code Examples

```shell
$ cargo new hello_cargo
$ cd hello_cargo
$ cargo build
$ cargo run
$ cargo check
$ cargo build --release
```

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2024"

[dependencies]
```

## Exercises or Practice Ideas

- `cargo new hello_cargo` bilan project yaratish va generated structure'ni tekshirish.
- `cargo build`, `cargo run`, `cargo check` commandlarini ketma-ket ishlatib farqini yozish.
- `Cargo.toml`dagi `name`, `version`, `edition`, `[dependencies]` ma'nosini tushuntirib berish.
- `cargo build --release` bilan `target/debug` va `target/release` outputlarini solishtirish.

## Questions Raised

- `Cargo.lock` qachon commit qilinadi va qachon qilinmaydi?
- `cargo check` qaysi holatlarda `cargo build` o'rnini bosa olmaydi?
- `debug` va `release` profiles orasidagi optimization farqi nima?
- Cargo conventions real-world Rust project organizationni qanday standartlashtiradi?

## Links To Update

- [[1-getting-started|Getting Started]]
- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[crate]]
- [[dependencies]]
- [[release-build|release build]]
