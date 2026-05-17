---
title: "Rust for Backend Developers: Install on Windows"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, windows, setup, chapter]
source_count: 1
---

# Rust for Backend Developers: Install on Windows

## Learning Goal

Windowsda Rust uchun MSVC va GNU targetlar orasidagi farqni va install smoke testni tushunish.

## Main Ideas

- Tavsiya qilingan default yo'l - [[msvc-toolchain|MSVC toolchain]].
- Alternative yo'l - [[mingw|MinGW]] yoki MSYS2 orqali GNU toolchain.
- `rustup-init.exe` target tanlovida `x86_64-pc-windows-msvc` yoki `x86_64-pc-windows-gnu` farq qiladi.
- `cargo new`, `cargo run` install verification uchun ishlatiladi.

## Concepts

- [[msvc-toolchain]]
- [[mingw]]
- [[rustup]]
- [[cargo|Cargo]]
- [[path-environment-variable|PATH]]

## Examples

```shell
cargo new test_rust
cd test_rust
cargo run
```

## Exercises

- MSVC va GNU Windows Rust target nomlarini yoddan yozing.
- `cargo run` smoke testda aslida qaysi bosqichlar bajarilishini sanang.

## Review Questions

- Nega Visual C++ Windowsda default recommendation?
- MinGW tanlansa rustup targeti nimaga o'zgaradi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-install-on-windows]]
- [[wiki/chapters/rust-for-backend-developers-install-on-linux]]
- [[rustup]]
