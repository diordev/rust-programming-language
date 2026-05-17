---
title: "Module System"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, modules, crates]
source_count: 6
---

# Module System

## Short Definition

Module system Rust code organization, scope, privacy, va item names'ni boshqaradigan feature'lar to'plami. Back-end beginner source buni name conflictlarni ajratish va code'ni mantiqiy guruhlarga bo'lish nuqtasidan kiritadi.

## Why It Matters

Katta Rust projectlarda qaysi code qayerda turishini, qaysi API public ekanini, qaysi implementation detail private qolishini, va nomlar qanday resolve qilinishini module system belgilaydi.

## Mental Model

Chapter 7 module systemni to'rt qismga ajratadi:

- [[package|Packages]]: Cargo feature'i; crates'ni build, test, share qiladi.
- [[crate|Crates]]: library yoki executable ishlab chiqaradigan module tree.
- [[module|Modules]] va [[use-declarations|use]]: paths organization, scope, va privacy'sini boshqaradi.
- [[paths|Paths]]: item'larni nomlash yo'li.

7.2 section bu modelni compiler qoidalariga bog'laydi: compiler [[crate-root|crate root]]dan boshlaydi, `mod` declarations orqali modules va submodules code'ini topadi, [[module-tree|module tree]]ni quradi, [[privacy]]ni private by default qiladi, [[pub-keyword|pub]] bilan public API ochadi, va [[use-declarations|use]] orqali path shortcutlarini scope'ga olib kiradi.

7.3 section path resolution va privacy interaction'ni ochadi: [[absolute-path|absolute paths]] `crate`dan, [[relative-path|relative paths]] current module'dan boshlanadi; [[super-keyword|super]] parent module'ga chiqadi; public API har bir module/item/field darajasida explicit tanlanadi.

7.4 section [[use-declarations|use]]ni scope-local import sifatida chuqurlashtiradi: idiomatic import conventionlar, [[use-aliases|use aliases]], [[pub-use|pub use]] re-export, external crates, [[nested-use-paths|nested use paths]], va [[glob-operator|glob operator]]. 7.5 section esa [[mod-declarations|mod declarations]] va [[module-file-layout|module file layout]]ni ajratadi: `mod` module code'ini crate tree'ga ulaydi, `use` esa mavjud path'ni faqat scope ichida qisqartiradi.

## Syntax and Examples

```rust
mod garden;
```

Bu syntax module e'lon qilishning bir ko'rinishi. Crate rootda `mod garden;` compilerga `src/garden.rs` yoki `src/garden/mod.rs` kabi joylardan module code'ini qidirishni bildiradi.

```rust
use crate::garden::vegetables::Asparagus;
```

Bu `Asparagus` type'ini current scope'da qisqa nom bilan ishlatishga imkon beradi.

```rust
pub use crate::front_of_house::hosting;
```

Bu `hosting` module'ini current module public API'si orqali re-export qiladi.

```rust
crate::front_of_house::hosting::add_to_waitlist();
front_of_house::hosting::add_to_waitlist();
super::deliver_order();
```

Bu pathlar mos ravishda absolute path, relative path, va parent module'dan boshlanadigan relative path.

```text
src/
  lib.rs
  front_of_house.rs
  front_of_house/
    hosting.rs
```

Bu file layout `mod front_of_house;` va `pub mod hosting;` declarations orqali module tree'ga ulanadi.

## Common Mistakes

- Module systemni faqat file splitting deb o'ylash.
- Package, crate, module, va path tushunchalarini bitta daraja deb ko'rish.
- Public/private API chegarasi module systemning bir qismi ekanini unutish.
- `use` declaration module tree'ni o'zgartiradi deb o'ylash; u faqat scope ichida shortcut yaratadi.
- Correct path privacy errorlarini avtomatik hal qiladi deb o'ylash.
- `mod` va `use`ni bir xil deb o'ylash.
- File layout o'zgarsa public module pathlar ham majburiy o'zgaradi deb taxmin qilish.

## Related Concepts

- [[package]]
- [[crate]]
- [[module]]
- [[module-tree|module tree]]
- [[use-declarations|use declarations]]
- [[paths]]
- [[absolute-path|absolute path]]
- [[relative-path|relative path]]
- [[super-keyword|super keyword]]
- [[scope]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[public-api|public API]]
- [[api-design|API design]]
- [[pub-use|pub use]]
- [[use-aliases|use aliases]]
- [[nested-use-paths|nested use paths]]
- [[glob-operator|glob operator]]
- [[mod-declarations|mod declarations]]
- [[module-file-layout|module file layout]]

## Sources

- [[wiki/sources/7-packages-crates-and-modules]]
- [[7-2-control-scope-and-privacy-with-modules]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword]]
- [[7-5-separating-modules-into-different-files]]
- [[wiki/sources/rust-for-backend-developers-modules]]
