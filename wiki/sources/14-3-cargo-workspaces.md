---
title: "14.3. Cargo Workspaces"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cargo, workspaces, monorepo, package]
source_count: 1
---

# 14.3. Cargo Workspaces

## Source

- URL: https://doc.rust-lang.org/stable/book/ch14-03-cargo-workspaces.html
- Kitob: The Rust Programming Language
- Bob: 14.3

## Detailed Summary

### Workspace nima?

**Workspace** — bitta `Cargo.lock` va bitta `target/` papkasini ulashuvchi bir nechta bog'liq package'lar to'plami. Katta loyihani kichik, mustaqil crate'larga bo'lishga imkon beradi.

### Workspace yaratish

```text
add/
  Cargo.toml     ← workspace root (package bo'limi yo'q!)
  Cargo.lock     ← yagona lock fayl
  target/        ← yagona build papkasi
  adder/
    Cargo.toml
    src/main.rs
  add_one/
    Cargo.toml
    src/lib.rs
```

**Root `Cargo.toml`:**
```toml
[workspace]
resolver = "3"
members = ["adder", "add_one"]
```

`cargo new adder` → workspace'ga avtomatik qo'shiladi.
`cargo new add_one --lib` → library crate yaratadi.

### Ichki dependency

Workspace crate'lari bir-biriga bog'liq emasligini Cargo o'z-o'zidan bilmaydi — explicit yozilishi kerak:

```toml
# adder/Cargo.toml
[dependencies]
add_one = { path = "../add_one" }
```

### Bitta `Cargo.lock` — versiya muvofiqligi

- Barcha crate'lar uchun bitta lock → barcha crate'lar bir xil versiyani ishlatadi
- Bir crate'da `rand = "0.8.5"` bo'lsa, boshqasida ham `Cargo.toml`'ga qo'shilishi kerak — lekin ikkinchi marta yuklanmaydi
- Crate'lar o'rtasida muvofiqlik kafolatlanadi

### Workspace buyruqlari

```shell
$ cargo build           # barcha crate'larni build
$ cargo run -p adder    # aniq package'ni run
$ cargo test -p add_one # aniq package'ni test
$ cargo test            # barcha crate'larni test
```

### Publish

```shell
$ cargo publish -p add_one   # aniq crate'ni publish
$ cargo publish -p adder     # har birini alohida
```

Workspace'dagi crate'lar alohida-alohida publish qilinadi.

## Key Concepts

- [[cargo-workspaces|cargo workspaces]] — to'liq tushuncha sahifasi
- [[cargo|Cargo]] — workspace boshqaruvi
- [[package]] — workspace'dagi har bir member
- [[cargo-lock|Cargo.lock]] — bitta yagona lock
- [[dependencies]] — workspace'da ichki va tashqi dependency'lar

## Code Examples

- Listing 14-7: `adder` crate'da `add_one` ishlatish

## Exercises or Practice Ideas

- `add_two` crate'ini workspace'ga qo'shing
- Barcha crate'larga umumiy `rand` dependency qo'shing
- `cargo test -p` bilan faqat bitta crate'ni test qiling

## Questions Raised

- Workspace'dagi crate'lar uchun umumiy `[profile.*]` qanday sozlanadi?
- Virtual manifest workspace (hech bir member `[package]` bo'lmagan root) qanday ishlaydi?

## Links To Update

- [[cargo-workspaces]]
- [[cargo]]
- [[wiki/chapters/14-more-about-cargo-and-crates-io]] chapter
