---
title: "Структуры - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, structs]
source_count: 1
---

# Структуры - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/25. Структуры.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/structs.html

## Detailed Summary

Bu source `struct`ni named fields orqali yangi custom type yaratish vositasi sifatida beradi. Durable nuqta oddiy: struct tuple yoki parallel variablelar o'rniga ma'noli shape beradi. Nomlash qoidalari ham aniq berilgan: struct nomi PascalCase, field nomlari snake_case.

Instance yaratish struct literal orqali qilinadi, field access esa `instance.field` bilan. Source field init shorthand'ni ham ko'rsatadi: local variable nomi field nomi bilan bir xil bo'lsa `field: field` o'rniga faqat `field` yoziladi. Bu constructor-like functionlarda takrorni kamaytiradi.

Mutability qismi muhim: Rust fieldni alohida `mut` qilmaydi. Agar struct instance fieldlari o'zgartirilsa, butun instance `let mut` bilan e'lon qilingan bo'lishi kerak. Bu structni ham oddiy binding qoidalariga bo'ysundiradi.

Struct update syntax `..old_instance` yangi instance qurishda ko'p boilerplateni kesadi, lekin ownership qoidalarini chetlab o'tmaydi. `Copy` bo'lmagan fieldlar move bo'lishi mumkin. Shu sabab `..old_instance` shunchaki "clone all fields" emas.

Methods qismi Rustning data va behavior ajratilgan modelini ko'rsatadi: struct definition data shape beradi, `impl` block esa methods va associated functionsni beradi. `&self`, `&mut self`, va `self` receiverlari turli ownership semanticsga ega. `new` kabi associated functionlar type namespace orqali chaqiriladi va ko'pincha constructor rolini bajaradi.

Tuple structlar positional fields bilan ishlaydi, lekin ularga type name va methods qo'shish mumkin. Source RGB misoli bilan `.0`, `.1`, `.2` access, destructuring, va method qo'shishni ko'rsatadi. Unit-like struct esa fieldsiz type: data saqlamaydi, lekin behavior yoki marker sifatida foydali bo'lishi mumkin.

Struct lifetime qismi beginnerlar uchun foydali: struct ichida reference field saqlansa, lifetime annotation kerak bo'ladi. Bu annotation reference umrini uzaytirmaydi; u struct instance qaysi borrowed data'dan uzoq yashamasligini compilerga bildiradi.

Source oxirida memory layout bo'limi bor. Wiki uchun saqlanadigan barqaror nuqta shuki: field order memoryda source code'dagi tartibga majburiy teng emas, padding va alignment bo'lishi mumkin, `#[repr(C)]` esa C bilan mos layout kerak bo'lganda ishlatiladi. Lekin source'dagi aniq size/address natijalari platforma va compiler versiyasiga bog'liq; ular universal invariant emas.

## Key Concepts

- [[structs]]
- [[struct-fields|struct fields]]
- [[struct-instances|struct instances]]
- [[field-init-shorthand]]
- [[struct-update-syntax]]
- [[methods]]
- [[impl-block|impl block]]
- [[associated-functions]]
- [[tuple-structs]]
- [[unit-like-structs]]
- [[lifetimes]]

## Code Examples

```rust
struct Person {
    first_name: String,
    last_name: String,
}

let person = Person {
    first_name: String::from("John"),
    last_name: String::from("Doe"),
};
```

```rust
let first_name = String::from("John");
let person = Person {
    first_name,
    last_name: String::from("Doe"),
};
```

```rust
impl Person {
    fn new(first: &str, last: &str) -> Person {
        Person {
            first_name: first.to_string(),
            last_name: last.to_string(),
        }
    }
}
```

```rust
#[derive(Debug)]
struct NameComponents<'a> {
    first_name: &'a str,
    last_name: &'a str,
}
```

## Exercises or Practice Ideas

- `User` yoki `DbConfig` struct yozib, field init shorthand bilan constructor-like `new` yarating.
- `..old_instance` ishlatib, qaysi fieldlar move bo'lishini `String` va `u32` bilan taqqoslang.
- Tuple struct bilan rang yoki koordinata modelini yozib, `.0` access va destructuringni birga ishlating.

## Questions Raised

- Qachon `String` field o'rniga `&str` field ishlatish haqiqatan foydali, qachon esa ortiqcha murakkablik beradi?
- `self` bilan method yozish va free function yozish orasidagi API farqi qachon sezilarli bo'ladi?
- `#[repr(C)]` backend application code'da qaysi real holatda kerak bo'ladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-structs]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[structs]]
- [[methods]]
- [[module]]
