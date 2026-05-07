---
title: "rustc"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, tool, compiler]
source_count: 2
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

## Related Pages

- [[1-1-installation-the-rust-programming-language]]
- [[1-2-hello-world-the-rust-programming-language]]
- [[hello-world]]
- [[cargo|Cargo]]
