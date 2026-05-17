---
title: "7.3. Paths for Referring to an Item in the Module Tree - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, modules, paths]
source_count: 1
source_path: "raw/books/the_rust_programming_language/7.3. Paths for Referring to an Item in the Module Tree.md"
source_url: "https://doc.rust-lang.org/stable/book/ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html"
---

# 7.3. Paths for Referring to an Item in the Module Tree - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/7.3. Paths for Referring to an Item in the Module Tree.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[module-tree|module tree]] ichidagi item'ga qanday murojaat qilishni tushuntiradi. Rustda item'ni topish uchun [[paths|path]] ishlatiladi; bu filesystem path'iga o'xshaydi, lekin Rust module tree va [[privacy]] qoidalari bilan ishlaydi.

Path ikki shaklda bo'ladi:

- [[absolute-path|absolute path]] crate rootdan boshlanadigan full path. Current crate ichidagi code uchun u `crate` literalidan boshlanadi; external crate ichidagi code uchun external crate nomidan boshlanadi.
- [[relative-path|relative path]] current module'dan boshlanadi va `self`, [[super-keyword|super]], yoki current module'dagi identifier bilan yoziladi.

Ikkala path shaklida ham identifiers `::` bilan ajratiladi. Masalan, `crate::front_of_house::hosting::add_to_waitlist()` absolute path, `front_of_house::hosting::add_to_waitlist()` esa `eat_at_restaurant` turgan module'dan boshlanadigan relative path.

Restaurant example'da `eat_at_restaurant` function crate rootda [[public-api|public API]] sifatida define qilinadi. U `front_of_house::hosting::add_to_waitlist` functionini ikki yo'l bilan chaqiradi: absolute path orqali `crate::front_of_house::hosting::add_to_waitlist()` va relative path orqali `front_of_house::hosting::add_to_waitlist()`. Ikkala path syntactically to'g'ri, lekin code darhol compile bo'lmaydi.

Compiler `error[E0603]: module 'hosting' is private` beradi. Bu [[e0603-private-item|E0603 private item]] misoli shuni ko'rsatadi: path to'g'ri bo'lishi item'ni ishlatish uchun yetarli emas; privacy ham ruxsat berishi kerak. Rustda functions, methods, structs, enums, modules, constants va boshqa items parent modules uchun private by default.

Privacy mental modeli: parent module child module ichidagi private item'larni ishlata olmaydi, lekin child module ancestor modulesdagi item'larni ko'ra oladi. Child modules o'z implementation detail'larini yashiradi, lekin ular define qilingan tashqi contextni ko'ra oladi. Rust bu defaultni inner implementation detail'larni o'zgartirishni safe qilish uchun tanlagan.

`hosting` module'ini `pub mod hosting` qilish birinchi E0603ni o'zgartiradi, lekin code hali compile bo'lmaydi: endi compiler `function 'add_to_waitlist' is private` deydi. Sabab: `pub mod hosting` faqat module'ni public qiladi, uning ichidagi function avtomatik public bo'lmaydi. Function ham `pub fn add_to_waitlist() {}` bo'lsa, absolute path ham, relative path ham ishlaydi.

7.3 public API'ni crate users bilan contract sifatida frame qiladi. Library crate boshqa projectlar ishlatishi uchun public items beradi; bu API'ni o'zgartirish boshqalar code'iga ta'sir qiladi. Shu sababli public surface'ni ongli tanlash [[api-design|API design]]ning bir qismi.

Binary + library crate pattern ham tushuntiriladi. Agar package `src/main.rs` binary crate root va `src/lib.rs` library crate root tutsa, odatda binary crate minimal entry point bo'ladi va asosiy reusable functionality library crate'ga joylanadi. Module tree `src/lib.rs`da define qilinadi; binary crate public items'ni package nomidan boshlanadigan paths orqali ishlatadi. Bu binary crate'ni library crate'ning client'i sifatida ishlatib, API designni tekshirishga yordam beradi.

