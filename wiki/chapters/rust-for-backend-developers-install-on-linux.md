---
title: "Rust for Backend Developers: Install on Linux"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, linux, setup, chapter]
source_count: 1
---

# Rust for Backend Developers: Install on Linux

## Learning Goal

Linuxda Rust setup uchun GCC/native toolchain va rustup o'rnatish ketma-ketligini tushunish.

## Main Ideas

- Linux install ikki qatlamdan iborat: GCC/native toolchain, keyin Rust toolchain.
- GCC distro package manager orqali o'rnatiladi.
- [[rustup]] official script orqali Rust toolchainni `~/.rustup`ga o'rnatadi.
- `rustc --version` install verification uchun ishlatiladi.

## Concepts

- [[c-toolchain|C/C++ toolchain]]
- [[rustup]]
- [[rustc]]
- [[linker]]

## Examples

```shell
sudo apt install gcc
sudo dnf install gcc
rustc --version
```

## Exercises

- O'zingizning distro oilangizni aniqlab, mos GCC install commandini yozing.
- `rustc --version` outputidagi version, commit hash, va sana qismlarini ajrating.

## Review Questions

- Rustupdan oldin GCC nima uchun kerak?
- Install script ishlatishdagi trust boundary qayerda?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-install-on-linux]]
- [[wiki/chapters/rust-for-backend-developers-install-on-windows]]
- [[rustup]]
