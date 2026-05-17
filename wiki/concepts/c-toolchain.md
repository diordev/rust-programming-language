---
title: "C/C++ Toolchain"
type: concept
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, tooling, native]
source_count: 3
---

# C/C++ Toolchain

## Short Definition

C/C++ toolchain - native code build qilish uchun compiler, [[linker]], headers, va platform libraries to'plami.

## Why It Matters

Rust toolchain alohida bo'lsa ham, final executable link bosqichida platform linker va C standard library kerak bo'lishi mumkin. Shuning uchun Rust install ko'pincha native C/C++ toolchain installi bilan birga yuradi.

## Mental Model

[[rustc]] Rust code'ni compile qiladi; native toolchain final binaryni OS kutgan shaklga ulashda qatnashadi.

## Syntax and Examples

Linuxda GCC:

```shell
sudo apt install gcc
sudo dnf install gcc
```

Windowsda odatiy variantlar:

- [[msvc-toolchain|MSVC toolchain]]
- [[mingw|MinGW]]

## Common Mistakes

- Rust install bo'lsa linker ham bor deb o'ylash.
- Windowsda MSVC va GNU targetlarini aralashtirish.
- Native dependencylar paydo bo'lganda CMake/vcpkg/pkg-config kabi qo'shimcha tools kerak bo'lishini hisobga olmaslik.

## Related Concepts

- [[rustup]]
- [[rustc]]
- [[linker]]
- [[msvc-toolchain]]
- [[mingw]]

## Sources

- [[wiki/sources/rust-for-backend-developers-setup-rust]]
- [[wiki/sources/rust-for-backend-developers-install-on-windows]]
- [[wiki/sources/rust-for-backend-developers-install-on-linux]]