`super` keyword relative path'ni parent module'dan boshlashga imkon beradi. Listing 7-8 da `back_of_house::fix_incorrect_order` current module'dagi `cook_order()`ni chaqiradi, keyin parent module'dagi `deliver_order()`ni `super::deliver_order()` orqali chaqiradi. Bu filesystemdagi `..`ga o'xshaydi. `super` parent-child relationship saqlanib, butun block boshqa joyga ko'chsa path update'larini kamaytirishi mumkin.

Section oxirida `pub` structs va enums uchun qanday ishlashi alohida ko'rsatiladi. `pub struct Breakfast` struct type'ni public qiladi, lekin fields private by default qoladi. `Breakfast`da `pub toast: String` public field bo'lsa, `seasonal_fruit: String` private field bo'lib qoladi. External code `meal.toast`ni o'qishi va yozishi mumkin, lekin `meal.seasonal_fruit`ga kira olmaydi.

Private field bor public struct uchun constructor-like public associated function kerak bo'ladi. `Breakfast::summer("Rye")` public associated function private `seasonal_fruit` field qiymatini ichkarida set qiladi. Agar bunday public constructor bo'lmasa, external code private field qiymatini set qila olmagani uchun struct instance yarata olmaydi.

Enums uchun qoidalar boshqacha: `pub enum Appetizer` public qilinganda uning variants ham public bo'ladi. `Appetizer::Soup` va `Appetizer::Salad`ni external code ishlata oladi. Enums ko'pincha variants public bo'lmasa foydasiz bo'lib qoladi, shuning uchun variants uchun alohida `pub` yozish talab qilinmaydi.

Section keyingi mavzuni tayyorlaydi: `use` keyword alohida ko'riladi, keyin `pub` va `use` birga qanday ishlashi muhokama qilinadi.

## Key Concepts

- [[paths]]
- [[absolute-path|absolute path]]
- [[relative-path|relative path]]
- [[super-keyword|super keyword]]
- [[module-tree|module tree]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[public-api|public API]]
- [[api-design|API design]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[struct-fields|struct fields]]
- [[enum-variants|enum variants]]
- [[e0603-private-item|E0603 private item]]

## Code Examples

Absolute va relative path:

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

Public struct, public field, private field:

```rust
mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,
    }

    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }
}
```

Public enum:

```rust
mod back_of_house {
    pub enum Appetizer {
        Soup,
        Salad,
    }
}
```

## Exercises or Practice Ideas

- `crate::front_of_house::hosting::add_to_waitlist()` path'ini module tree bo'yicha bosqichma-bosqich yozing.
- `front_of_house::hosting::add_to_waitlist()` relative path nima uchun `eat_at_restaurant` crate rootda turganda ishlashini tushuntiring.
- `pub mod hosting` yozib, lekin `add_to_waitlist`ni private qoldirsangiz nega E0603 chiqishini izohlang.
- `super::deliver_order()`ni absolute path bilan qayta yozing va qaysi biri refactorda kamroq o'zgarishini tushuntiring.
- `Breakfast` structida qaysi fields public/private ekanini belgilang va nega `summer` associated function kerakligini yozing.
- `Appetizer` enumidagi variants nima uchun alohida `pub` talab qilmasligini tushuntiring.

## Questions Raised

- Projectda absolute path tanlash yaxshiroqmi yoki relative path? Refactor ehtimoli bu qarorga qanday ta'sir qiladi?
- Public API'ni qanchalik kichik saqlash kerak?
- Public struct fields API contract'ni qanday kengaytiradi?
- Nega enum variants public bo'ladi, struct fields esa private by default qoladi?
- Binary crate library crate'ning client'i sifatida ishlaganda API designdagi zaif joylarni qanday ko'rsatadi?

## Links To Update

- [[wiki/chapters/7-packages-crates-and-modules|7. Packages, Crates, and Modules]]
- [[paths]]
- [[absolute-path|absolute path]]
- [[relative-path|relative path]]
- [[super-keyword|super keyword]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[public-api|public API]]
- [[e0603-private-item|E0603 private item]]
- [[restaurant-paths-and-privacy|restaurant paths and privacy]]
