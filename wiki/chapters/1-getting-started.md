---
title: "1. Getting Started"
type: chapter
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, chapter]
source_count: 4
---

# 1. Getting Started

## Learning Goal

Rust development environmentni tayyorlash, birinchi Rust programini `rustc` bilan compile/run qilish, va keyingi projectlar uchun [[cargo|Cargo]] workflowni tushunish.

## Main Ideas

- Rustni recommended install qilish yo'li [[rustup]] orqali latest stable toolchainni o'rnatish.
- Setup to'g'ri bo'lganini `rustc --version` bilan tekshirish mumkin.
- `rustup doc` local API documentationni ochadi; `rustup doc --book` Rust Bookni offline ochadi.
- Minimal executable Rust program `fn main()`dan boshlanadi.
- `println!` macro call bo'lib, `!` belgisi function emas macro ishlatilayotganini bildiradi.
- `rustc main.rs` oddiy file uchun yetarli, lekin real projectlarda Cargo conventionlari kerak.
- Cargo source codeni `src/` ichida saqlaydi, configurationni `Cargo.toml`da boshqaradi, outputni `target/` ichiga yozadi.
- `cargo check` tez compile check, `cargo run` build+run, `cargo build --release` optimized release binary beradi.

## Concepts

- [[rustup]]
- [[rustc]]
- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[main-function|main function]]
- [[println-macro|println! macro]]
- [[macro]]
- [[crate]]
- [[dependencies]]
- [[ahead-of-time-compilation|ahead-of-time compilation]]
- [[release-build|release build]]

## Examples

- [[hello-world]]

Minimal Rust program:

```rust
fn main() {
    println!("Hello, world!");
}
```

Direct compile/run:

```shell
$ rustc main.rs
$ ./main
```

Cargo workflow:

```shell
$ cargo new hello_cargo
$ cd hello_cargo
$ cargo run
```

## Exercises

- `rustc --version` bilan Rust installni tekshirish.
- `main.rs` yaratib `rustc main.rs` bilan compile qilish.
- `cargo new hello_cargo` bilan project structure yaratish.
- `cargo check`, `cargo build`, `cargo run` va `cargo build --release` commandlari natijasini solishtirish.
- `Cargo.toml`dagi `[package]` va `[dependencies]` sectionlarini o'z so'zlaring bilan tushuntirish.

## Review Questions

- `rustup`, `rustc`, va Cargo orasidagi vazifa farqi nima?
- `main` function nima uchun special?
- `println!`dagi `!` nimani bildiradi?
- Rust ahead-of-time compiled language bo'lishi nimani anglatadi?
- `cargo check` nima uchun development paytida foydali?
- `debug` build va `release` build qachon ishlatiladi?

## Related Pages

- [[1-getting-started-the-rust-programming-language]]
- [[1-1-installation-the-rust-programming-language]]
- [[1-2-hello-world-the-rust-programming-language]]
- [[1-3-hello-cargo-the-rust-programming-language]]
- [[rust-learning-roadmap]]
