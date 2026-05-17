---
title: "12. An I/O Project: Building a Command Line Program"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, io, cli, project]
source_count: 1
---

# 12. An I/O Project: Building a Command Line Program

## Source

- Manba: The Rust Programming Language, 12-bob
- URL: https://doc.rust-lang.org/stable/book/ch12-00-an-io-project.html

## Detailed Summary

12-bob — bu o'quvchi uchun amaliy loyiha bo'lib, ilgari o'rganilgan barcha bilimlarni bir joyda birlashtiradi. Maqsad: `minigrep` — `grep` ning oddiy versiyasini qurishdir.

`grep` nima qiladi: berilgan faylda berilgan qatorni qidiradi va topilgan satrlarni chiqaradi. Masalan:

```shell
$ minigrep the poem.txt
```

Bu buyruq `poem.txt` faylida `"the"` so'zini qidiradi.

Loyihada quyidagi mavzular birlashadi:

- **Kod tashkillash** (7-bob: modules va crates)
- **Vektorlar va String** (8-bob: collections)
- **Xatolarni qayta ishlash** (9-bob: Result, panic!)
- **Traits va lifetimes** (10-bob)
- **Testlar yozish** (11-bob)

Bundan tashqari, qisqacha tanishuv:
- **closures** (13-bob)
- **iterators** (13-bob)
- **trait objects** (18-bob)

Rushdagi real `grep`ning to'liq versiyasi: **`ripgrep`** (Andrew Gallant tomonidan).

## Key Concepts

- [[cli|command line tool]]
- [[separation-of-concerns|separation of concerns]]
- [[env-args|std::env::args]]
- [[fs-read-to-string|fs::read_to_string]]
- [[stderr]] va [[stdout]] farqi
- [[env-var|environment variables]]

## Code Examples

Hozircha kod misollari 12.1 va 12.2 source sahifalarida.

## Exercises or Practice Ideas

- `minigrep`ni o'zingiz tugatib ko'ring: avval faqat 12.1 va 12.2 ni o'qib, keyin qidiruv mantiqini yozing.
- Keyin kitob bilan taqqoslang.

## Questions Raised

- Environment variable orqali case-insensitive qidiruv qanday amalga oshiriladi? (12.5)
- `stderr`ga xato chiqarish qachon foydali?

## Links To Update

- [[12-an-io-project]] (chapter)
- [[12-1-accepting-command-line-arguments]]
- [[12-2-reading-a-file]]
