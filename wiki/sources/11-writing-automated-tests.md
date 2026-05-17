---
title: "11. Writing Automated Tests"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, correctness]
source_count: 1
---

# 11. Writing Automated Tests

## Source

- Kitob: *The Rust Programming Language*
- Bo'lim: Chapter 11 (kirish)
- URL: https://doc.rust-lang.org/stable/book/ch11-00-testing.html
- Raw fayl: `raw/books/the_rust_programming_language/11. Writing Automated Tests.md`

## Detailed Summary

Edsger W. Dijkstra 1972-yilgi "The Humble Programmer" maqolasida shunday degan: "Dasturni test qilish buglarning borligini ko'rsatishning samarali usuli, lekin ularning yo'qligini isbotlashga mutlaqo yaroqsiz." Shunga qaramay, imkon boricha test yozish kerak.

Rustda *correctness* — kодning biz kutgan ishni bajaradigan darajasi. Rust type system va borrow checker bu yukni katta qismini ko'taradi, lekin hammasini ushlay olmaydi. Masalan, `add_two(3)` funksiyasi `5` qaytarishini tekshirish uchun type system yetarli emas — bu mantiqiy xato, compiler ushlamaydi.

Chapter 11 uch qismga bo'lingan:
- Test yozish mexanikasi: annotatsiyalar, macrolar (11.1)
- Test ishlatish optsiyalari va standart xulq-atvor (11.2)
- Testlarni unit tests va integration tests sifatida tashkil qilish (11.3)

## Key Concepts

- [[testing]] — Rust test mexanikasiga kirish
- [[correctness]] — kod o'ziga mo'ljallangan ishni bajarishi

## Questions Raised

- Unit tests va integration tests farqi nimada? (11.3)
- `cargo test`da qanday optsiyalar mavjud? (11.2)
- Documentation tests qanday yoziladi? (14.2)

## Links To Update

- [[index]] — yangi source qo'shish
- [[overview]] — Chapter 11 synthesisini boshlash
