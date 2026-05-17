---
title: "src/bin"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, cargo, binaries, layout]
source_count: 1
---

# src/bin

## Short Definition

`src/bin/` Cargo package ichida qo'shimcha executable'lar uchun ishlatiladigan directory. Undagi har bir `*.rs` file alohida [[binary-crate|binary crate]].

## Why It Matters

Bitta package ichida bir nechta CLI yoki helper executable saqlash mumkin. Shared logic `src/lib.rs`ga chiqarilib, barcha binary'lar bir xil public API'dan foydalanadi.

## Mental Model

- `src/main.rs` -> default binary crate
- `src/bin/tool.rs` -> qo'shimcha binary crate
- `src/lib.rs` -> shared library crate, `src/bin/*.rs` shuni package nomi orqali import qiladi

## Syntax and Examples

```text
my-project/
  src/
    lib.rs
    main.rs
    bin/
      import.rs
      serve.rs
```

```toml
[[bin]]
name = "serve-api"
path = "src/bin/serve.rs"
```

## Common Mistakes

- `src/bin/*.rs` file'lari `src/` module'larini to'g'ridan-to'g'ri `mod` bilan import qila oladi deb o'ylash.
- Qo'shimcha binary logic'ni `src/main.rs`ga yig'ib yuborish.
- `src/bin/` ichidagi file nomi final executable nomi bilan har doim bir xil bo'lishi shart deb o'ylash; TOML `[[bin]]` section bilan override qilish mumkin.

## Related Concepts

- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[package]]
- [[crate]]
- [[cargo|Cargo]]
- [[public-api|public API]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multiple-binaries]]
