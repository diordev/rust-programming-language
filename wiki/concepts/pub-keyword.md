---
title: "pub Keyword"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, modules, keywords]
source_count: 4
---

# pub Keyword

## Short Definition

`pub` Rust item'ini yoki module'ini public qilib, privacy chegarasidan tashqarida ishlatishga ruxsat beradigan keyword.

## Why It Matters

Rustda module ichidagi code private by default. `pub` public API'ni explicit qiladi: qaysi module, struct, function yoki boshqa item tashqi code ishlatishi mumkinligini developer aniq belgilaydi. Backend beginner source aynan shu default-private qoidani module mavzusining markaziga qo'yadi.

## Mental Model

`pub` eshik ochadi, lekin faqat qo'yilgan joyida. `pub mod garden;` module nomini ochadi; `garden` ichidagi item'lar tashqaridan ishlatilishi uchun ular ham `pub` bo'lishi kerak.

`pub struct` struct type'ni public qiladi, lekin fields private by default qoladi. `pub enum` esa variantsni public qiladi.

`pub use` `pub` va `use`ni birlashtirib, item'ni yangi public path orqali re-export qiladi.

## Syntax and Examples

```rust
pub mod garden;
```

```rust
pub struct Asparagus {}
```

Backyard example'da `pub mod garden;`, `pub mod vegetables;`, va `pub struct Asparagus {}` birga ishlatilgani uchun `main.rs` `crate::garden::vegetables::Asparagus` path'i orqali struct'ni ishlata oladi.

Restaurant example'da function call ishlashi uchun module ham, function ham public bo'lishi kerak:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}
```

Public struct field:

```rust
pub struct Breakfast {
    pub toast: String,
    seasonal_fruit: String,
}
```

Public enum:

```rust
pub enum Appetizer {
    Soup,
    Salad,
}
```

Re-export:

```rust
pub use crate::front_of_house::hosting;
```

## Common Mistakes

- `pub mod` ichidagi barcha item'larni avtomatik public qiladi deb o'ylash.
- `pub`ni ortiqcha ishlatib, implementation detail'larni API surface'ga chiqarib yuborish.
- `pub` privacy'ni compile-time name access bilan bog'lashini unutish.
- `pub struct` fieldsni ham avtomatik public qiladi deb o'ylash.
- `pub enum` variants alohida `pub` talab qiladi deb o'ylash.
- `pub use` original item joyini o'zgartiradi deb o'ylash.

## Related Concepts

- [[privacy]]
- [[public-api|public API]]
- [[module]]
- [[module-system|module system]]
- [[paths]]
- [[struct-fields|struct fields]]
- [[enum-variants|enum variants]]
- [[api-design|API design]]
- [[keywords]]
- [[pub-use|pub use]]

## Sources

- [[7-2-control-scope-and-privacy-with-modules]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword]]
- [[wiki/sources/rust-for-backend-developers-modules]]
