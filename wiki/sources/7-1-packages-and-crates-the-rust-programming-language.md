---
title: "7.1. Packages and Crates - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, crates]
source_count: 1
source_path: "raw/books/the_rust_programming_language/7.1. Packages and Crates - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch07-01-packages-and-crates.html"
---

# 7.1. Packages and Crates - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/7.1. Packages and Crates - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch07-01-packages-and-crates.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[module-system|module system]]ning birinchi ikki qismi — [[package|packages]] va [[crate|crates]] — ni aniq ajratib beradi.

[[crate]] Rust compiler bir vaqtda ko'rib chiqadigan eng kichik code birligi. Agar `cargo` o'rniga to'g'ridan-to'g'ri `rustc` ishlatib bitta source file bersangiz ham, compiler shu file'ni crate deb hisoblaydi. Crate ichida [[module|modules]] bo'lishi mumkin; module'lar boshqa file'larda define qilinsa ham, ular shu crate bilan birga compile qilinadi.

Crate ikki shaklda bo'ladi:

- [[binary-crate|Binary crate]] executable programga compile bo'ladi. Command line program yoki server shunday bo'lishi mumkin. Har bir binary crate'da `main` function bo'lishi kerak; u executable ishga tushganda nima bajarilishini belgilaydi.
- [[library-crate|Library crate]] `main` function'ga ega emas va executable chiqarmaydi. U boshqa projectlar ishlatadigan functionality'ni define qiladi. Chapter 2'da ishlatilgan [[rand]] crate random number generation functionality'sini bergan. Rustaceanlar ko'pincha "crate" deganda aynan library crate'ni nazarda tutadi va uni umumiy "library" tushunchasi bilan almashtirib ishlatadi.

[[crate-root|Crate root]] compiler boshlaydigan source file. Shu file crate'ning root module'ini hosil qiladi. Keyingi sectionlarda module'lar chuqurroq tushuntiriladi.

[[package]] bir yoki bir nechta crate'ni bir functionality set sifatida bundle qiladi. Package ichida [[cargo-toml|Cargo.toml]] bo'ladi; u Cargo'ga crate'larni qanday build qilishni tasvirlaydi. [[cargo|Cargo]]ning o'zi ham package: unda command line tool uchun binary crate bor, va shu binary crate tayanadigan library crate ham bor. Boshqa projectlar Cargo command line tool ishlatadigan logic'dan foydalanish uchun Cargo library crate'iga depend qilishi mumkin.

Package qoidalari:

- package xohlagancha [[binary-crate|binary crate]] tutishi mumkin;
- package eng ko'pi bilan bitta [[library-crate|library crate]] tutadi;
- package kamida bitta crate tutishi shart — u library yoki binary bo'lishi mumkin.

`cargo new my-project` package yaratadi. Natijada `Cargo.toml` va `src/main.rs` hosil bo'ladi. `Cargo.toml` ichida `src/main.rs` alohida tilga olinmaydi, chunki Cargo convention ishlatadi: `src/main.rs` package bilan bir xil nomdagi binary crate'ning [[crate-root|crate root]]idir. Xuddi shunday, agar package directory ichida `src/lib.rs` bo'lsa, Cargo package bilan bir xil nomdagi library crate bor deb biladi va `src/lib.rs`ni uning crate root'i sifatida oladi.

Agar package'da `src/main.rs` ham, `src/lib.rs` ham bo'lsa, unda ikkita crate bor: bitta binary va bitta library, ikkalasi ham package bilan bir xil nomda. Multiple binary crate kerak bo'lsa, file'lar `src/bin/` ichiga qo'yiladi; har bir file alohida binary crate bo'ladi. Cargo crate root file'larni `rustc`ga uzatadi.

## Key Concepts

- [[crate]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[crate-root|crate root]]
- [[package]]
- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[module]]
- [[main-function|main function]]
- [[rand]]
- [[dependencies]]

## Code Examples

`cargo new` yaratadigan minimal binary package:

```shell
$ cargo new my-project
     Created binary (application) `my-project` package
$ ls my-project
Cargo.toml
src
$ ls my-project/src
main.rs
```

Cargo convention bo'yicha crate roots:

```text
my-project/
  Cargo.toml
  src/
    main.rs    # binary crate root
    lib.rs     # library crate root, agar mavjud bo'lsa
```

Multiple binary crates:

```text
my-project/
  Cargo.toml
  src/
    main.rs
    lib.rs
    bin/
      admin.rs
      worker.rs
```

Bu layoutda `src/bin/admin.rs` va `src/bin/worker.rs` har biri alohida binary crate bo'ladi.

## Exercises or Practice Ideas

- `cargo new my-project` yaratadigan layout'ni chizing va package/crate/crate root qismlarini belgilang.
- `src/lib.rs` qo'shilsa package'dagi crate count qanday o'zgarishini tushuntiring.
- `src/bin/tool.rs` qo'shib, nega u alohida binary crate bo'lishini yozing.
- `crate`, `package`, va `module` uchun bir jumladan farq yozing.
- `rand` crate'i nima uchun library crate sifatida ishlatilishini Chapter 2 misoli bilan izohlang.

## Questions Raised

- Nega package ko'p binary crate, lekin faqat bitta library crate tutishi mumkin?
- `Cargo.toml`da `src/main.rs` yozilmasa ham Cargo uni qayerdan biladi?
- Binary crate va library crate bitta package ichida qanday hamkorlik qiladi?
- `rustc` to'g'ridan-to'g'ri ishlatilganda crate tushunchasi qanday saqlanib qoladi?

## Links To Update

- [[7-packages-crates-and-modules|7. Packages, Crates, and Modules]]
- [[crate]]
- [[package]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[crate-root|crate root]]
- [[cargo|Cargo]]
