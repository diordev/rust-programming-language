---
title: "Crate Root"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, crates, modules]
source_count: 3
---

# Crate Root

## Short Definition

Crate root compiler crate'ni o'qishni boshlaydigan source file bo'lib, crate'ning root module'ini yaratadi.

## Why It Matters

Module tree qayerdan boshlanishini crate root belgilaydi. Cargo convention orqali `src/main.rs`ni binary crate root, `src/lib.rs`ni library crate root deb oladi.

## Mental Model

- `rustc` crate root file'dan boshlaydi.
- `src/main.rs` -> [[binary-crate|binary crate]] root.
- `src/lib.rs` -> [[library-crate|library crate]] root.
- Root file content'i implicit `crate` module'ini yaratadi.
- Root module qolgan [[module|modules]] va [[module-tree|module tree]] uchun yuqori nuqta bo'ladi.
- Beginner backend source to'g'ridan-to'g'ri `rustc main.rs` misoli bilan shu mental modelni ishlatadi: parent file'ga ulangan modules alohida qo'lda compile qilinmaydi.

## Syntax and Examples

```text
my-project/
  Cargo.toml
  src/
    main.rs    # binary crate root
    lib.rs     # library crate root
```

Cargo bu file'larni `rustc`ga crate root sifatida uzatadi.

```rust
pub mod garden;
```

Crate root ichidagi bunday declaration compilerga `garden` module code'ini crate'ga qo'shishni bildiradi.

## Common Mistakes

- `Cargo.toml` crate root deb o'ylash; u manifest, source root emas.
- Crate root va package root tushunchalarini aralashtirish.
- `src/main.rs` faqat file nomi emas, Cargo convention bo'yicha binary crate root ekanini unutish.
- Crate root implicit `crate` root module'ini yaratishini unutish.

## Related Concepts

- [[crate]]
- [[package]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[module]]
- [[module-tree|module tree]]
- [[cargo|Cargo]]

## Sources

- [[7-1-packages-and-crates]]
- [[7-2-control-scope-and-privacy-with-modules]]
- [[wiki/sources/rust-for-backend-developers-modules]]
