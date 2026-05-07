---
title: "14.1. Customizing Builds with Release Profiles"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cargo, release-profiles, opt-level, performance]
source_count: 1
---

# 14.1. Customizing Builds with Release Profiles

## Source

- URL: https://doc.rust-lang.org/stable/book/ch14-01-release-profiles.html
- Kitob: The Rust Programming Language
- Bob: 14.1

## Detailed Summary

Rust'da **release profiles** — kompilyatsiya parametrlarini boshqarish uchun oldindan belgilangan, sozlanuvchi profillar. Har bir profil mustaqil konfiguratsiya bo'lib, biri ikkinchisiga ta'sir qilmaydi.

### Ikki asosiy profil

| Profil | Buyruq | Maqsad |
|--------|--------|--------|
| `dev` | `cargo build` | Tez kompilyatsiya, development |
| `release` | `cargo build --release` | Tez runtime, production |

Compile output'da ko'rinadi:
```shell
$ cargo build
    Finished `dev` profile [unoptimized + debuginfo] ...
$ cargo build --release
    Finished `release` profile [optimized] ...
```

### opt-level: asosiy farq

`opt-level` — kompilyator qo'llaydigan optimallashtirish darajasi (0–3):

```toml
# Cargo default qiymatlari
[profile.dev]
opt-level = 0      # kam optimallashtirish → tez compile

[profile.release]
opt-level = 3      # to'liq optimallashtirish → sekin compile, tez runtime
```

### Profil sozlash

`Cargo.toml` da `[profile.*]` bo'limi qo'shib default qiymatlarni override qilish mumkin:

```toml
[profile.dev]
opt-level = 1   # standart 0 o'rniga biroz ko'proq optimallashtirish
```

Faqat o'zgartirilgan parametrlar yoziladi — qolganlar Cargo default qiymatlarida qoladi. To'liq profil parametrlari Cargo dokumentatsiyasida.

## Key Concepts

- [[release-build|release build]] — `opt-level` va profil sozlash
- [[cargo|Cargo]] — profil boshqaruvi
- [[dependencies]] — build optimallashtirish development va production uchun farq qiladi

## Exercises or Practice Ideas

- `[profile.dev]` da `opt-level = 2` qo'yib compile vaqti va runtime farqini benchmark qiling

## Questions Raised

- `release` profilida `debug = true` qo'yish nima beradi?
- `bench` va `test` profillari ham sozlanishi mumkinmi?

## Links To Update

- [[release-build]]
- [[14-more-about-cargo-and-crates-io]] chapter
