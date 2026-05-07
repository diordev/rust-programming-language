---
title: "Crate"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, crates, modules]
source_count: 4
---

# Crate

## Short Definition

Crate Rust compiler bir vaqtda ko'rib chiqadigan eng kichik code birligi. U [[binary-crate|binary crate]] yoki [[library-crate|library crate]] bo'lishi mumkin.

## Why It Matters

Crate Rust build modelining markazida turadi: compiler crate'ni compile qiladi, Cargo esa package ichidagi crate root file'larni `rustc`ga uzatadi. Rust ecosystem reusable library crates atrofida qurilgan.

## Mental Model

- [[binary-crate|Binary crate]]: executable programga compile bo'ladi va `main` function talab qiladi.
- [[library-crate|Library crate]]: reusable functionality beradi, executable chiqarmaydi.
- [[crate-root|Crate root]]: compiler boshlaydigan source file.
- Crate ichida [[module|modules]] bo'lishi mumkin; module'lar boshqa file'larda define qilinsa ham crate bilan birga compile qilinadi.
- Rustaceanlar ko'pincha "crate" deganda library crate'ni nazarda tutadi.
- External crate pathlari crate nomidan boshlanadi, masalan `rand::thread_rng()` yoki `std::collections::HashMap`.

## Syntax and Examples

Dependency sifatida ishlatiladigan library crate:

```toml
[dependencies]
rand = "0.8.5"
```

Cargo convention bo'yicha crate roots:

```text
my-project/
  src/
    main.rs    # binary crate root
    lib.rs     # library crate root
```

## Common Mistakes

- Crate bilan module yoki package tushunchalarini aralashtirish.
- External crate qo'shib, kerakli traitni scopega olib kirmaslik.
- Har bir crate executable beradi deb o'ylash.
- [[package]] bir nechta library crate tutishi mumkin deb taxmin qilish.

## Related Concepts

- [[cargo|Cargo]]
- [[package]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[crate-root|crate root]]
- [[module]]
- [[dependencies]]
- [[rand]]
- [[standard-library|standard library]]
- [[semver|SemVer]]

## Sources

- [[2-programming-a-guessing-game-the-rust-programming-language]]
- [[7-packages-crates-and-modules-the-rust-programming-language]]
- [[7-1-packages-and-crates-the-rust-programming-language]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
