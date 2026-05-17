---
title: "Несколько исполняемых файлов - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, binaries]
source_count: 1
---

# Несколько исполняемых файлов - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/41. Несколько исполняемых файлов.md`
- URL: https://rust-for-backend-developers.github.io/project/multiple-bin.html

## Detailed Summary

Bu source bitta package ichida bir nechta binary crate bo'lishi mumkinligini amaliy layout bilan ko'rsatadi. Asosiy qoida aniq: default entry point `src/main.rs`, qo'shimcha executable'lar esa `src/bin/*.rs`.

Fibonacci example reusable logic va entry point'larni ajratish uchun ishlatiladi. `src/fibonacci.rs` ichidagi sequence generator bevosita `src/bin/*.rs`dan ko'rinmaydi; ularni umumiy ishlatish uchun `src/lib.rs` orqali public module sifatida expose qilish kerak.

Bu source package ichidagi binary + library patternni mustahkamlaydi. `src/main.rs` ham, `src/bin/fibonacci_sum.rs` ham, `src/bin/fibonacci_prod.rs` ham bir-biriga sibling executable'lar; ularning shared code'ga normal yo'li package nomi bilan library crate'ni import qilish.

Muhim nuance: `mod fibonacci;` yondashuvi `src/main.rs`da ishlashi mumkin, lekin `src/bin/*.rs` fayllariga shu yo'l ko'chmaydi. Durable takeaway shuki, qo'shimcha binaries uchun shared code library crate public API'si orqali chiqishi kerak.

`cargo build --bin fibonacci_sum` specific binary build qilishni ko'rsatadi. TOML `[[bin]]` section esa file nomidan boshqa executable nom berish imkonini beradi. Shunday qilib `src/bin/fibonacci_sum.rs` -> `sum_10` kabi explicit mapping yoziladi.

## Key Concepts

- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[src-bin|src/bin]]
- [[crate]]
- [[package]]
- [[public-api|public API]]

## Code Examples

```rust
pub mod fibonacci;
```

```shell
cargo build --bin fibonacci_sum
```

```toml
[[bin]]
name = "sum_10"
path = "src/bin/fibonacci_sum.rs"
```

## Exercises or Practice Ideas

- Bitta shared `src/lib.rs` va kamida ikkita `src/bin/*.rs` file bilan mini package yarating.
- `src/main.rs`dagi reusable logicni `src/lib.rs`ga ko'chirib, `src/bin/`dan ishlating.
- TOML `[[bin]]` bilan bitta executable nomini source file nomidan mustaqil qiling.

## Questions Raised

- Qachon bitta package ichida bir nechta binary saqlash to'g'ri, qachon workspace yoki alohida package kerak?
- Nega `src/bin/*.rs` shared module'larni to'g'ridan-to'g'ri `mod` bilan emas, `src/lib.rs` orqali ishlatishi kerak?
- CLI tools soni ko'payganda package ichidagi public API design qanchalik muhim bo'ladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[src-bin|src/bin]]
- [[cargo|Cargo]]
- [[package]]
