---
title: "Установка на Linux - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, linux, setup, source]
source_count: 1
---

# Установка на Linux - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/1. intro/4. Установка на Linux.md`
- URL: https://rust-for-backend-developers.github.io/setup/install-on-linux.html

## Detailed Summary

Linux install flow ikki bosqichdan iborat: avval C++ toolchain, keyin Rust toolchain. Source Linuxda C/C++ toolchain uchun GCCni asosiy variant sifatida ko'rsatadi. Distroga qarab GCC package manager orqali o'rnatiladi: Debian/Ubuntu/Mint oilasida `apt`, CentOS/Fedora/Oracle Linux oilasida `yum` yoki `dnf`.

GCC o'rnatilgandan keyin [[rustup]] o'rnatiladi. Official install command `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`; u install scriptni olib, Rust toolchainni `~/.rustup` ichiga joylaydi. Bu command networkdan script olib ishga tushirgani uchun production/security kontekstda official sourcega ishonch va commandni tushunish muhim.

Install tekshiruvi `rustc --version` orqali qilinadi. Source output misolida Rust version ko'rsatadi, lekin buni current version claim sifatida emas, sample output sifatida o'qish kerak.

## Key Concepts

- [[c-toolchain|C/C++ toolchain]]
- [[linker]]
- [[rustup]]
- [[rustc]]
- package manager

## Code Examples

GCC install:

```shell
sudo apt install gcc
sudo yum install gcc
sudo dnf install gcc
```

Rustup install:

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Install verification:

```shell
rustc --version
```

## Exercises or Practice Ideas

- O'zingiz ishlatadigan Linux distro uchun GCC install commandini aniqlang.
- `rustc --version` outputini yozib, local compiler versionni wiki note sifatida saqlang.

## Questions Raised

- Rust toolchain installidan oldin native compiler/linker kerakligini qaysi errorlar orqali sezish mumkin?
- Rustup install scriptini production workstationda ishlatishdan oldin qanday verification qilish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-install-on-linux]]
- [[wiki/chapters/rust-for-backend-developers-1-intro]]
- [[rustup]]
- [[rustc]]
- [[c-toolchain]]
