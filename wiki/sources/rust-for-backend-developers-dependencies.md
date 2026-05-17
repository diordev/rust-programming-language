---
title: "Зависимости - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, dependencies]
source_count: 1
---

# Зависимости - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/39. Зависимости.md`
- URL: https://rust-for-backend-developers.github.io/project/dependencies.html

## Detailed Summary

Bu source dependency management'ni real Cargo workflow sifatida ko'rsatadi. Fundamental nuqta oddiy: deyarli har qanday real Rust program standard library'dan tashqari external crate'ga tayanadi, va bu bog'liqlik `Cargo.toml`dagi `[dependencies]` sectionida e'lon qilinadi.

`rand` misoli dependency qo'shishning amaliy flow'ini beradi: package yaratiladi, manifestga `rand = "0.9"` yoziladi, code ichida `use rand::random;` ishlatiladi, keyin Cargo dependency va uning transitive dependency'larini yuklab compile qiladi. Bu yerda muhim boundary: manifest dependency'ni deklaratsiya qiladi, lekin kerakli function yoki trait code ichida alohida import qilinadi.

crates.io qismi markaziy registry modelini mustahkamlaydi. Source Cargo dependency'ni crates.io'dan olishini, transitive dependency'lar ham avtomatik resolve bo'lishini ochadi. `cargo build` logida aynan shuning izlari ko'rinadi.

`cargo tree` bo'limi dependency graph'ni ko'rinadigan qiladi. Bu beginner uchun muhim, chunki project ko'pincha "bitta crate qo'shdim" darajasida emas; bitta top-level dependency ortida bir nechta transitive crate turadi.

Feature'lar bo'limi dependency metadata'ning keyingi qatlami. `uuid = { version = "1", features = ["v4"] }` misoli shuni ko'rsatadi: crate functionality'sining hammasi default yoqilmasligi mumkin. `default` feature set alohida, qolgan feature'lar explicit opt-in bilan yoqiladi.

SemVer bo'limi source'dagi eng muhim dependency-management qoidalaridan biri. `uuid = "1"` exact `1.0.0` degani emas; Cargo latest compatible `1.x.y` versiyani tanlaydi. Shuning uchun version constraint va haqiqiy resolved versionni aralashtirish xato.

Source feature ma'lumotini crates.io sahifasi va crate repository'dagi `[features]` sectioni orqali topishni tavsiya qiladi. Shu yerda eng ishonchli manba crate'ning o'z `Cargo.toml`i ekani qayd qilinadi.

## Key Concepts

- [[dependencies]]
- [[cargo-toml|Cargo.toml]]
- [[crates-io|crates.io]]
- [[cargo-features]]
- [[semver|SemVer]]
- [[crate]]

## Code Examples

```toml
[dependencies]
rand = "0.9"
```

```toml
[dependencies]
uuid = { version = "1", features = ["v4"] }
```

```shell
cargo tree
```

## Exercises or Practice Ideas

- `rand` yoki `uuid` qo'shib, `cargo tree` natijasidan top-level va transitive dependency'larni ajrating.
- Bitta crate uchun `[features]` sectionini repository'dan topib, default va explicit feature'larni yozing.
- `rand = "0.9"` constrainti bilan lock qilingan exact version farqini tushuntiring.

## Questions Raised

- Qachon dependency feature yoqish kerak, qachon default behavior yetarli?
- `Cargo.toml`dagi version constraint nega real resolved version bilan bir xil emas?
- Top-level dependency soni kam bo'lsa ham, transitive dependency risklari qayerda paydo bo'ladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[dependencies]]
- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[crates-io|crates.io]]
- [[cargo-features]]
- [[semver|SemVer]]

