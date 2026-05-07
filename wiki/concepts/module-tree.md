---
title: "Module Tree"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules]
source_count: 3
---

# Module Tree

## Short Definition

Module tree crate ichidagi [[module|modules]] va item'larning nested tuzilmasi; u implicit `crate` rootidan boshlanadi.

## Why It Matters

[[paths]] module tree bo'ylab item'ga boradi, [[privacy]] esa parent/child module chegaralarida tekshiriladi. Shuning uchun module tree Rust projectda code qayerda turishi va qaysi nom bilan chaqirilishini tushunish uchun asosiy mental model.

## Mental Model

`src/main.rs` yoki `src/lib.rs` content'i root module, ya'ni `crate`ni yaratadi. Shu root ostida child modules joylashadi. Bir parent ichida define qilingan modules sibling modules hisoblanadi.

Pathlar shu tree bo'ylab yuradi. `crate::...` rootdan boshlanadi, relative path current module'dan boshlanadi, `super::...` esa parent module'ga chiqadi.

Module code alohida file'larga ajratilsa ham module tree declaration joyiga qarab qoladi. `mod front_of_house;` root child module'ni e'lon qiladi; `pub mod hosting;` `front_of_house` child module'ini e'lon qiladi.

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

## Syntax and Examples

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}
```

Bu code'da `front_of_house` `crate`ning child module'i, `hosting` esa `front_of_house`ning child module'i. `hosting` ichidagi `add_to_waitlist` function module tree'dagi leaf item'lardan biri.

## Common Mistakes

- Module tree'ni faqat filesystem tree deb o'ylash.
- `src/main.rs` yoki `src/lib.rs` implicit `crate` root module'ini yaratishini unutish.
- Sibling modules bir-birining private item'larini avtomatik ko'ra oladi deb o'ylash.
- Path module tree'da mavjud bo'lsa privacy har doim ruxsat beradi deb o'ylash.
- File layout va module tree har doim bir xil ko'rinishda bo'ladi deb o'ylash.

## Related Concepts

- [[module]]
- [[module-system|module system]]
- [[crate-root|crate root]]
- [[paths]]
- [[absolute-path|absolute path]]
- [[relative-path|relative path]]
- [[super-keyword|super keyword]]
- [[scope]]
- [[privacy]]
- [[module-file-layout|module file layout]]
- [[mod-declarations|mod declarations]]

## Sources

- [[7-2-control-scope-and-privacy-with-modules-the-rust-programming-language]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
- [[7-5-separating-modules-into-different-files-the-rust-programming-language]]
