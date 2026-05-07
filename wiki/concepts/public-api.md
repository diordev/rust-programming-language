---
title: "Public API"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, api-design, modules]
source_count: 3
---

# Public API

## Short Definition

Public API crate users ishlata oladigan public items to'plami; u users bilan crate orasidagi contract sifatida qaraladi.

## Why It Matters

Public API o'zgarsa, crate'ga depend qilgan code buzilishi mumkin. Shu sababli [[privacy]] va [[pub-keyword|pub]] faqat access control emas, balki [[api-design|API design]] vositasidir.

## Mental Model

Private code ichki implementation detail: uni tashqi code'ni buzmasdan o'zgartirish osonroq. Public API esa tashqi code yozadigan interfeys; u barqarorroq va ongli tanlangan bo'lishi kerak.

[[pub-use|pub use]] internal organization va public API organizationni ajratadi: item ichkarida chuqur module tree'da turishi mumkin, lekin crate users uchun qisqa va domain-friendly path re-export qilinadi.

Trait ham public API bo'lishi mumkin. `pub trait Summary` crate usersga shared behavior contractini beradi; users traitni scope'ga olib method chaqirishi yoki orphan rule ruxsat bergan holatda o'z local typelari uchun implement qilishi mumkin.

## Syntax and Examples

```rust
pub fn eat_at_restaurant() {
    crate::front_of_house::hosting::add_to_waitlist();
}
```

Bu function library crate'dan tashqaridagi code chaqirishi mumkin bo'lgan public API qismiga aylanadi.

Binary + library package patternida `src/lib.rs` public API'ni beradi, `src/main.rs` esa shu API'ni external crate kabi ishlatadi.

```rust
pub use crate::front_of_house::hosting;
```

Bu line external code uchun `restaurant::hosting` public pathini yaratadi.

Public trait:

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

## Common Mistakes

- Har bir helper functionni `pub` qilish.
- Public struct fields ham API contract ekanini unutish.
- Binary crate ichidan library internallariga to'g'ridan-to'g'ri tayanib, API designni tekshirmaslik.
- Re-export qilingan pathni keyin oson o'zgartirsa bo'ladi deb o'ylash.
- Public trait method signatures ham API contract ekanini unutish.
- Public traitni implement qilish imkoniyati [[orphan-rule|orphan rule]] bilan cheklanishini hisobga olmaslik.

## Related Concepts

- [[api-design|API design]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[library-crate|library crate]]
- [[binary-crate|binary crate]]
- [[paths]]
- [[pub-use|pub use]]
- [[traits]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[orphan-rule|orphan rule]]

## Sources

- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
