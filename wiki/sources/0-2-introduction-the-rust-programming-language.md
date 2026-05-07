---
title: "0.2. Introduction - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source]
source_count: 1
source_path: "raw/books/the_rust_programming_language/0.2. Introduction - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch00-00-introduction.html"
---

# 0.2. Introduction - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/0.2. Introduction - The Rust Programming Language.md`
- Type: book introduction
- URL: https://doc.rust-lang.org/stable/book/ch00-00-introduction.html
- Date processed: 2026-05-06

## Detailed Summary

Bu introduction [[0-the-rust-programming-language-the-rust-programming-language|The Rust Programming Language]] kitobining kimlar uchun yozilgani, Rust nimani va'da qilishi, kitobni qanday o'qish kerakligi, va chapterlar qanday tartibda borishini tushuntiradi.

Rust introductory book sifatida tanishtiriladi: u faster va more reliable software yozishga yordam beradi. Rustning asosiy dizayn nuqtasi high-level ergonomics bilan low-level control o'rtasidagi odatiy ziddiyatni kamaytirishdir. Ya'ni developer memory usage kabi low-level detallarni nazorat qila oladi, lekin bu nazorat traditional systems programmingdagi kabi og'ir manual burden bo'lmasligi kerak.

Rust bir nechta auditoriya uchun mos deb beriladi. Large teams uchun compiler gatekeeper vazifasini bajaradi: subtle bugs va concurrency bugs compile bosqichida ushlanishi mumkin, shuning uchun team program logicga ko'proq e'tibor beradi. Rust ecosystemdagi [[cargo|Cargo]], [[rustfmt]] va [[rust-language-server|Rust Language Server]] kabi toolar systems-level code yozishda productivityni oshiradi.

Students uchun Rust systems conceptsni o'rganish yo'li sifatida ko'rsatiladi. Companies Rustni command line tools, web services, DevOps tooling, embedded devices, media processing, cryptocurrencies, bioinformatics, search engines, IoT, machine learning va Firefox qismlarida ishlatishi aytiladi. Open source developers Rust language, tools va librariesga hissa qo'shishi mumkin.

Speed and stability bo'limida Rust speedni ikki ma'noda ishlatadi: runtime performance va development speed. Compiler checks feature qo'shish va refactoring paytida stability beradi. [[zero-cost-abstractions|zero-cost abstractions]] g'oyasi safe code ham fast code bo'lishi kerak degan maqsadni bildiradi.

Kitob programmingni mutlaqo noldan o'rgatmaydi. U boshqa language tajribasi bor o'quvchini kutadi, lekin aynan qaysi language ekanini faraz qilmaydi.

Kitob sequential o'qishga mo'ljallangan, chunki later chapters earlier concepts ustiga quriladi. Chapterlar ikki turga bo'linadi: concept chapters va project chapters. Project chapters: Chapter 2 guessing game, Chapter 12 `grep` subset implementation, Chapter 21 low-level multithreaded web server. Qolgan chapterlar asosan concept chapters.

Introduction Rust Bookning full mapini beradi: installation va Cargo, guessing game, common programming concepts, [[ownership]], structs/methods, enums va [[pattern-matching|pattern matching]], [[module]] system, collections, [[error-handling|error handling]], [[generics]], [[traits]], [[lifetimes]], testing, `grep` project, closures/iterators, Cargo publishing, smart pointers, concurrency, async/await, OOP comparison, advanced patterns, [[unsafe-rust|unsafe Rust]], macros, va final web server project.

Muhim learning convention: Rustni o'rganishda compiler error messagesni o'qishni o'rganish jarayonning bir qismi. Kitob ba'zan ataylab compile bo'lmaydigan, panic qiladigan yoki desired behavior bermaydigan examplelar beradi. O'quvchi code blockni ishlatishdan oldin surrounding textni o'qishi kerak.

Source code kitobning GitHub repositorysida mavjud: `https://github.com/rust-lang/book/tree/main/src`.

## Key Concepts

- [[compiler]]
- [[cargo|Cargo]]
- [[rustfmt]]
- [[rust-language-server|Rust Language Server]]
- [[zero-cost-abstractions|zero-cost abstractions]]
- [[ownership]]
- [[pattern-matching|pattern matching]]
- [[module]]
- [[error-handling|error handling]]
- [[generics]]
- [[traits]]
- [[lifetimes]]
- [[testing]]
- [[closures]]
- [[iterators]]
- [[smart-pointers|smart pointers]]
- [[concurrency]]
- [[async-await|async/await]]
- [[unsafe-rust|unsafe Rust]]
- [[macros]]

## Code Examples

Bu introduction Rust code example bermaydi. U kitobdagi keyingi examplelarning ba'zilari ataylab compile bo'lmasligi, panic qilishi yoki not desired behavior berishini ogohlantiradi.

```rust
// No Rust code in this source.
```

## Exercises or Practice Ideas

- Rust Book chapter mapini shaxsiy learning roadmapga aylantirish.
- `Cargo`, `rustfmt` va IDE integration nima ish qilishini alohida tekshirish.
- Compiler error messagesni o'qish uchun keyingi chapterlardan compile bo'lmaydigan examplelarni alohida yig'ib borish.
- Chapter 2 projectini oldin qilish yoki Chapter 3dan keyin qaytish variantini tanlash.

## Questions Raised

- Rust compiler qaysi turdagi subtle bugs va concurrency bugsni compile vaqtida ushlaydi?
- [[zero-cost-abstractions|zero-cost abstractions]] amalda qachon va qanday namoyon bo'ladi?
- Rust Book project chapters orqali conceptlarni mustahkamlashni qanday tartibda qilish kerak?
- Compiler error messages uchun alohida `wiki/errors/` sahifalarini qachondan boshlab yaratamiz?

## Links To Update

- [[0-2-introduction|Introduction]]
- [[rust-learning-roadmap]]
- [[cargo|Cargo]]
- [[rustfmt]]
- [[compiler]]
- [[zero-cost-abstractions|zero-cost abstractions]]
- [[ownership]]
- [[concurrency]]
