---
title: "9. Error Handling - The Rust Programming Language"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, rust-book, source, error-handling]
source_count: 1
source_path: "raw/books/the_rust_programming_language/9. Error Handling - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch09-00-error-handling.html"
---

# 9. Error Handling - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/9. Error Handling - The Rust Programming Language.md`
- Type: book chapter opening
- URL: https://doc.rust-lang.org/stable/book/ch09-00-error-handling.html
- Date processed: 2026-05-07

## Detailed Summary

Chapter 9 [[error-handling|error handling]] mavzusini ochadi. Rust xatolarni yashirin exception mexanizmi orqali emas, explicit language features orqali ko'rishga majbur qiladi. Ko'p holatda code compile bo'lishidan oldin error ehtimolini tan olish va unga qanday munosabatda bo'lishni belgilash kerak. Bu productionga chiqishdan oldin error pathsni ko'rishga yordam beradi.

Rust errorlarni ikki katta kategoriyaga ajratadi: [[recoverable-errors|recoverable errors]] va [[unrecoverable-errors|unrecoverable errors]]. Recoverable error - masalan file topilmasligi - userga xabar berish, retry qilish, yoki callerga error qaytarish mumkin bo'lgan holat. Unrecoverable error esa odatda bug belgisi: masalan array yoki vector oxiridan tashqaridagi indexga murojaat qilish. Bunday holatda programni davom ettirish o'rniga darhol to'xtatish ma'qul.

Rustda exceptions yo'q. Recoverable errors uchun [[result|Result<T, E>]] ishlatiladi, unrecoverable errors uchun esa [[panic|panic!]] macro executionni to'xtatadi. Chapter avval `panic!`ni, keyin `Result<T, E>` qaytarishni, undan keyin esa qachon recover qilish va qachon to'xtatish kerakligini tanlash mezonlarini ko'rsatishini aytadi.

Bu opening section Chapter 9ning mapini beradi: 9.1 [[panic|panic!]] va unrecoverable failures, 9.2 `Result` bilan recoverable errors, 9.3 esa `panic!` ishlatish yoki ishlatmaslik qarori haqida bo'ladi.

## Key Concepts

- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[unrecoverable-errors|unrecoverable errors]]
- [[result|Result<T, E>]]
- [[panic|panic!]]

## Code Examples

Opening section concrete code bermaydi, lekin chapterning asosiy tanlovini quyidagicha map qilish mumkin:

```text
recoverable error   -> Result<T, E>
unrecoverable error -> panic!
```

## Exercises or Practice Ideas

- "File not found", invalid user input, out-of-bounds index, va violated invariant holatlarini recoverable/unrecoverable sifatida ajrating.
- Oldingi guessing game projectida qaysi errorlar `Result`, qaysi holatlar `panic!` bo'lishi mumkinligini yozing.
- `panic!` va `Result<T, E>` tanlovini user experience va bug detection nuqtai nazaridan solishtiring.

## Questions Raised

- Qachon error caller tomonidan handle qilinishi kerak, qachon program to'xtashi kerak?
- Rust exceptions ishlatmagani sabab API design qanday o'zgaradi?
- `Result<T, E>`ni e'tiborsiz qoldirish nega compiler tomonidan bosim ostiga olinadi?
- `panic!` bugni tez ochib beradigan vosita bo'ladimi yoki user-facing error handling o'rnini bosadimi?

## Links To Update

- [[9-error-handling|9. Error Handling]]
- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[unrecoverable-errors|unrecoverable errors]]
- [[result|Result<T, E>]]
- [[panic|panic!]]
