---
title: "Workspace проект - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, workspace]
source_count: 1
---

# Workspace проект - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/43. Workspace проект.md`
- URL: https://rust-for-backend-developers.github.io/project/workspace-project.html

## Detailed Summary

Bu source katta Rust projectni alohida package'larga bo'lishni workspace orqali ko'rsatadi. Framing back-end layering misoli bilan beriladi: data layer, business logic, presentation layer. Maqsad faqat fayl bo'lish emas, balki dependency boundary va logical isolation yaratish.

Workspace ta'rifi source'da soddalashtirilgan: root package o'rniga boshqa package'larni tutadigan container. Amaliy Cargo nuqtai nazarida bu virtual manifest ham bo'lishi mumkin, lekin boshlang'ich workflow uchun `Cargo.toml`dagi `[workspace]` va `members = [...]` asosiy signal.

Eng foydali durable detail: workspace root'da `cargo build` ishlatilsa, Cargo barcha member package'larni build qiladi va ular orasidagi dependency'larni resolve qiladi. Build artifact'lar umumiy `target/`ga, lock state esa umumiy `Cargo.lock`ga tushadi.

Example `data`, `processor`, `cli` package'lari bilan composition ko'rsatadi. Ichki dependency'lar har bir child package manifestida explicit `path = "../..."` bilan yoziladi. Workspace membership dependency'ni avtomatik import qilmaydi.

`cargo run -p cli` sectioni workspace ichida bitta member package bilan ishlashni ko'rsatadi. Bu monorepo ichida aniq binary yoki test target'ni tanlash uchun muhim command pattern.

`[workspace.dependencies]` qismi bir xil external dependency versiyasini root'da markazlash imkonini beradi. Child package'da `rand = { workspace = true }` yozilishi version duplication'ni kamaytiradi va sync muammosini yechadi.

## Key Concepts

- [[cargo-workspaces]]
- [[workspace-dependencies]]
- [[package]]
- [[dependencies]]
- [[library-crate|library crate]]
- [[binary-crate|binary crate]]

## Code Examples

```toml
[workspace]
members = ["cli", "processor", "data"]
```

```shell
cargo run -p cli
```

```toml
[workspace.dependencies]
rand = "0.8"
```

## Exercises or Practice Ideas

- Root workspace va ikkita child package'dan iborat skelet yozing.
- Bitta internal `path` dependency qo'shing va `cargo build`ni root'dan ishlating.
- `workspace.dependencies` orqali bitta external crate versiyasini markazlashtiring.

## Questions Raised

- Qachon alohida package'lar workspace ichida qolishi kerak, qachon bitta package ichida modullar yetarli?
- Workspace membership nega dependency declaration o'rnini bosmaydi?
- `workspace.dependencies` qaysi joyda foydali, qaysi joyda ortiqcha abstraction bo'lishi mumkin?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[cargo-workspaces]]
- [[workspace-dependencies]]
- [[cargo|Cargo]]
- [[package]]
- [[dependencies]]

