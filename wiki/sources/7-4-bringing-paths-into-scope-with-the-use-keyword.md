---
title: "7.4. Bringing Paths Into Scope with the use Keyword - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, modules, use]
source_count: 1
source_path: "raw/books/the_rust_programming_language/7.4. Bringing Paths Into Scope with the use Keyword.md"
source_url: "https://doc.rust-lang.org/stable/book/ch07-04-bringing-paths-into-scope-with-the-use-keyword.html"
---

# 7.4. Bringing Paths Into Scope with the use Keyword - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/7.4. Bringing Paths Into Scope with the use Keyword.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch07-04-bringing-paths-into-scope-with-the-use-keyword.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[use-declarations|use declarations]]ni chuqurlashtiradi. Oldingi sectionlarda [[paths|path]]lar item'ga yetib borish uchun full yoki relative shaklda yozilgan edi; `use` esa path'ni bir marta current [[scope]]ga olib kirib, keyin qisqaroq nom bilan ishlatishga imkon beradi. Masalan, `use crate::front_of_house::hosting;` root scope'da `hosting` nomini valid qiladi, shundan keyin `hosting::add_to_waitlist()` deb chaqirish mumkin.

`use` filesystemdagi symbolic linkga o'xshatiladi: original item boshqa joyda qoladi, lekin current scope ichida unga qisqa nom bilan murojaat qilish mumkin bo'ladi. Bu shortcut ham [[privacy]] qoidalarini tekshiradi; `use` private item'ga accessni chetlab o'tmaydi.

Muhim qoida: `use` faqat yozilgan scope'da amal qiladi. Agar `use crate::front_of_house::hosting;` crate rootda yozilib, `eat_at_restaurant` function `customer` child module'iga ko'chirilsa, child module ichida `hosting::add_to_waitlist()` nomi resolve bo'lmaydi. Fix: `use`ni `customer` module ichiga ko'chirish yoki child module'dan parent scope'dagi shortcutga `super::hosting` orqali murojaat qilish.

Section idiomatic `use` pathlarini ham ajratadi. Function olib kirilganda odatda functionning parent module'i scope'ga kiritiladi: `use crate::front_of_house::hosting;` va call site'da `hosting::add_to_waitlist()`. Bu call qayerdan kelayotganini ko'rsatadi va local function bilan chalkashishni kamaytiradi. Functionning o'zini `use crate::front_of_house::hosting::add_to_waitlist;` qilib olib kirish ishlaydi, lekin ko'pincha unidiomatic, chunki `add_to_waitlist()` local define qilingandek ko'rinadi.

Struct, enum va boshqa items uchun convention boshqacha: odatda full path bilan itemning o'zi scope'ga kiritiladi. Masalan, `use std::collections::HashMap;` yozilgandan keyin `HashMap::new()` ishlatiladi. Bu kuchli technical sababdan emas, balki Rust community'da shakllangan reading/writing convention.

Bir xil nomli item'lar kelganda conflictni parent module orqali ajratish idiomatic. Masalan, `std::fmt::Result` va `std::io::Result` ikkalasi ham `Result` nomiga ega. `use std::fmt; use std::io;` qilib, return typeda `fmt::Result` va `io::Result<()>` yozish nom collisionni oldini oladi.

Nom collision uchun ikkinchi yechim [[use-aliases|use aliases]]: `as` keyword bilan local alias berish. Masalan, `use std::fmt::Result; use std::io::Result as IoResult;` yozilganda `function1() -> Result` va `function2() -> IoResult<()>` bir scope ichida aniq bo'ladi. Har ikkala style ham idiomatic; tanlov readabilityga bog'liq.

`use` bilan olib kirilgan nom default holatda private. [[pub-use|pub use]] esa re-export qiladi: item current module scope'iga keladi va shu module tashqarisidagi code ham uni o'sha joyda define qilingandek ishlata oladi. Restaurant example'da `pub use crate::front_of_house::hosting;` external users uchun `restaurant::hosting::add_to_waitlist()` public pathini yaratadi; oldingi internal path `restaurant::front_of_house::hosting::add_to_waitlist()` kerak bo'lmaydi.

`pub use` internal organization va public API organizationni ajratish uchun foydali. Library author ichkarida `front_of_house` / `back_of_house` kabi implementation-oriented structure saqlashi mumkin, lekin crate users domainni boshqa shaklda ko'rishi kerak bo'lsa, re-export orqali qulayroq public API beriladi.

External packages ishlatilganda ikki qadam bor: dependency `Cargo.toml`ga qo'shiladi, keyin kerakli item'lar crate nomidan boshlanadigan path bilan scope'ga kiritiladi. `rand = "0.8.5"` `Cargo.toml`da dependency sifatida yoziladi; code ichida `use rand::Rng;` traitni scope'ga olib kiradi, `rand::thread_rng()` esa crate nomidan boshlanadigan absolute path bilan chaqiriladi.

