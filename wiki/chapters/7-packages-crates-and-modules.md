---
title: "7. Packages, Crates, and Modules"
type: chapter
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, chapter, modules]
source_count: 6
---

# 7. Packages, Crates, and Modules

## Learning Goal

Rust project kattalashganda code'ni [[module-system|module system]] orqali tartiblash: [[package]], [[crate]], [[module]], [[module-tree|module tree]], [[use-declarations|use]], [[paths|path]], [[scope]], public/private API chegaralari, re-exports, external crate imports, va [[module-file-layout|module file layout]]ni tushunish.

## Main Ideas

- Katta programlarda code organization muhim: related functionality group qilinadi, distinct feature'lar ajratiladi.
- Oldingi kichik programlar bitta module va bitta file'da edi; katta projectlarda code bir nechta [[module]] va file'ga bo'linadi.
- [[module-system|Module system]] to'rt asosiy feature'ni birga bog'laydi: [[package|packages]], [[crate|crates]], [[module|modules]] + [[use-declarations|use]], va [[paths|paths]].
- [[package]] Cargo boshqaradigan bundle: crate'larni build, test, va share qilishga yordam beradi.
- [[crate]] compiler ko'radigan eng kichik birlik; u library yoki executable ishlab chiqaradigan module tree.
- [[binary-crate|Binary crate]] executable beradi va `main` function talab qiladi.
- [[library-crate|Library crate]] executable bermaydi; u reusable functionality'ni boshqa projectlar ishlatishi uchun define qiladi.
- [[crate-root|Crate root]] compiler boshlaydigan source file va crate'ning root module'ini yaratadi.
- `cargo new my-project` `Cargo.toml` va `src/main.rs` yaratadi; `src/main.rs` default binary crate root hisoblanadi.
- `src/lib.rs` mavjud bo'lsa, package library crate ham tutadi; `src/bin/*.rs` file'lari multiple binary crates yaratadi.
- Package xohlagancha binary crate tutishi mumkin, lekin eng ko'pi bilan bitta library crate tutadi va kamida bitta crate'ga ega bo'lishi kerak.
- Compiler crate compile qilayotganda [[crate-root|crate root]]dan boshlaydi: `src/main.rs` binary crate root, `src/lib.rs` library crate root.
- `mod garden;` declaration module code'ini inline body, `src/garden.rs`, yoki `src/garden/mod.rs` kabi joylardan qidirishga olib keladi.
- Submodule parent module directory'si ostida qidiriladi: `src/garden.rs` ichidagi `mod vegetables;` uchun `src/garden/vegetables.rs` yoki `src/garden/vegetables/mod.rs`.
- [[module-tree|Module tree]] implicit `crate` rootidan boshlanadi; modules parent, child, va sibling munosabatlari bilan nested bo'ladi.
- [[privacy]] Rustda private by default: module yoki item tashqaridan ishlatilishi uchun [[pub-keyword|pub]] bilan public qilinishi kerak.
- `pub mod` module'ni public qiladi, lekin uning ichidagi item'lar ham alohida `pub` bo'lishi kerak.
- [[use-declarations|use]] long path'ni scope-local shortcut sifatida olib kiradi, masalan `use crate::garden::vegetables::Asparagus;`.
- [[absolute-path|Absolute path]] crate rootdan boshlanadi: current crate uchun `crate::...`, external crate uchun crate nomi bilan.
- [[relative-path|Relative path]] current module'dan boshlanadi va `self`, [[super-keyword|super]], yoki local identifier bilan yoziladi.
- Path syntax to'g'ri bo'lishi access uchun yetarli emas; [[privacy]] ruxsat bermasa [[e0603-private-item|E0603 private item]] chiqadi.
- `pub mod hosting` module'ni public qiladi, lekin `add_to_waitlist` function public bo'lishi uchun `pub fn` ham kerak.
- [[public-api|Public API]] crate users bilan contract: public items boshqalar code'iga ta'sir qiladigan surface hisoblanadi.
- Binary + library package'da reusable code odatda `src/lib.rs`dagi library crate'ga joylanadi; `src/main.rs` binary crate public API'ni package nomi orqali external client kabi ishlatadi.
- [[super-keyword|super]] relative path'ni parent module'dan boshlaydi, masalan `super::deliver_order()`.
- `pub struct` fieldsni avtomatik public qilmaydi; har bir field alohida `pub` bo'lishi kerak.
- `pub enum` variantsni public qiladi, chunki enum variants odatda enumdan foydalanishning asosiy yo'li.
- Public interface va private implementation detail chegarasi API'ni boshqaradi; bu katta codebase'da mental load'ni kamaytiradi.
- [[scope]] nomlar qayerda valid va qaysi item'ga tegishli ekanini belgilaydi; Chapter 7 shu scope'larni boshqarish vositalarini ochadi.
- `use` faqat yozilgan scope'da amal qiladi; parent module'dagi import child module ichida avtomatik valid bo'lmaydi.
- Function importida parent module'ni scope'ga olib kirish ko'proq idiomatic: `use crate::front_of_house::hosting;` va call site'da `hosting::add_to_waitlist()`.
- Struct, enum va boshqa items ko'pincha full path bilan bevosita olib kiriladi: `use std::collections::HashMap;`.
- Bir xil nomli items uchun parent module bilan ajratish yoki [[use-aliases|use aliases]] (`as`) ishlatish mumkin.
- [[pub-use|pub use]] item'ni re-export qilib, internal structure'dan farqli public API path beradi.
- External crate ishlatish ikki qadamdan iborat: `Cargo.toml`ga dependency qo'shish va crate nomidan boshlanadigan path yoki `use` bilan item'ni scope'ga olib kirish.
- [[standard-library|Standard library]] `std` crate sifatida pathlarda ishlatiladi, lekin `Cargo.toml`ga qo'shilmaydi.
- [[nested-use-paths|Nested use paths]] bir xil prefixli importsni `{ ... }` bilan ixchamlaydi; `self` prefixning o'zini ham import qiladi.
- [[glob-operator|Glob operator]] `*` orqali hamma public item'larni scope'ga olib kiradi, lekin readability va name collision risklari bor.
- [[mod-declarations|mod declarations]] module file'larini crate'ga ulaydi; `use` qaysi file compile bo'lishini belgilamaydi.
- Modern module file layout `src/front_of_house.rs` va `src/front_of_house/hosting.rs` style'ini ishlatadi; older `mod.rs` style ham supported, lekin bir module uchun ikkala style birga ishlatilmaydi.
- File'larga ajratish module tree'ni o'zgartirmaydi; code boshqa file'da tursa ham paths o'sha module declaration joyiga qarab ishlaydi.

