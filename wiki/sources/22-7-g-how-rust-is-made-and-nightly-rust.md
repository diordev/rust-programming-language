---
title: "22.7. G - How Rust is Made and Nightly Rust"
type: source
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, nightly, release-channels, feature-flags, rfc]
source_count: 1
---

# 22.7. G - How Rust is Made and Nightly Rust

## Source

- File: `raw/books/the_rust_programming_language/22.7. G - How Rust is Made and “Nightly Rust”.md`
- URL: <https://doc.rust-lang.org/stable/book/appendix-07-nightly-rust.html>
- Book: *The Rust Programming Language*

## Detailed Summary

Appendix G Rust projectining release modeli, unstable feature policy'si, va community governance jarayonini tushuntiradi. Bu appendix Rust developer uchun muhim meta-knowledge beradi: til qanday evolyutsiya qiladi va nega stable Rust'ga upgrade qilishdan qo'rqmaslik kerak.

### Stability without stagnation

Appendixning markaziy g'oyasi — [[stability-without-stagnation]]. Rust bir vaqtning o'zida ikki talabni ushlamoqchi:

- code stability: yangi stable versionga o'tish og'riqsiz bo'lsin;
- experimentation: yangi feature'lar real userlar bilan sinovdan o'tsin.

Shu sabab guiding principle shunday ifodalanadi: stable Rust'ni upgrade qilishdan qo'rqmaslik kerak, lekin har upgrade yangi featurelar, bug fix'lar, va performance yaxshilanishini olib keladi.

### Release train model

Rust development `main` branch atrofida yuradi va release train modeli ishlatiladi. Uchta [[release-channels]] bor:

- stable
- beta
- nightly

Har kecha nightly build avtomatik chiqadi. Har olti haftada nightly'dan beta branch ajraladi. Yana olti haftadan keyin beta stable'ga aylanadi. Shu bilan bir vaqtda keyingi beta yana nightly'dan ajraladi. Bu "train" metaforasi Appendixda bir nechta ASCII diagramma bilan ko'rsatiladi.

### Beta test va regressions

Ko'p userlar stable ishlatadi, lekin beta'ni CI'da test qilish regressions'ni stable'ga chiqishidan oldin ushlashga yordam beradi. Agar bug topilsa:

1. fix main/nightly'ga tushadi;
2. keyin beta'ga backport qilinadi;
3. yangi beta release chiqadi.

### Maintenance time

Project faqat eng so'nggi stable versionni support qiladi. Yangi stable chiqqach, oldingi stable EOL bo'ladi. Demak har stable version amalda olti hafta support intervaliga ega.

### Unstable features va feature flags

Nightly'dagi work-in-progress feature'lar [[feature-flags]] orqasida turadi. User bunday feature'ni sinash uchun:

- nightly toolchain ishlatishi;
- source code'da tegishli feature flag bilan opt in qilishi kerak.

Stable yoki beta'da feature flags ishlamaydi. Shu design "real-world experimentation without stable breakage" mexanizmini beradi.

### rustup va nightly workflow

Appendix G [[rustup]] bilan channel switchingni amaliy ko'rsatadi:

- `rustup toolchain install nightly`
- `rustup toolchain list`
- project papkasida `rustup override set nightly`

Muhim use case: default stable bo'lib turadi, lekin faqat ma'lum project nightly ishlatadi.

### RFC process va teams

Yangi featurelar [[rust-rfc-process]] orqali keladi. Har kim RFC yozishi mumkin. Proposal Rust teams tomonidan muhokama qilinadi. Konsensus bo'lsa:

1. issue ochiladi;
2. kimdir implement qiladi;
3. feature gate ortida nightly'ga tushadi;
4. vaqt o'tib, nightly feedback asosida stable'ga chiqish-qilmaslik qarori olinadi.

Bu jarayon governance va engineering feedback'ni bog'laydi.

### Yakuniy takeaway

Appendix G Rust'ning texnik feature'larini emas, uning social-technical evolution modelini tushuntiradi: release channels, feature gates, rustup toolchains, va RFC process birgalikda Rustni barqaror, lekin sekinlashmagan holatda ushlab turadi.

## Key Concepts

- [[stability-without-stagnation]]
- [[release-channels]]
- [[nightly-rust]]
- [[feature-flags]]
- [[rust-rfc-process]]
- [[rustup]]

## Code Examples

- [[rustup-nightly-override-example|rustup nightly override example]]

## Exercises or Practice Ideas

1. Nega stable/beta/nightly uch kanal kerakligini yozing.
2. Feature flag'lar nega faqat nightly'da ishlashini tushuntiring.
3. `rustup override set nightly` workflow'ining afzalliklarini yozing.
4. RFC process experimental feature'dan stable feature'gacha bo'lgan yo'lni qanday boshqarishini izohlang.

## Questions Raised

- Beta'ni CI'da test qilishning amaliy strategiyasi qanday?
- Nightly feature'dan production use'gacha bo'lgan risk modelini qanday baholash kerak?
- RFC acceptance va stabilization o'rtasida qaysi mezonlar hal qiluvchi bo'ladi?

## Links To Update

- [[index|Rust Wiki Index]]
- [[overview]]
- [[log]]
- [[wiki/chapters/22-7-g-how-rust-is-made-and-nightly-rust|Chapter 22.7]]
- [[nightly-rust]]
- [[release-channels]]
- [[feature-flags]]
- [[rust-rfc-process]]
