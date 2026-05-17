---
title: "7. Packages, Crates, and Modules - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, modules]
source_count: 1
source_path: "raw/books/the_rust_programming_language/7. Packages, Crates, and Modules.md"
source_url: "https://doc.rust-lang.org/stable/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html"
---

# 7. Packages, Crates, and Modules - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/7. Packages, Crates, and Modules.md`
- Type: book chapter opener
- URL: https://doc.rust-lang.org/stable/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html
- Date processed: 2026-05-06

## Detailed Summary

Bu Chapter 7 kirish sahifasi. Asosiy muammo: program kattalashgani sari code'ni bitta file va bitta [[module]] ichida saqlash yetarli bo'lmay qoladi. Related functionality'ni group qilish va distinct feature'larni ajratish code qayerda joylashganini topishni, o'zgartirish kerak bo'lgan joyni aniqlashni, va mental load'ni kamaytirishni osonlashtiradi.

Oldingi chapterlarda yozilgan programlar asosan bitta module va bitta file ko'rinishida edi. Katta projectlarda code avval bir nechta module'ga, keyin bir nechta file'ga bo'linadi. [[package]] bir nechta [[binary-crate|binary crates]] va ixtiyoriy bitta [[library-crate|library crate]] tutishi mumkin. Package kattalashsa, uning ayrim qismlari alohida [[crate]] sifatida chiqarilib, keyin [[dependencies|external dependency]]ga aylanishi mumkin. Juda katta, birga rivojlanadigan package'lar to'plami uchun Cargo workspaces Chapter 14'da keladi.

Chapter yana implementation detail'larni yashirish mavzusini ochadi. API design nuqtayi nazaridan, code qaysi qismlar public interface ekanini va qaysi qismlar private implementation detail ekanini belgilaydi. Public interface orqali boshqa code operation'ni ishlatadi; implementation detail esa keyinroq o'zgartirilishi mumkin bo'lgan ichki qism bo'lib qoladi. Bu [[api-design|API design]] va encapsulation uchun asosiy motivatsiya.

Yana bir markaziy tushuncha — [[scope]]. Scope nested context bo'lib, qaysi nomlar "in scope" ekanini belgilaydi. Programmer ham, compiler ham ma'lum joydagi nom variable, function, struct, enum, module, constant yoki boshqa item'ga tegishli ekanini bilishi kerak. Scope yaratish va nomlarni scope ichiga yoki tashqarisiga olib chiqish mumkin. Bitta scope ichida bir xil nomli ikki item bo'la olmaydi; name conflict'larni hal qilish uchun Rust va Cargo tooling beradi.

Rust code organization uchun bir nechta feature beradi. Ular birgalikda [[module-system|module system]] deb yuritiladi:

- [[package|Packages]]: Cargo feature'i; crates'ni build, test, va share qilishga yordam beradi.
- [[crate|Crates]]: library yoki executable ishlab chiqaradigan module tree.
- [[module|Modules]] va [[use-declarations|use]]: paths'ning organization, scope, va privacy'sini boshqaradi.
- [[paths|Paths]]: struct, function, module kabi item'larni nomlash yo'li.

Chapter oxirgi maqsadi: module system qismlari qanday bog'lanishini tushuntirish va scope bilan professional darajada ishlash uchun mental model berish.

## Key Concepts

- [[module-system|module system]]
- [[package]]
- [[crate]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[module]]
- [[use-declarations|use declarations]]
- [[paths]]
- [[scope]]
- [[cargo|Cargo]]
- [[dependencies]]
- [[api-design|API design]]

## Code Examples

Bu chapter opener konkret code bermaydi. U Chapter 7'da keladigan organization modelini tayyorlaydi: package, crate, module, `use`, va path.

## Exercises or Practice Ideas

- Oldingi `guessing-game` yoki `hello_cargo` projectini tasavvur qiling: undagi package, crate, va crate root qaysi ekanini yozib chiqing.
- Katta projectda qaysi logic'ni module ichida saqlash, qaysi logic'ni alohida crate'ga chiqarish mumkinligini 2-3 misol bilan o'ylab ko'ring.
- API public interface'i va private implementation detail'i bo'lishi kerak bo'lgan kichik Rust type misolini yozing.
- Bitta scope ichida bir xil nomli ikki item yaratishga urining va compiler xabarini kuzating.

## Questions Raised

- Qachon code'ni faqat module'ga bo'lish yetarli, qachon alohida crate kerak bo'ladi?
- Public interface bilan private implementation detail chegarasi Rustda qanday belgilanadi?
- `use` va path'lar real projectda nomlar conflict'ini qanday kamaytiradi?
- [[package]] va [[crate]] farqini Cargo project layoutida qanday tez ko'rish mumkin?

## Links To Update

- [[wiki/chapters/7-packages-crates-and-modules|7. Packages, Crates, and Modules]]
- [[module-system|module system]]
- [[package]]
- [[crate]]
- [[module]]
- [[scope]]
