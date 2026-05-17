---
title: "Установка на Windows - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, windows, setup, source]
source_count: 1
---

# Установка на Windows - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/1. intro/3. Установка на Windows.md`
- URL: https://rust-for-backend-developers.github.io/setup/install-on-windows.html

## Detailed Summary

Windowsda Rust development uchun Rust toolchaindan tashqari C standard library va linker kerak. Source ikki yo'lni ajratadi: [[msvc-toolchain|Microsoft Visual C++ / MSVC]] va [[mingw|MinGW]]. Default va tavsiya qilingan yo'l - Visual C++ toolchain, chunki `rustup` Windowsda default sifatida `x86_64-pc-windows-msvc` targetini taklif qiladi.

MSVC yo'lida Visual Studio Community installeridan `Desktop development with C++` workload tanlanadi, minimal kerakli components sifatida MSVC Build Tools for x64/x86 va Windows 11 SDK ko'rsatiladi. C/C++ librarylar build qilinmasa, source bo'yicha shu ikki component yetarli. Kelajakda C/C++ dependencylar kerak bo'lsa, CMake tools va vcpkg kerak bo'lishi mumkin.

Rust installer Windowsda `rustup-init.exe` shaklida olinadi. MSVC variantda default install option tanlanadi va Rust toolchain `x86_64-pc-windows-msvc` target bilan o'rnatiladi. Tekshirish uchun `cargo new test_rust`, `cd test_rust`, `cargo run` ishlatiladi.

MinGW yo'li GCC portiga tayanadi. Source ikki install variantini beradi: MinGW-w64 builds archive va MSYS2. MinGW-w64 archive bilan `mingw64\bin` system `Path`ga qo'shiladi, keyin `rustup-init.exe` custom konfiguratsiyada `x86_64-pc-windows-gnu` target bilan o'rnatiladi. Source yozilgan paytda `mcf` build variant Rust bilan ishlamagan deb qayd qilingan; bu vaqtga bog'liq claim sifatida o'qilishi kerak.

MSYS2 variantda `pacman` package manager orqali `mingw-w64-x86_64-toolchain` va `base-devel` o'rnatiladi. Keyin rustup custom config orqali `x86_64-pc-windows-gnu` target tanlanadi. Natijada Windowsda GNU toolchain asosidagi Rust build environment hosil bo'ladi.

## Key Concepts

- [[rustup]]
- [[cargo|Cargo]]
- [[c-toolchain|C/C++ toolchain]]
- [[msvc-toolchain]]
- [[mingw]]
- [[linker]]
- [[path-environment-variable|PATH environment variable]]

## Code Examples

MSVC yoki MinGW installidan keyingi smoke test:

```shell
cargo new test_rust
cd test_rust
cargo run
```

MinGW target tanlashda rustup konfiguratsiyasi `x86_64-pc-windows-gnu`ga o'zgartiriladi.

## Exercises or Practice Ideas

- Windowsda MSVC va GNU target nomlarini solishtiring: `x86_64-pc-windows-msvc` va `x86_64-pc-windows-gnu`.
- `cargo new` va `cargo run` orqali install smoke test workflowini qayta yozing.

## Questions Raised

- Windows backend developer uchun MSVC default tanlov bo'lishi nimani soddalashtiradi?
- C/C++ native dependencylar paydo bo'lsa, build environmentga qaysi qo'shimcha tools kerak bo'lishi mumkin?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-install-on-windows]]
- [[wiki/chapters/rust-for-backend-developers-1-intro]]
- [[rustup]]
- [[cargo|Cargo]]
- [[msvc-toolchain]]
- [[mingw]]