## Concepts

- [[module-system|module system]]
- [[package]]
- [[crate]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[crate-root|crate root]]
- [[module]]
- [[module-tree|module tree]]
- [[use-declarations|use declarations]]
- [[paths]]
- [[absolute-path|absolute path]]
- [[relative-path|relative path]]
- [[super-keyword|super keyword]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[public-api|public API]]
- [[pub-use|pub use]]
- [[use-aliases|use aliases]]
- [[nested-use-paths|nested use paths]]
- [[glob-operator|glob operator]]
- [[mod-declarations|mod declarations]]
- [[module-file-layout|module file layout]]
- [[scope]]
- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[dependencies]]
- [[standard-library|standard library]]
- [[api-design|API design]]

## Examples

Minimal Cargo package:

```text
my-project/
  Cargo.toml
  src/
    main.rs
```

Bu layoutda `my-project` package bitta binary crate tutadi. `src/main.rs` shu binary crate'ning [[crate-root|crate root]]idir.

Binary va library crate bir package ichida:

```text
my-project/
  Cargo.toml
  src/
    main.rs
    lib.rs
```

Bu layoutda package ikkita crate tutadi: `src/main.rs`dan binary crate, `src/lib.rs`dan library crate.

Multiple binary crate:

```text
my-project/
  Cargo.toml
  src/
    main.rs
    lib.rs
    bin/
      import.rs
      serve.rs
```

`src/bin/import.rs` va `src/bin/serve.rs` har biri alohida binary crate bo'ladi.

Backyard module layout:

```text
backyard/
  src/
    main.rs
    garden.rs
    garden/
      vegetables.rs
```

```rust
use crate::garden::vegetables::Asparagus;

pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {plant:?}!");
}
```

`src/main.rs` crate root. `pub mod garden;` `src/garden.rs`ni, `pub mod vegetables;` esa `src/garden/vegetables.rs`ni crate'ga qo'shadi.

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

Restaurant paths and privacy:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    crate::front_of_house::hosting::add_to_waitlist();
    front_of_house::hosting::add_to_waitlist();
}
```

`super` bilan parent module'ga chiqish:

```rust
fn deliver_order() {}

mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::deliver_order();
    }

    fn cook_order() {}
}
```

Idiomatic `use` for function parent module:

```rust
use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

Struct import:

```rust
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();
    map.insert(1, 2);
}
```

Alias and nested imports:

```rust
use std::fmt::Result;
use std::io::Result as IoResult;
use std::{cmp::Ordering, io};
use std::io::{self, Write};
```

Restaurant re-export and split file layout:

```text
restaurant/
  src/
    lib.rs
    front_of_house.rs
    front_of_house/
      hosting.rs
```

`src/lib.rs`:

