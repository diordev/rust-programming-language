---
title: "22.5. E - Editions"
type: source
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, editions, compatibility, cargo-fix]
source_count: 1
---

# 22.5. E - Editions

## Source

- File: `raw/books/the_rust_programming_language/22.5. E - Editions.md`
- URL: <https://doc.rust-lang.org/stable/book/appendix-05-editions.html>
- Book: *The Rust Programming Language*

## Detailed Summary

Appendix E Rust [[edition]] tizimining maqsadini tushuntiradi. Markaziy muammo shunday: Rust va compiler har olti haftada release bo'ladi, shuning uchun o'zgarishlar doimiy ravishda kirib boradi. Bu yaxshi, lekin ma'lum oraliqda to'plangan o'zgarishlarni "katta package" sifatida tushunish qiyinlashadi. Edition shuni hal qiladi.

### Editions nimaga kerak?

Book editionlarga uch xil auditoriya nuqtai nazaridan qaraydi:

- aktiv Rust userlar uchun — incremental changes'ni bitta tushunarli package'ga yig'adi;
- non-userlar uchun — Rust'da sezilarli progress bo'lganini signal qiladi;
- Rust ustida ishlovchilar uchun — project-wide rallying point bo'ladi.

At the time of this writing, mavjud editionlar:

- Rust 2015
- Rust 2018
- Rust 2021
- Rust 2024

Bookning o'zi Rust 2024 idiomlari bilan yozilgan.

### `edition` key nimani boshqaradi?

`Cargo.toml` ichidagi `edition` key compiler sizning code'ingizni qaysi language rules bilan parse qilishini belgilaydi. Agar key bo'lmasa, backward compatibility sabab default `2015`.

Muhim distinction:

- compiler version = qaysi toolchain ishlayapti;
- edition = source code qaysi language boundary bilan o'qilyapti.

### Opt-in compatibility

Editionlar incompatible parsing changes olib kelishi mumkin, masalan yangi keywordlar. Lekin bu o'zgarishlar opt-in: siz editionni almashtirmasangiz, compiler yangilansa ham code continuing compile bo'ladi.

Demak edition tizimining asosiy va'dasi:

- old code sinmasin;
- yangi edition xohlagan project uni aniq tanlasin.

### Cross-edition linking

Appendix E eng muhim amaliy detailardan birini beradi: turli editiondagi crates bir-biri bilan link bo'la oladi. Edition change asosan compilerning code'ni **initial parsing** bosqichiga ta'sir qiladi. Shu sabab:

- 2015 project 2018 dependency'ni ishlata oladi;
- 2018 project 2015 dependency'ni ishlata oladi.

Bu Rust ecosystem stability uchun juda muhim.

### New features va editionlar

Ko'p featurelar barcha editionlarga stable release'lar orqali yetib boradi. Lekin ayrim hollarda, ayniqsa yangi keywordlar kiritilganda, feature faqat keyingi editionda to'liq mavjud bo'lishi mumkin. Shuning uchun ba'zi capability'lar uchun edition switch kerak bo'ladi.

### Migration

Book batafsil command bermaydi, lekin muhim pointer beradi: Edition Guide code'ni yangi editionga automatic upgrade qilishni [[cargo-fix]] orqali tushuntiradi. Bu Appendix D bilan to'g'ridan-to'g'ri bog'lanadi.

### Yakuniy takeaway

Editions Rustning "tez release + barqaror ecosystem" muvozanatini ushlab turadigan compatibility layer. Ular compiler version bilan bir xil narsa emas, va ecosystem ichida turli editiondagi crates birga ishlay olishi asosiy design yutug'idir.

## Key Concepts

- [[edition]]
- [[rust-2024-edition|Rust 2024 Edition]]
- [[rust-editions]]
- [[edition-migration]]
- [[cargo-fix-edition-migration]]

## Code Examples

Practical workflow Appendix D bilan bog'liq:

- [[cargo-fix-unused-mut-example|cargo fix unused mut example]]

## Exercises or Practice Ideas

1. Compiler version va edition o'rtasidagi farqni yozing.
2. Nega edition changes opt-in bo'lishi kerakligini tushuntiring.
3. Cross-edition linking ecosystem uchun nima beradi?
4. Qaysi turdagi featurelar ko'proq edition boundary talab qilishini misol bilan yozing.

## Questions Raised

- Edition Guide'da 2021 -> 2024 migrationning aniq bosqichlari qanday?
- Yangi keywordlar ecosystemga kirganda public API naming'ga qanday ta'sir qiladi?
- Edition migrationni CI workflow'ga qayerda qo'shish to'g'ri?

## Links To Update

- [[index|Rust Wiki Index]]
- [[overview]]
- [[log]]
- [[wiki/chapters/22-5-e-editions|Chapter 22.5]]
- [[edition]]
- [[rust-editions]]
- [[edition-migration]]
