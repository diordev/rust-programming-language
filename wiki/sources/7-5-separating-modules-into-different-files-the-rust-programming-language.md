---
title: "7.5. Separating Modules into Different Files - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, modules, file-layout]
source_count: 1
source_path: "raw/books/the_rust_programming_language/7.5. Separating Modules into Different Files - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch07-05-separating-modules-into-different-files.html"
---

# 7.5. Separating Modules into Different Files - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/7.5. Separating Modules into Different Files - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch07-05-separating-modules-into-different-files.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[module-file-layout|module file layout]]ni ko'rsatadi: module definitions kattalashsa, ularni alohida file'larga ko'chirish mumkin, lekin [[module-tree|module tree]] o'zgarmaydi. Oldingi restaurant example'da hamma module body'lari `src/lib.rs` ichida edi; 7.5 shu structure'ni bir nechta file'ga ajratadi.

Library crate root `src/lib.rs`da `mod front_of_house;` declaration qoldiriladi. Bu declaration `front_of_house` module borligini bildiradi, lekin body'ni inline curly braces ichida bermaydi. Compiler `front_of_house` code'ini `src/front_of_house.rs` file'dan topadi. Shu bosqichda `src/lib.rs` hali `pub use crate::front_of_house::hosting;` va `eat_at_restaurant` functionini saqlaydi.

`src/front_of_house.rs` ichiga ilgari `mod front_of_house { ... }` body ichida turgan code joylanadi. Masalan:

```rust
pub mod hosting {
    pub fn add_to_waitlist() {}
}
```

Keyingi bosqichda child module `hosting` ham alohida file'ga ajratiladi. `src/front_of_house.rs` endi faqat `pub mod hosting;` declarationni tutadi. Compiler child module file'ini parent module directory'si ostida qidiradi: `src/front_of_house/hosting.rs`.

`src/front_of_house/hosting.rs` ichida `hosting` module body ichidagi items joylanadi, lekin yana `mod hosting { ... }` deb o'ralmaydi. File'ning o'zi declaration qilingan module body sifatida ishlatiladi:

```rust
pub fn add_to_waitlist() {}
```

Muhim mental model: `mod` declaration file'ni crate'ga bir marta ulaydi; u boshqa tillardagi textual `include` operatsiyasi emas. Compiler file qayerda module tree'ga joylashishini `mod` declaration turgan joydan biladi. Keyin boshqa file'lar shu module code'iga u e'lon qilingan module tree pathi orqali murojaat qiladi.

File placement module tree bilan mos bo'lishi kerak. Agar `hosting.rs` `src/` ichiga qo'yilsa, compiler uni crate rootdagi `hosting` module uchun deb kutadi; `front_of_house` child module'i uchun emas. `hosting` `front_of_house`ning child module'i bo'lgani uchun file `src/front_of_house/hosting.rs`da bo'lishi kerak.

Rust eski `mod.rs` style'ni ham qo'llab-quvvatlaydi. Crate rootda declared `front_of_house` uchun compiler `src/front_of_house.rs` yoki `src/front_of_house/mod.rs`ni qidiradi. Child module `hosting` uchun `src/front_of_house/hosting.rs` yoki `src/front_of_house/hosting/mod.rs` ishlashi mumkin. Bitta module uchun ikkala style birga ishlatilsa compiler error beradi.

Book yangi style sifatida `src/front_of_house.rs` va `src/front_of_house/hosting.rs`ni ko'rsatadi. Eski `mod.rs` style supported bo'lsa ham, ko'p `mod.rs` file'lar editor tablarida chalkash bo'lishi mumkin. Bir project ichida turli modules uchun ikki style'ni aralashtirish allowed, lekin odamlar navigate qilishi uchun confusing bo'lishi mumkin.

Section oxirida 7-bobning asosiy sintezi beriladi: Rust package'ni multiple cratesga, crate'ni modulesga bo'lishga imkon beradi; itemsga absolute yoki relative paths bilan murojaat qilinadi; [[use-declarations|use]] pathlarni current scope'da qisqartiradi; module code private by default; public qilish uchun [[pub-keyword|pub]] kerak.

`use` file compilationga ta'sir qilmaydi. `pub use crate::front_of_house::hosting;` `src/lib.rs`da o'zgarmasdan qoladi, lekin qaysi file crate'ga compile qilinishini `use` emas, `mod` declarations belgilaydi. Bu `mod` va `use` orasidagi eng amaliy farqlardan biri.

## Key Concepts

- [[module-file-layout|module file layout]]
- [[module]]
- [[module-tree|module tree]]
- [[crate-root|crate root]]
- [[mod-declarations|mod declarations]]
- [[use-declarations|use declarations]]
- [[pub-use|pub use]]
- [[paths]]
- [[privacy]]
- [[pub-keyword|pub keyword]]

## Code Examples

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

Idiomatic file layout:

```text
src/
  lib.rs
  front_of_house.rs
  front_of_house/
    hosting.rs
```

Older supported style:

```text
src/
  lib.rs
  front_of_house/
    mod.rs
    hosting/
      mod.rs
```

## Exercises or Practice Ideas

- Inline `mod front_of_house { ... }` example'ini uch file'ga ajrating.
- `mod front_of_house;` declaration turgan joy nima uchun `front_of_house` module tree joyini belgilashini tushuntiring.
- `hosting.rs`ni `src/`ga qo'yish bilan `src/front_of_house/`ga qo'yish orasidagi farqni yozing.
- `mod` va `use` orasidagi farqni ikki jumlada ifodalang.
- Modern file style va `mod.rs` style uchun bir xil module tree chizmasini tuzing.

## Questions Raised

- Katta projectda module file'larini qachon ajratish kerak?
- `mod.rs` style existing projectda uchrasa, uni modern style'ga o'tkazish worthwhilemi?
- `pub use` public API'ni o'zgartirmasdan internal file layoutni refactor qilishga qanday yordam beradi?
- File layout va module tree farqini newcomer'ga qanday tez tushuntirish mumkin?

## Links To Update

- [[7-packages-crates-and-modules|7. Packages, Crates, and Modules]]
- [[module-file-layout|module file layout]]
- [[module]]
- [[module-tree|module tree]]
- [[crate-root|crate root]]
- [[mod-declarations|mod declarations]]
- [[use-declarations|use declarations]]
- [[pub-use|pub use]]
- [[paths]]
