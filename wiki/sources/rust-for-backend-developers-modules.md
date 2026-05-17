---
title: "Модули - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, modules]
source_count: 1
---

# Модули - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/26. Модули.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/modules.html

## Detailed Summary

Bu source modulelarni ikki amaliy sabab bilan kiritadi: name conflictlarni ajratish va codeni mantiqiy guruhlarga bo'lish. Beginner darajadagi asosiy model to'g'ri: module namespace va visibility boundary beradi.

Birinchi usul inline `mod name { ... }`. Bu kichik yoki shartli kompilyatsiya qilinadigan kod bloklari uchun qulay. Source `a::get_num()` va `b::get_num()` misoli bilan module path name conflictni qanday hal qilishini ko'rsatadi.

Visibility qismi aniq: module ichidagi items private by default. Tashqaridan ishlatish uchun `pub` kerak. Bu faqat functionga emas, boshqa itemlarga ham tegishli umumiy qoida.

Ikkinchi usul alohida `*.rs` fayl. Crate root yoki parent module ichidagi `mod my_module;` declaration compilerga shu module kodini boshqa fayldan topishni bildiradi. Muhim farq: `mod` module code'ini crate tree'ga ulaydi, `use` esa faqat existing path'ni current scope ichida qisqartiradi.

Uchinchi usul katalog + `mod.rs` patterni. Source buni parent module bir nechta submodulelarga bo'linganda ko'rsatadi. Bu pattern supported, lekin wiki hozirgi Rust amaliyotida `name.rs` + `name/child.rs` style ham mavjudligini saqlab yozadi; `mod.rs` yagona yo'l emas.

Source `main.rs` va unga ulangan modules "go'yo bitta katta faylga yig'ilgandek" compile bo'lishini aytadi. Buni literal textual include deb qabul qilish noto'g'ri, lekin beginner mental model sifatida foydali: compiler crate rootdan boshlaydi va shu crate ichidagi module tree'ni bitta compile unit sifatida ko'radi.

`use` misoli uzun pathni current scope ichida qisqa ishlatishga xizmat qiladi. U privacy'ni chetlab o'tmaydi va module file'ni compile qilmaydi. `mod` va `use`ni bir xil narsa deb tushunish shu source bo'yicha ham eng ko'p chalkashadigan joy.

## Key Concepts

- [[module-system|module system]]
- [[module]]
- [[pub-keyword|pub keyword]]
- [[use-declarations|use declarations]]
- [[mod-declarations|mod declarations]]
- [[module-file-layout|module file layout]]
- [[crate]]
- [[crate-root|crate root]]

## Code Examples

```rust
mod a {
    pub fn get_num() -> i32 { 1 }
}

mod b {
    pub fn get_num() -> i32 { 2 }
}
```

```rust
mod my_module;
use my_module::get_num;
```

```rust
// my_math/mod.rs
pub mod add;
pub mod mul;
```

## Exercises or Practice Ideas

- Bitta inline `mod` va bitta alohida file module ishlatadigan kichik project skeleti yozing.
- `pub`ni olib tashlab ko'ring va compiler qaysi visibility xatosini berishini yozib oling.
- `mod` va `use`ni aralashtirmaslik uchun bitta paragraph mental model yozing.

## Questions Raised

- Kichik binary projectda qachon inline `mod {}` yetarli, qachon file'ga ajratish kerak?
- Public module ichida private helper qoldirish API design nuqtai nazaridan qachon foydali?
- `mod.rs` style'dan qachon voz kechib, modern file layout ishlatish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-modules]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[module-system|module system]]
- [[module]]
- [[crate]]