[[standard-library|Standard library]] ham package'ingizga tashqi crate sifatida ko'riladi, lekin Rust bilan birga kelgani uchun `Cargo.toml`ga yozilmaydi. `std::collections::HashMap`, `std::io`, `std::cmp::Ordering` kabi pathlar `std` crate nomidan boshlanadi va `use` bilan scope'ga olib kiriladi.

Bir xil prefixga ega ko'p `use` statementsni [[nested-use-paths|nested use paths]] bilan qisqartirish mumkin. Masalan, `use std::cmp::Ordering; use std::io;` o'rniga `use std::{cmp::Ordering, io};` yoziladi. Agar bitta path boshqa pathning prefixi bo'lsa, `self` ishlatiladi: `use std::io::{self, Write};` bu `std::io` va `std::io::Write`ni birga scope'ga olib kiradi.

[[glob-operator|Glob operator]] `*` orqali path ichidagi hamma public item'larni scope'ga olib kiradi: `use std::collections::*;`. Bu ehtiyotkorlik bilan ishlatilishi kerak, chunki qaysi nom qayerdan kelganini noaniq qiladi va dependency yangi item qo'shsa, nom collision yoki compiler error paydo bo'lishi mumkin. Glob ko'pincha tests module ichida hamma tested itemsni olib kirish yoki prelude patternlarda uchraydi.

## Key Concepts

- [[use-declarations|use declarations]]
- [[scope]]
- [[paths]]
- [[privacy]]
- [[pub-use|pub use]]
- [[use-aliases|use aliases]]
- [[nested-use-paths|nested use paths]]
- [[glob-operator|glob operator]]
- [[public-api|public API]]
- [[dependencies]]
- [[standard-library|standard library]]
- [[rand]]

## Code Examples

Module'ni scope'ga olib kirish:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

`use` scope-local ekanini ko'rsatuvchi xato pattern:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

mod customer {
    pub fn eat_at_restaurant() {
        hosting::add_to_waitlist();
    }
}
```

Function parent module'ini olib kirish idiomatic:

```rust
use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

Structni to'g'ridan-to'g'ri olib kirish idiomatic:

```rust
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();
    map.insert(1, 2);
}
```

Bir xil nomli `Result` typesni parent module orqali ajratish:

```rust
use std::fmt;
use std::io;

fn function1() -> fmt::Result {
    Ok(())
}

fn function2() -> io::Result<()> {
    Ok(())
}
```

Alias bilan nom collisionni hal qilish:

```rust
use std::fmt::Result;
use std::io::Result as IoResult;

fn function1() -> Result {
    Ok(())
}

fn function2() -> IoResult<()> {
    Ok(())
}
```

Re-export:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

Nested paths:

```rust
use std::{cmp::Ordering, io};
use std::io::{self, Write};
```

Glob import:

```rust
use std::collections::*;
```

## Exercises or Practice Ideas

- `use crate::front_of_house::hosting;` qaysi scope'da amal qilishini module tree chizmasida belgilang.
- Function importida nega parent module'ni olib kirish ko'proq idiomatic ekanini izohlang.
- `HashMap` importi bilan `add_to_waitlist` importi convention jihatdan nimasi bilan farq qilishini yozing.
- `std::fmt::Result` va `std::io::Result` conflictini ikki style'da hal qiling: parent module va `as` alias.
- `pub use` orqali internal path va external public pathni alohida yozing.
- `use std::io; use std::io::Write;`ni nested path bilan bir line'ga birlashtiring.
- Glob import qachon qulay, qachon xavfli bo'lishini misollar bilan yozing.

## Questions Raised

- Public API uchun `pub use` bilan qaysi internal modulesni yashirish kerak?
- Bir team ichida function importlari uchun parent-module conventionni qanchalik qat'iy saqlash kerak?
- Glob import testlardan tashqarida qachon oqlanadi?
- `as` alias ishlatilganda alias nomini qanday naming convention bilan tanlash kerak?
- External crate traitlarini scope'ga olib kirish method resolutionga qanday ta'sir qiladi?

## Links To Update

- [[wiki/chapters/7-packages-crates-and-modules|7. Packages, Crates, and Modules]]
- [[use-declarations|use declarations]]
- [[scope]]
- [[paths]]
- [[pub-use|pub use]]
- [[use-aliases|use aliases]]
- [[nested-use-paths|nested use paths]]
- [[glob-operator|glob operator]]
- [[public-api|public API]]
- [[dependencies]]
- [[standard-library|standard library]]
- [[rand]]
