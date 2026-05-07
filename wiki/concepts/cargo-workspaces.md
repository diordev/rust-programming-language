---
title: "Cargo Workspaces"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cargo, workspaces, package, monorepo]
source_count: 1
---

# Cargo Workspaces

## Short Definition

Bitta `Cargo.lock` va bitta `target/` papkasini ulashuvchi bir nechta bog'liq Rust package'lar to'plami. Katta loyihani kichik, mustaqil crate'larga ajratishga imkon beradi, ularning versiya muvofiqligini esa avtomatik ta'minlaydi.

## Why It Matters

Loyiha o'sib bitta krate uchun juda katta bo'lganda workspace yordamida kod kichik bo'laklaiga bo'linadi. Bitta `Cargo.lock` barcha crate'lar uchun dependency versiyalari bir xilligini ta'minlaydi — muvofiqlik muammolari yo'q. Bitta `target/` esa keraksiz qayta kompilyatsiyani oldini oladi.

## Mental Model

```text
add/                    ← workspace root
  Cargo.toml            ← faqat [workspace] bo'limi
  Cargo.lock            ← yagona, barcha crate'lar uchun
  target/               ← yagona build papkasi
  adder/                ← binary crate (package)
    Cargo.toml
    src/main.rs
  add_one/              ← library crate (package)
    Cargo.toml
    src/lib.rs
```

## Syntax and Examples

**Root `Cargo.toml`:**
```toml
[workspace]
resolver = "3"
members = ["adder", "add_one"]
```

**Workspace ichida yangi crate yaratish:**
```shell
$ cargo new adder          # binary, avtomatik qo'shiladi
$ cargo new add_one --lib  # library, avtomatik qo'shiladi
```

**Ichki dependency (explicit!):**
```toml
# adder/Cargo.toml
[dependencies]
add_one = { path = "../add_one" }
```

**Tashqi dependency:**
```toml
# add_one/Cargo.toml
[dependencies]
rand = "0.8.5"
# adder/Cargo.toml da ham alohida yozilishi kerak — lekin ikkinchi marta yuklanmaydi
```

**Workspace buyruqlari:**
```shell
$ cargo build              # hammasi
$ cargo build -p add_one   # faqat bitta
$ cargo run -p adder       # aniq package'ni run
$ cargo test               # barcha crate'larda test
$ cargo test -p add_one    # faqat bitta crate
$ cargo publish -p add_one # faqat bitta crate'ni publish
```

## Common Mistakes

**1. Root `Cargo.toml`'da `[package]` bo'limi qo'shish:**
Workspace root'da `[package]` bo'lmaydi — faqat `[workspace]`.

**2. Ichki dependency'ni avtomatik deb o'ylash:**
Bir crate workspace'ga qo'shilganida boshqa crate'lar uni avtomatik ko'rmaydi. `[dependencies]`'da `path = ...` bilan explicit ko'rsatilishi kerak.

**3. Tashqi dependency faqat bir joyda yozilgan deb ishlatish:**
```shell
error[E0432]: unresolved import `rand`
```
`rand` bir crate'da bo'lsa ham, boshqasi o'z `Cargo.toml`'iga qo'shishi kerak.

## Related Concepts

- [[package]] — workspace'ning har bir member'i package
- [[cargo|Cargo]] — workspace boshqaruvchi tool
- [[cargo-lock|Cargo.lock]] — workspace'da yagona, barcha crate'lar uchun
- [[dependencies]] — ichki (`path`) va tashqi dependency'lar
- [[binary-crate|binary crate]] / [[library-crate|library crate]] — workspace a'zolari

## Sources

- [[14-3-cargo-workspaces-the-rust-programming-language|14.3 Cargo Workspaces]]
