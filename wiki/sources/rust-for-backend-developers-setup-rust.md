---
title: "Установка Rust - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, setup, source]
source_count: 1
---

# Установка Rust - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/1. intro/2. Установка.md`
- URL: https://rust-for-backend-developers.github.io/setup/setup-rust.html

## Detailed Summary

Bu source Rust development uchun minimal environment nimadan iboratligini tushuntiradi. Faqat Rust compiler yetarli emas: amaliy build uchun [[rustc]], build/project tooling, [[linker]], va C standard library kerak bo'ladi. Rust source files avval machine code/object filesga compile bo'ladi, keyin linker bir nechta object fileni final executablega yig'adi.

Source muhim nuqtani ochadi: Rust ba'zi low-level operations uchun C runtime facilitiesdan foydalanadi, masalan memory allocation va ayrim copy operations. Shu sababli platformada C/C++ toolchain yoki kamida linker va C standard library mavjud bo'lishi kerak. Bu Rustni "hamma narsani o'zi olib keladigan" isolated runtime deb o'ylash xatosini tuzatadi.

Rust toolchain official yo'l bilan [[rustup]] orqali o'rnatiladi. Ammo linker va C standard library OS va C/C++ toolchainga bog'liq: Windowsda [[msvc-toolchain|MSVC toolchain]] yoki [[mingw|MinGW]], Linuxda ko'pincha GCC. Keyingi source'lar shu ikki OS bo'yicha amaliy install yo'llarini ajratadi.

## Key Concepts

- [[c-toolchain|C/C++ toolchain]]
- [[linker]]
- [[rustup]]
- [[rustc]]
- [[cargo|Cargo]]
- [[binary-executable|binary executable]]

## Code Examples

Bu source buyruqdan ko'ra build pipeline mental modelini beradi. Amaliy install commands keyingi Windows/Linux source'larda berilgan.

## Exercises or Practice Ideas

- Rust project buildida compiler va linker qaysi bosqichda ishlashini diagramma qilib yozing.
- Local machine'da `rustc --version` va system linker mavjudligini tekshiring.

## Questions Raised

- Nima uchun Rust buildi C standard libraryga ehtiyoj sezishi mumkin?
- Rust toolchain va C/C++ toolchain bir xil narsa emasligini qanday tushuntirish mumkin?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-setup-rust]]
- [[wiki/chapters/rust-for-backend-developers-1-intro]]
- [[rustup]]
- [[rustc]]
- [[linker]]
- [[tooling]]
