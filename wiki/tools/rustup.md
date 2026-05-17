---
title: "rustup"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, tool, rustup]
source_count: 7
---

# rustup

## Short Definition

`rustup` Rust versions va associated toolsni o'rnatish, yangilash, boshqarish, va local documentationni ochish uchun ishlatiladigan command line tool.

## Common Commands

```shell
$ rustc --version
$ rustup update
$ rustup self uninstall
$ rustup doc
$ rustup doc --book
$ rustup toolchain install nightly
$ rustup toolchain list
$ rustup override set nightly
$ rustup component add rust-analyzer
```

## Notes

- Rust Book Rustni `rustup` orqali o'rnatishni tavsiya qiladi.
- `rustup update` latest stable Rust toolchainni yangilaydi.
- `rustup doc` local documentationni browserda ochadi.
- `rustup doc --book` Rust Bookni offline ochadi.
- Rustni `rustup`dan boshqa yo'l bilan o'rnatish mumkin, lekin bookning default workflowi `rustup`ga tayanadi.
- `rustup toolchain install nightly` nightly channelni qo'shadi; unstable features va nightly-only tools shu qatlamga tayanadi.
- `rustup override set nightly` current project papkasi uchun toolchain override o'rnatadi.
- `rustup toolchain list` qaysi channels local o'rnatilganini ko'rsatadi.
- Windowsda `rustup-init.exe` target sifatida MSVC yoki GNU toolchain tanlashi mumkin.
- Linuxda official install script Rust toolchainni odatda `~/.rustup` ichiga o'rnatadi.
- `rustup component add rust-analyzer` editor integration uchun [[rust-analyzer]] componentini qo'shadi.

## Related Pages

- [[1-1-installation]]
- [[0-the-rust-programming-language]]
- [[wiki/chapters/1-getting-started|Getting Started]]
- [[rustc]]
- [[cargo|Cargo]]
- [[c-toolchain|C/C++ toolchain]]
- [[msvc-toolchain]]
- [[mingw]]
- [[rust-analyzer]]
- [[nightly-rust]]
- [[release-channels]]

## Sources

- [[wiki/sources/0-the-rust-programming-language|0. The Rust Programming Language]]
- [[1-1-installation|1.1. Installation]]
- [[wiki/sources/22-7-g-how-rust-is-made-and-nightly-rust|22.7. G - How Rust is Made and Nightly Rust]]
- [[wiki/sources/rust-for-backend-developers-setup-rust]]
- [[wiki/sources/rust-for-backend-developers-install-on-windows]]
- [[wiki/sources/rust-for-backend-developers-install-on-linux]]
- [[wiki/sources/rust-for-backend-developers-development-environment]]
