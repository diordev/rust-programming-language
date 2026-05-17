---
title: "rustc"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, tool, compiler]
source_count: 5
---

# rustc

## Short Definition

`rustc` Rust compiler. U `.rs` source filelarni compile qilib binary executable yaratadi.

## Common Commands

```shell
$ rustc --version
$ rustc main.rs
$ ./main
```

## Notes

- `rustc --version` Rust install to'g'ri ishlayotganini tekshirish uchun birinchi command.
- `rustc main.rs` oddiy single-file program uchun yetarli.
- Project kattalashganda build options, dependencylar va project structure uchun [[cargo|Cargo]] ishlatiladi.
- Rust ahead-of-time compiled language: compile qilingan executable boshqa machine'da Rust installed bo'lmasa ham ishlashi mumkin.
- `rustc` Rust code'ni compile qiladi, lekin final executable buildida [[linker]] va platform libraries ham qatnashishi mumkin.

## Related Pages

- [[1-1-installation]]
- [[1-2-hello-world]]
- [[hello-world]]
- [[cargo|Cargo]]
- [[linker]]
- [[c-toolchain|C/C++ toolchain]]

## Sources

- [[1-1-installation]]
- [[1-2-hello-world]]
- [[wiki/sources/rust-for-backend-developers-setup-rust]]
- [[wiki/sources/rust-for-backend-developers-install-on-linux]]
- [[wiki/sources/rust-for-backend-developers-first-look]]