```rust
mod front_of_house;

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

`src/front_of_house.rs`:

```rust
pub mod hosting;
```

`src/front_of_house/hosting.rs`:

```rust
pub fn add_to_waitlist() {}
```

## Exercises

- Existing `hello_cargo` layoutini package/crate/crate root terminlari bilan qayta tushuntiring.
- `src/lib.rs` qo'shilgan projectda qaysi code reusable library qismiga, qaysi code binary entry point'ga kirishini ajrating.
- `src/bin/` ichiga ikkita file qo'yilgan package uchun nechta binary crate borligini hisoblang.
- `package`, `crate`, `module`, va `path` uchun bir-biridan farqli mental model yozing.
- Bitta kichik project uchun public interface va private implementation detail chegarasini yozib ko'ring.
- `backyard` layoutida `crate::garden::vegetables::Asparagus` path'i qaysi file'lardan o'tishini izohlang.
- `restaurant` example'da parent, child, sibling module'larni belgilang.
- `pub mod garden;` bor, lekin `Asparagus` `pub` bo'lmasa nima bo'lishini tushuntiring.
- Restaurant path example'da absolute path va relative pathni alohida belgilang.
- `pub mod hosting` bor, lekin `pub fn add_to_waitlist` yo'q bo'lsa nima uchun E0603 chiqishini tushuntiring.
- `Breakfast` structidagi public/private fieldsni API design nuqtayi nazaridan tahlil qiling.
- `Appetizer` enum variantsini ishlatish uchun nega `Soup` va `Salad`ga alohida `pub` kerak emasligini yozing.
- Parent module'dagi `use` child module ichida nima uchun ishlamasligini kichik example bilan tushuntiring.
- `std::fmt::Result` va `std::io::Result` conflictini parent module va alias style'ida yozing.
- `pub use` yordamida internal path va external public pathni alohida chizing.
- `use std::io; use std::io::Write;`ni nested path bilan birlashtiring.
- Glob importning testlarda qulay, production codeda esa xavfli bo'lishi mumkinligini izohlang.
- Inline restaurant module'ini `src/lib.rs`, `src/front_of_house.rs`, va `src/front_of_house/hosting.rs`ga ajrating.
- `mod` va `use` orasidagi farqni compiler nuqtayi nazaridan yozing.

## Review Questions

- [[package]] va [[crate]] farqi nimada?
- [[binary-crate|Binary crate]] nima uchun `main` function talab qiladi?
- [[library-crate|Library crate]] nega executable chiqarmaydi?
- [[crate-root|Crate root]] nima va Cargo uni qayerdan topadi?
- Package nechta binary crate va nechta library crate tutishi mumkin?
- `src/bin/*.rs` file'lari qanday crate yaratadi?
- [[scope]] va [[paths|paths]] code organization bilan qanday bog'lanadi?
- [[module-tree|Module tree]] nima va uning root'i qaysi implicit module?
- `mod garden;` compilerga qaysi file'larni qidirishni aytadi?
- [[pub-keyword|pub]] module va item'larda qanday farq bilan ishlaydi?
- [[use-declarations|use]] path'ni qanday qisqartiradi va qaysi scope'da amal qiladi?
- [[absolute-path|Absolute path]] va [[relative-path|relative path]] farqi nimada?
- [[super-keyword|super]] qachon ishlatiladi?
- [[e0603-private-item|E0603]] nimani bildiradi?
- `pub struct` fields va `pub enum` variants privacy bo'yicha qanday farq qiladi?
- Function va struct importlari bo'yicha Rust idiomlari qanday farq qiladi?
- `as` alias qachon kerak bo'ladi?
- [[pub-use|pub use]] re-export public API designga qanday yordam beradi?
- External crate ishlatishda `Cargo.toml` va `use`ning roli qanday farq qiladi?
- [[glob-operator|Glob operator]] qaysi risklarni keltiradi?
- [[mod-declarations|mod declaration]] va [[use-declarations|use declaration]] farqi nimada?
- Rust compiler child module file'ini qayerdan qidiradi?
- `src/name.rs` va `src/name/mod.rs` style'larini bir module uchun birga ishlatsa nima bo'ladi?

## Related Pages

- [[wiki/sources/7-packages-crates-and-modules]]
- [[7-1-packages-and-crates]]
- [[7-2-control-scope-and-privacy-with-modules]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword]]
- [[7-5-separating-modules-into-different-files]]
- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[module]]
- [[module-tree|module tree]]
- [[module-file-layout|module file layout]]
- [[mod-declarations|mod declarations]]
- [[paths]]
- [[absolute-path|absolute path]]
- [[relative-path|relative path]]
- [[super-keyword|super keyword]]
- [[use-declarations|use declarations]]
- [[pub-use|pub use]]
- [[use-aliases|use aliases]]
- [[nested-use-paths|nested use paths]]
- [[glob-operator|glob operator]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[public-api|public API]]
- [[restaurant-paths-and-privacy|restaurant paths and privacy]]
- [[restaurant-reexports-and-file-layout|restaurant re-exports and file layout]]
- [[dependencies]]
