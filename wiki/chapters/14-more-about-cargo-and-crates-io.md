---
title: "14. More About Cargo and Crates.io"
type: chapter
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cargo, crates-io, workspaces, publishing, tooling]
source_count: 6
---

# 14. More About Cargo and Crates.io

## Learning Goal

Cargo'ning ilg'or imkoniyatlarini o'rganish: release profillari, crates.io'ga publish qilish, workspaces bilan katta loyihalar, binary tool'lar o'rnatish va Cargo kengaytirish.

## Main Ideas

- `dev` va `release` profillari dastur lifecycle'iga mos optimallashtirish darajalarini beradi
- `///` va `//!` doc comments'lar HTML dokumentatsiya va avtomatik doc-test'lar generatsiya qiladi
- `pub use` ichki modul tuzilmasi va foydalanuvchi uchun API'ni mustaqil qiladi
- Workspace bir nechta bog'liq crate'larni bitta `Cargo.lock` va `target/` ostida birlashtiradi
- `cargo install` binary tool'larni `~/.cargo/bin/` ga o'rnatadi
- `cargo-something` naming convention Cargo'ni yangi subcommandlar bilan kengaytiradi

## Concepts

### 14.1 Release Profiles

- `dev` (`cargo build`): `opt-level = 0` — tez compile, slow runtime
- `release` (`cargo build --release`): `opt-level = 3` — slow compile, tez runtime
- `Cargo.toml`'da `[profile.dev]` / `[profile.release]` bo'limlari orqali override qilish

```toml
[profile.dev]
opt-level = 1   # default 0 o'rniga
```

### 14.2 Publishing a Crate

**Doc comments:**
- `///` — item dokumentatsiyasi; Markdown; `cargo doc` bilan HTML
- `//!` — o'z ichiga olgan item (crate/module) dokumentatsiyasi
- `# Examples` bloki → `cargo test` bilan doc-test sifatida ishga tushadi
- Standart bo'limlar: `# Panics`, `# Errors`, `# Safety`

**`pub use` — toza public API:**
- Ichki tuzilma va export tuzilmasini ajratish imkoniyati
- `pub use self::kinds::PrimaryColor;` → `use art::PrimaryColor;`

**Publish workflow:**
1. crates.io hisob + `cargo login`
2. Metadata: `description`, `license` (SPDX), noyob `name`
3. `cargo publish` — mangu, o'chirib bo'lmaydi
4. Yangi versiya: SemVer + `cargo publish`
5. `cargo yank --vers X.X.X` — mavjud loyihalarga ta'sir qilmasdan yangilarini bloklash

### 14.3 Cargo Workspaces

- Root `Cargo.toml`: `[workspace]` + `members = [...]`
- Bitta `Cargo.lock` → barcha crate'lar bir xil dependency versiyasida
- Ichki dependency: `add_one = { path = "../add_one" }` — explicit
- Tashqi dependency: har crate o'z `Cargo.toml`'iga qo'shishi kerak (bir versiya yuklanadi)
- `cargo run -p name`, `cargo test -p name`
- Publish: `cargo publish -p name` — har crate alohida

### 14.4 cargo install

- `cargo install ripgrep` → `~/.cargo/bin/rg`
- Faqat binary target (src/main.rs) bo'lgan crate'lar
- `$PATH`'da `~/.cargo/bin` bo'lishi kerak

### 14.5 Custom Commands

- `cargo-something` binary `$PATH`'da → `cargo something`
- `cargo --list` barcha subcommandlarni ko'rsatadi

## Examples

```rust
/// Adds one to the number given.
///
/// # Examples
/// ```
/// assert_eq!(6, my_crate::add_one(5));
/// ```
pub fn add_one(x: i32) -> i32 { x + 1 }
```

```toml
# workspace root Cargo.toml
[workspace]
resolver = "3"
members = ["adder", "add_one"]

# adder/Cargo.toml
[dependencies]
add_one = { path = "../add_one" }
```

```toml
# crates.io uchun to'liq Cargo.toml
[package]
name = "my_crate"
version = "0.1.0"
edition = "2024"
description = "Short description here."
license = "MIT OR Apache-2.0"
```

## Exercises

1. `[profile.dev]` da `opt-level = 1` qo'ying va compile vaqtini solishtiring
2. `add_one` funksiyasiga doc comment + `# Examples` yozing, `cargo test` bilan sinang
3. `pub use` bilan nested module'ni top levelda re-export qiling
4. Ikki crate'dan iborat workspace yarating (binary + library)
5. `cargo install ripgrep` qilib `rg --help` ishlatib ko'ring

## Review Questions

1. `dev` va `release` profillari opt-level bo'yicha qanday farq qiladi?
2. `///` va `//!` qanday farq qiladi?
3. `# Examples` bloki `cargo test`'da qanday ishlatiladi?
4. `pub use` nima uchun kerak va qanday ishlaydi?
5. Workspace'da yagona `Cargo.lock` nimani kafolatlaydi?
6. `cargo yank` mavjud loyihalarga ta'sir qiladimi?
7. `cargo-something` pattern qanday ishlaydi?

## Related Pages

- [[wiki/sources/14-more-about-cargo-and-crates-io|Source: 14. Intro]]
- [[14-1-customizing-builds-with-release-profiles|Source: 14.1]]
- [[14-2-publishing-a-crate-to-crates-io|Source: 14.2]]
- [[14-3-cargo-workspaces|Source: 14.3]]
- [[14-4-installing-binaries-with-cargo-install|Source: 14.4]]
- [[14-5-extending-cargo-with-custom-commands|Source: 14.5]]
- [[cargo|Cargo]]
- [[crates-io|crates.io]]
- [[cargo-workspaces|cargo workspaces]]
- [[documentation-comments|doc comments]]
- [[pub-use|pub use]]
- [[release-build|release build]]
- [[semver|SemVer]]
