---
title: "Создание библиотеки - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, library]
source_count: 1
---

# Создание библиотеки - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/40. Создание библиотеки.md`
- URL: https://rust-for-backend-developers.github.io/project/making-lib.html

## Detailed Summary

Bu source library crate yaratishni binary package'dan ajratib ko'rsatadi. `cargo new my_lib --lib` default `src/lib.rs` bilan boshlanadi va Cargo package ichida reusable functionality uchun alohida crate root paydo bo'ladi.

`[lib] crate-type = ["lib"]` bo'limi Rust library output turlarini sanaydi: `lib`, `rlib`, `dylib`, `cdylib`, `staticlib`, `proc-macro`. Beginner uchun durable takeaway shuki, odatiy Rust-to-Rust reuse holatida `lib` normal default model.

Source exported functionlar `pub` bo'lishi kerakligini to'g'ri aytadi. Shu bilan library crate public API'si private helperlardan ajraladi.

Code block ichida clipping artifact bor: `pub fn sum2` misoli atrofida keraksiz `fn main()` wrapper paydo bo'lgan. Wiki buni literal qabul qilmaydi; library example'ning to'g'ri shakli standalone `pub fn sum2(...) -> ...` function.

Path dependency qismi amaliy jihatdan muhim. Cargo dependency'ni faqat crates.io'dan emas, `git`, `path`, va alternate registry'dan ham ola oladi. `my_lib = { path = "../my_lib" }` misoli local library'ni boshqa package'da ishlatishning minimal ko'rinishini beradi.

Run output misolida yana bitta xato bor: `println!("sum2(1,2)={}", sum2(1, 1));` natijasi `2` chiqadi. Demak matndagi `sum2(1,2)` yozuvi claim emas, typo. Wiki bunu tuzatilgan holda eslatadi, lekin source oqimini saqlaydi.

## Key Concepts

- [[library-crate|library crate]]
- [[crate]]
- [[package]]
- [[cargo-toml|Cargo.toml]]
- [[dependencies]]
- [[public-api|public API]]

## Code Examples

```shell
cargo new my_lib --lib
```

```toml
[lib]
crate-type = ["lib"]
```

```toml
[dependencies]
my_lib = { path = "../my_lib" }
```

## Exercises or Practice Ideas

- `--lib` bilan package yarating va `src/lib.rs` ichida bitta `pub fn` yozing.
- Shu library'ni alohida binary package'dan `path` dependency sifatida ulang.
- `pub`ni olib tashlab, library item tashqaridan ko'rinmasligini compiler orqali tekshiring.

## Questions Raised

- `crate-type` qachon default `lib`dan boshqa variantni talab qiladi?
- Reusable logicni library crate'ga ajratish qachon design yutug'i, qachon ortiqcha?
- Local `path` dependency va workspace member o'rtasidagi amaliy farq nima?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[library-crate|library crate]]
- [[package]]
- [[crate]]
- [[cargo-toml|Cargo.toml]]
- [[dependencies]]

