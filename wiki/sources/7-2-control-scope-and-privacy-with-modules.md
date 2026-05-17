---
title: "7.2. Control Scope and Privacy with Modules - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, modules]
source_count: 1
source_path: "raw/books/the_rust_programming_language/7.2. Control Scope and Privacy with Modules.md"
source_url: "https://doc.rust-lang.org/stable/book/ch07-02-defining-modules-to-control-scope-and-privacy.html"
---

# 7.2. Control Scope and Privacy with Modules - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/7.2. Control Scope and Privacy with Modules.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch07-02-defining-modules-to-control-scope-and-privacy.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[module-system|module system]]ning amaliy qismini boshlaydi: [[module|modules]], [[paths]], [[use-declarations|use]], va [[pub-keyword|pub keyword]] code organization, [[scope]], va [[privacy]]ni qanday boshqarishini ko'rsatadi. Section boshida `as` keyword, external packages, va glob operator keyinroq muhokama qilinishi aytiladi, lekin bu file'ning asosiy detallari modules, file layout, `pub`, `use`, va [[module-tree|module tree]] atrofida.

Modules cheat sheet compiler module'larni qanday topishini qisqa qoidalarga ajratadi. Compiler crate compile qilayotganda avval [[crate-root|crate root]] file'dan boshlaydi: library crate uchun odatda `src/lib.rs`, binary crate uchun odatda `src/main.rs`. Shu root file crate'ning implicit `crate` nomli root module'ini yaratadi.

Module e'lon qilish `mod garden;` kabi yoziladi. Agar bu crate root ichida bo'lsa, compiler `garden` module code'ini uch joydan qidiradi: inline body (`mod garden { ... }`), `src/garden.rs`, yoki `src/garden/mod.rs`. Submodule ham shunga o'xshash, lekin parent module directory'si ostida qidiriladi. Masalan, `src/garden.rs` ichidagi `mod vegetables;` compilerga inline body, `src/garden/vegetables.rs`, yoki `src/garden/vegetables/mod.rs`ni qidirishni bildiradi.

Module crate'ga qo'shilgandan keyin undagi item'larga [[paths|path]] orqali murojaat qilinadi, privacy qoidalari ruxsat bersa. Masalan, `Asparagus` type'i `garden::vegetables` ichida bo'lsa, path `crate::garden::vegetables::Asparagus` ko'rinishida bo'lishi mumkin. Bu path crate rootdan boshlanadi va module tree bo'ylab type'ga yetib boradi.

Rustda module ichidagi code parent module'lardan private by default. Public API ochish uchun module `pub mod` bilan e'lon qilinadi; public module ichidagi item'lar ham alohida `pub` bo'lishi kerak. Ya'ni `pub mod garden;` module'ni ochadi, lekin `garden` ichidagi struct yoki function public bo'lishi uchun ular ham `pub struct`, `pub fn`, yoki mos declaration bilan public qilinadi.

`use` keyword long path'larni current [[scope]] ichida shortcut bilan ishlatish imkonini beradi. Masalan, `use crate::garden::vegetables::Asparagus;` yozilgandan keyin shu scope ichida `Asparagus` qisqa nomi ishlatiladi. `use` module tree'ni o'zgartirmaydi; u faqat scope ichida name resolution'ni qulay qiladi.

Backyard example bu qoidalarni bitta binary crate layoutida ko'rsatadi. `src/main.rs` crate root bo'lib, unda `use crate::garden::vegetables::Asparagus;`, `pub mod garden;`, va `main` function bor. `pub mod garden;` compilerga `src/garden.rs`ni qo'shishni aytadi. `src/garden.rs` ichidagi `pub mod vegetables;` esa `src/garden/vegetables.rs`ni qo'shadi. `vegetables.rs` ichida `#[derive(Debug)] pub struct Asparagus {}` public struct sifatida e'lon qilinadi.

Section keyin modules related code'ni group qilishini tushuntiradi. Modules crate ichida readability va reuse uchun code'ni tashkil qiladi; shu bilan birga item'lar private by default bo'lgani uchun implementation detail'larni yashirishga xizmat qiladi. Public module va public item'lar external code ularga depend qilishi uchun ochiladi.

