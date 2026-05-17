---
title: "Пакет, крэйт, модуль - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, package, crate, module]
source_count: 1
---

# Пакет, крэйт, модуль - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/42. Пакет, крэйт, модуль.md`
- URL: https://rust-for-backend-developers.github.io/project/package-crate-module.html

## Detailed Summary

Bu source uchta markaziy tushunchani bitta qisqa ta'rifga yig'adi. Eng muhim signal: `crate` compile unit, `package` Cargo container, `module` esa code organization boundary.

Source crate'ni "kutubxona yoki executable" deb soddalashtiradi. Beginner mental model uchun bu foydali, chunki library crate va binary crate ikkisi ham compiler alohida build qiladigan birliklar.

`package` qismi amaliy Cargo nuqtai nazaridan beriladi: `cargo new` bilan yaratilgan project, `Cargo.toml`dagi `[package]`, va ichida bir yoki bir nechta crate'lar. Bu oldingi source'lardagi `src/lib.rs`, `src/main.rs`, va `src/bin/*.rs` conventions bilan to'g'ridan-to'g'ri ulanadi.

`module` qismi esa compile unit emas, crate ichidagi namespace va code layout qatlami ekanini qayta mustahkamlaydi. `mod name {}` yoki alohida file'dagi module code keyin crate tarkibida compile bo'ladi.

Source juda qisqa bo'lsa ham, package/crate/module chalkashuvini kesib tashlash uchun foydali: `Cargo.toml` package'ga tegishli, `rustc` crate'ni compile qiladi, `mod` esa crate ichidagi code'ni tartiblaydi.

## Key Concepts

- [[package]]
- [[crate]]
- [[module]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]

## Code Examples

```text
package
  Cargo.toml
  src/main.rs
  src/lib.rs
  src/bin/tool.rs
```

## Exercises or Practice Ideas

- Bitta package tree'sida qaysi fayl qaysi crate'ga tegishli ekanini ajratib yozing.
- `module`, `crate`, `package` uchun bittadan bir jumlalik ta'rif yozing.
- `Cargo.toml`, `src/lib.rs`, `src/bin/tool.rs` uchligini qaysi darajalarga tegishli ekanini jadvalga tushiring.

## Questions Raised

- Qachon "crate" deganda aynan library crate nazarda tutiladi, qachon compile unit ma'nosi kerak bo'ladi?
- Module'ni file system papkasi deb tushunish qayerda xato beradi?
- Package va workspace root o'rtasidagi farq qachon muhim bo'lib qoladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[package]]
- [[crate]]
- [[module]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]

