---
title: "Module"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, modules]
source_count: 5
---

# Module

## Short Definition

`module` Rust code'ni namespace, [[scope]], [[privacy]], va related functionality guruhlari bo'yicha tashkil qilish vositasi.

## Why It Matters

Katta programlarda modules code organization, visibility, va API surface'ni boshqaradi. Chapter 7 module'larni package, crate, `use`, va path bilan birga [[module-system|module system]]ning asosiy qismi sifatida ko'rsatadi.

## Mental Model

[[module-tree|Module tree]] [[crate]] ichidagi yo'llarni belgilaydi. Item public bo'lishi uchun [[pub-keyword|pub]] kerak. Module'lar code'ni file'lar bo'yicha ajratishi mumkin, lekin ular faqat filesystem emas: ular names, scope, va privacy chegaralarini ham belgilaydi.

Module ichida boshqa modules, functions, structs, enums, constants, traits va boshqa Rust item'lar bo'lishi mumkin. Module'lar parent, child, va sibling munosabatlari bilan nested bo'ladi. Default holatda module ichidagi items private bo'ladi; bu visibility boundary ham module modelining bir qismi.

Module body inline yozilishi yoki [[module-file-layout|module file layout]] orqali alohida file'da turishi mumkin. File'ga ajratish module tree'ni o'zgartirmaydi; `mod` declaration module'ni crate'ga ulaydi, [[use-declarations|use]] esa path shortcut yaratadi.

## Syntax and Examples

```rust
mod garden;
```

Crate root ichida bu declaration compilerga `garden` module code'ini inline body, `src/garden.rs`, yoki `src/garden/mod.rs`dan qidirishni bildiradi.

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}
```

Bu yerda `hosting` `front_of_house`ning child module'i.

```text
src/
  lib.rs
  front_of_house.rs
  front_of_house/
    hosting.rs
```

Bu layoutda `front_of_house` root child module, `hosting` esa uning child module'i.

## Common Mistakes

- File structure va module path bir xil mexanizm deb o'ylash.
- Private item boshqa module'dan ko'rinadi deb kutish.
- `pub mod` ichidagi item'larni avtomatik public qiladi deb o'ylash.
- Module, [[crate]], va [[package]] tushunchalarini bir xil daraja deb qabul qilish.
- `mod` declaration boshqa tillardagi `include` kabi ishlaydi deb o'ylash.

## Related Concepts

- [[crate]]
- [[package]]
- [[module-system|module system]]
- [[module-tree|module tree]]
- [[paths]]
- [[use-declarations|use declarations]]
- [[scope]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[api-design|API design]]
- [[compiler]]
- [[mod-declarations|mod declarations]]
- [[module-file-layout|module file layout]]

## Sources

- [[wiki/sources/0-2-introduction]]
- [[wiki/sources/7-packages-crates-and-modules]]
- [[7-2-control-scope-and-privacy-with-modules]]
- [[7-5-separating-modules-into-different-files]]
- [[wiki/sources/rust-for-backend-developers-modules]]