Restaurant library example `cargo new restaurant --lib` orqali yaratilgan library crate sifatida ko'rsatiladi. `src/lib.rs` ichida `front_of_house` module bor; uning ichida `hosting` va `serving` submodule'lari bor. `hosting` `add_to_waitlist` va `seat_at_table` functionlarini, `serving` esa `take_order`, `serve_order`, va `take_payment` functionlarini group qiladi. Function body'lar bo'sh, chunki maqsad implementation emas, organization.

Module body curly braces ichida yoziladi. Module ichida boshqa modules, functions, structs, enums, constants, traits va boshqa Rust item'lar bo'lishi mumkin. Bu grouping programmerlarga code'ni logical area bo'yicha topishga yordam beradi: yangi functionality qo'shayotgan developer qaysi module ichiga qo'yishni tezroq tanlaydi.

`src/main.rs` va `src/lib.rs` crate roots deb atalishining sababi: bu file'lar content'i implicit `crate` module rootini yaratadi. Restaurant example'da module tree `crate -> front_of_house -> hosting/serving -> functions` shaklida. `hosting` va `serving` sibling modules; ikkalasi ham `front_of_house` ichida define qilingan. Agar A module B ichida bo'lsa, A child module, B parent module hisoblanadi. Butun module tree implicit `crate` rooti ostida turadi.

Module tree filesystem directory tree'ga o'xshaydi, lekin aynan filesystem emas: modules code'ni logical namespace, path, scope, va privacy orqali tashkil qiladi. File layout compiler qidiradigan joylarni belgilaydi, module tree esa Rust item'lariga qanday path bilan borishni ko'rsatadi.

## Key Concepts

- [[module-system|module system]]
- [[module]]
- [[module-tree|module tree]]
- [[crate-root|crate root]]
- [[paths]]
- [[use-declarations|use declarations]]
- [[pub-keyword|pub keyword]]
- [[privacy]]
- [[scope]]
- [[api-design|API design]]
- [[library-crate|library crate]]
- [[binary-crate|binary crate]]

## Code Examples

Backyard binary crate layout:

```text
backyard/
  Cargo.lock
  Cargo.toml
  src/
    main.rs
    garden.rs
    garden/
      vegetables.rs
```

`src/main.rs`:

```rust
use crate::garden::vegetables::Asparagus;

pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {plant:?}!");
}
```

`src/garden.rs`:

```rust
pub mod vegetables;
```

`src/garden/vegetables.rs`:

```rust
#[derive(Debug)]
pub struct Asparagus {}
```

Restaurant module grouping:

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}

        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}

        fn serve_order() {}

        fn take_payment() {}
    }
}
```

Restaurant module tree:

```text
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```

## Exercises or Practice Ideas

- `backyard` layoutini chizing va har bir file qaysi `mod` declaration orqali crate'ga qo'shilishini belgilang.
- `crate::garden::vegetables::Asparagus` path'ini module tree bo'yicha bosqichma-bosqich tushuntiring.
- `pub mod garden;` va `pub struct Asparagus {}` orasidagi farqni yozing.
- Restaurant example'da parent, child, va sibling modules qaysilarini belgilang.
- `front_of_house` ichiga yangi `bar` module qo'shib, unda `make_drink` function signature'ini joylashtiring.

## Questions Raised

- Public module ichidagi item'lar nega avtomatik public bo'lmaydi?
- `mod garden;` bilan `pub mod garden;` API surface nuqtayi nazaridan qanday farq qiladi?
- Module tree va filesystem tree qayerda o'xshaydi, qayerda farq qiladi?
- Qachon inline module ishlatish, qachon alohida file'ga ajratish ma'qul?
- `use` declaration qaysi scope'da amal qilishini real projectda qanday nazorat qilish kerak?

## Links To Update

- [[wiki/chapters/7-packages-crates-and-modules|7. Packages, Crates, and Modules]]
- [[module]]
- [[module-system|module system]]
- [[module-tree|module tree]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[paths]]
- [[use-declarations|use declarations]]
- [[backyard-module-layout|backyard module layout]]
