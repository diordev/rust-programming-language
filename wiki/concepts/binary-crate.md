---
title: "Binary Crate"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, crates, cargo]
source_count: 2
---

# Binary Crate

## Short Definition

Binary crate executable programga compile bo'ladigan [[crate]]. U ishga tushish nuqtasi sifatida `main` function talab qiladi.

## Why It Matters

Command line tool, server, yoki boshqa run qilinadigan programlar binary crate sifatida build qilinadi. Cargo default `src/main.rs` file'ni package bilan bir xil nomdagi binary crate root'i deb biladi.

## Mental Model

- Binary crate: run qilinadigan output.
- `src/main.rs`: default binary crate root.
- `src/bin/*.rs`: har bir file alohida binary crate.
- `main` function executable ishga tushganda bajariladigan entry point.
- Binary + library package patternida binary crate odatda minimal entry point bo'lib, library crate public API'ni package nomi orqali ishlatadi.

## Syntax and Examples

```text
my-project/
  Cargo.toml
  src/
    main.rs
    bin/
      import.rs
      serve.rs
```

Bu package'da `main.rs`, `import.rs`, va `serve.rs` alohida binary crates bo'ladi.

## Common Mistakes

- Har bir crate executable beradi deb o'ylash; bu faqat binary crate uchun to'g'ri.
- `src/bin/` ichidagi har bir file alohida binary crate ekanini unutish.
- Binary crate `main` function talab qilishini unutish.
- Binary crate package ichidagi library internallariga bemalol kira oladi deb o'ylash; u public API'ni external crate kabi ishlatadi.

## Related Concepts

- [[crate]]
- [[library-crate|library crate]]
- [[package]]
- [[crate-root|crate root]]
- [[main-function|main function]]
- [[binary-executable|binary executable]]
- [[cargo|Cargo]]
- [[public-api|public API]]
- [[api-design|API design]]

## Sources

- [[7-1-packages-and-crates-the-rust-programming-language]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
