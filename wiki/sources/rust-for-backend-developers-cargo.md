---
title: "Cargo - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, cargo]
source_count: 1
---

# Cargo - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/38. Cargo.md`
- URL: https://rust-for-backend-developers.github.io/project/cargo.html

## Detailed Summary

Bu source `rustc` bilan bitta file compile qilish va real Rust project workflow'i orasidagi chiziqni aniq tortadi. Asosiy signal to'g'ri: amaliy project yozishda Cargo oddiy wrapper emas, balki build system, package manager, project layout convention, va ecosystem entry point.

Kirish qismida Cargo nimalarni qilishi sanaladi: project yaratish, binary yoki library build qilish, dependencylarni yuklash va compile qilish, unit va integration testlarni ishlatish, benchmarklarni boshqarish, va ecosystem utility'larni install qilish. Bu beginner uchun muhim, chunki Cargo faqat `cargo run` emas.

`cargo new` sectioni Cargo package layoutini tanishtiradi. `cargo new hello_world --bin` default binary package yaratadi; `--lib` esa library uchun. Bu yerda asosiy durable mental model: `src/main.rs` default binary crate root, `src/lib.rs` esa default library crate root.

`Cargo.toml` bo'limi `[package]` va `[dependencies]`ni ko'rsatadi. Source `name`, `version`, `edition`ni package metadata sifatida beradi. `edition = "2024"` aynan source manifestidagi misol, lekin wiki buni umumiy Cargo qoidasi deb emas, shu source baseline'i sifatida saqlaydi.

Build va run sectioni `cargo build`, `target/debug`, `cargo build --release`, `target/release`, va `cargo run` orasidagi aloqani mustahkamlaydi. Muhim nuqta: Cargo default `dev` profile bilan build qiladi, optimized executable uchun esa explicit `--release` kerak.

Source hali dependency graph, workspace, yoki test organizationni chuqur ochmaydi, lekin keyingi source'lar uchun platforma tayyorlaydi: Rust projectning package manifesti, crate root fayllari, va standard directory layout shu yerda birinchi marta yig'iladi.

## Key Concepts

- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[package]]
- [[crate]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[crate-root|crate root]]

## Code Examples

```shell
cargo new hello_world --bin
```

```toml
[package]
name = "hello_world"
version = "0.1.0"
edition = "2024"

[dependencies]
```

```shell
cargo build
cargo run
cargo build --release
```

## Exercises or Practice Ideas

- `cargo new` bilan bitta binary va bitta library project yarating, keyin tree farqini yozing.
- `target/debug` va `target/release` ichidagi executable nomlarini solishtiring.
- `Cargo.toml`dagi `[package]` va `[dependencies]` sectionlari nimani boshqarishini qisqa ta'riflang.

## Questions Raised

- Cargo qachondan boshlab faqat build tool emas, project orchestration qatlami bo'lib qoladi?
- `src/main.rs` va `src/lib.rs` bir package ichida birga turganda crate modeli qanday ishlaydi?
- `edition` manifest metadata bo'lsa, u syntax va ecosystem compatibility'ga qayerda ta'sir qiladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[package]]
- [[crate]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]

