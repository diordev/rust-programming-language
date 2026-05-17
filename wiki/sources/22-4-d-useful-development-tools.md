---
title: "22.4. D - Useful Development Tools"
type: source
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, tooling, rustfmt, clippy, cargo-fix, rust-analyzer]
source_count: 1
---

# 22.4. D - Useful Development Tools

## Source

- File: `raw/books/the_rust_programming_language/22.4. D - Useful Development Tools.md`
- URL: <https://doc.rust-lang.org/stable/book/appendix-04-useful-development-tools.html>
- Book: *The Rust Programming Language*

## Detailed Summary

Appendix D Rust projectining rasmiy development tooling qatlamini compact reference ko'rinishida beradi. Bo'lim to'rtta asosiy yordamchini ko'rsatadi: formatting uchun [[rustfmt]], warning fix'larni avtomatik qo'llash uchun `rustfix`/[[cargo-fix]], linting uchun [[clippy]], va IDE integration uchun [[rust-analyzer]].

### rustfmt

`rustfmt` Rust code'ni community style bo'yicha avtomatik formatlaydi. Mualliflar uning asosiy foydasini juda amaliy tarzda qo'yadi: collaborative projectlarda "qaysi style to'g'ri?" degan bahslarni kamaytiradi, chunki hamma bitta tool bilan formatlaydi.

Book ikki commandni farqlaydi:

- `rustfmt` — fine-grained control;
- `cargo fmt` — Cargo project conventions'ini tushunadigan wrapper.

Muhim nuance: `cargo fmt` semantics'ni o'zgartirmasligi, faqat style'ni o'zgartirishi kerak.

### rustfix va cargo fix

Book `rustfix` tooli Rust install bilan kelishini aytadi, lekin amaliy surface sifatida `cargo fix` commandini ko'rsatadi. Misol `unused_mut` warningi ustida qurilgan:

```rust
let mut x = 42;
println!("{x}");
```

Compiler "variable does not need to be mutable" deb warning beradi va `mut`ni olib tashlashni taklif qiladi. `cargo fix` shu suggestion'ni avtomatik qo'llab:

```rust
let x = 42;
println!("{x}");
```

ko'rinishiga o'tkazadi.

Appendix D shuningdek `cargo fix` edition migration uchun ham ishlatilishini aytadi; Appendix E bu nuqtani edition upgrade kontekstida davom ettiradi.

### Clippy

[[clippy]] — additional lints collection. Book uni "common mistakesni ushlash va code quality'ni yaxshilash" vositasi sifatida ta'riflaydi. Amaliy misol `approx_constant` lint:

```rust
let x = 3.1415;
```

Clippy `std::f64::consts::PI` ishlatishni tavsiya qiladi. Demak Clippy oddiy compile errorlardan tashqariga chiqib, correctness va style-quality signal ham beradi.

### rust-analyzer

IDE integration uchun current tavsiya — [[rust-analyzer]]. Book uni compiler-centric utilities to'plami va Language Server Protocol bilan gaplashadigan server sifatida tavsiflaydi. Natijada IDE ichida:

- autocompletion,
- jump to definition,
- inline errors

paydo bo'ladi.

Bu bo'limning muhim meta-takeaway'i: Rust development experience tilning o'zi bilan tugamaydi; rasmi tooling qatlamini ishlatish developer feedback loop'ni ancha tezlashtiradi.

## Key Concepts

- [[rustfmt]]
- [[cargo-fix]]
- [[clippy]]
- [[rust-analyzer]]
- [[tooling]]
- [[readable-code|readable code]]

## Code Examples

- [[cargo-fix-unused-mut-example|cargo fix unused mut example]]
- [[clippy-approx-constant-example|Clippy approx constant example]]

## Exercises or Practice Ideas

1. `cargo fmt`, `cargo fix`, va `cargo clippy` orasidagi vazifa farqini yozing.
2. `unused_mut` warningi nega automatic fix uchun yaxshi nomzod ekanini tushuntiring.
3. `approx_constant` lint correctnessga qanday xizmat qilishini yozing.
4. `rust-analyzer` compiler bilan qanday feedback loop hosil qilishini izohlang.

## Questions Raised

- `cargo fix` qaysi warninglarni xavfsiz tarzda qo'llaydi, qaysilarini emas?
- Team style policy'da `rustfmt` va `clippy` qaysi darajada mandatory bo'lishi kerak?
- `rust-analyzer` va boshqa LSP clientlar o'rtasidagi farq qayerda seziladi?

## Links To Update

- [[index|Rust Wiki Index]]
- [[overview]]
- [[log]]
- [[wiki/chapters/22-4-d-useful-development-tools|Chapter 22.4]]
- [[rustfmt]]
- [[clippy]]
- [[cargo-fix]]
- [[rust-analyzer]]
