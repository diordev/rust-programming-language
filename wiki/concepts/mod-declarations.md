---
title: "mod Declarations"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules, keywords]
source_count: 1
---

# mod Declarations

## Short Definition

`mod` declaration module'ni crate module tree'iga qo'shadi va, inline body bo'lmasa, compilerga module code'ini qaysi file'lardan qidirishni bildiradi.

## Why It Matters

`mod` va `use`ni ajratish Rust module systemni tushunishda asosiy nuqta: `mod` code'ni crate'ga kiritadi, `use` esa mavjud item'ga qisqa path beradi.

## Mental Model

`mod hosting;` "shu joyda `hosting` child module bor" degani. Declaration qayerda turgani module tree'dagi parent-child munosabatni belgilaydi.

## Syntax and Examples

```rust
mod front_of_house;
```

Crate rootda bu declaration `front_of_house`ni root child module sifatida e'lon qiladi.

```rust
pub mod hosting;
```

`front_of_house` module ichida bu declaration `hosting`ni `front_of_house`ning public child module'i qiladi.

## Common Mistakes

- `mod`ni har bir ishlatadigan file'da qayta yozish kerak deb o'ylash.
- `mod`ni C/C++ `include` kabi textual copy deb tushunish.
- `pub mod` child item'larni ham avtomatik public qiladi deb kutish.

## Related Concepts

- [[module]]
- [[module-tree|module tree]]
- [[module-file-layout|module file layout]]
- [[crate-root|crate root]]
- [[use-declarations|use declarations]]
- [[pub-keyword|pub keyword]]

## Sources

- [[7-5-separating-modules-into-different-files-the-rust-programming-language]]
