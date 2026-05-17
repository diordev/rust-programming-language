---
title: "Переменные - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, variables]
source_count: 1
---

# Переменные - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/8. Переменные.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/variables.html

## Detailed Summary

Bu source Rustdagi variable modelini bir nechta qatlamda ochadi: `let` orqali binding e'lon qilish, type inference, delayed initialization, default immutability, `mut`, identifier rules, `const`, `static`, va `_` bilan bog'liq ikki xil pattern.

Asosiy boshlanish sintaksisi `let name: Type = value;`. Source muhim yengillikni ko'rsatadi: agar initial value bor bo'lsa, type ko'pincha compiler tomonidan infer qilinadi. Bundan tashqari bindingni avval e'lon qilib, keyin initializatsiya qilish ham mumkin: `let a; a = 5;`. Bu hali mutable degani emas; bu faqat bir martalik initial assignment.

Immutability Rustning default'i. Source `E0384 cannot assign twice to immutable variable` misoli bilan shuni ko'rsatadi. Qiymat keyinroq o'zgarishi kerak bo'lsa, binding `mut` bilan e'lon qilinadi. Bu oddiy syntax emas, balki intent signalidir: qaysi data qayta yozilishi mumkinligini koddan tez ko'rish mumkin.

Identifier qoidalari ham amaliy: Unicode harflari, raqamlar, `_` ishlatish mumkin, lekin name raqam bilan boshlanmaydi va keyword ishlatilmaydi. Shu yerda [[raw-identifiers|raw identifiers]] uchun `r#` escape ham beriladi: `let r#if = 5;`. Bu serialization yoki edition-compatibility holatlarida kerak bo'lishi mumkin.

Source `const` va `static`ni bir-biridan ajratadi. `const` explicit type talab qiladi va odatda uppercase snake case bilan yoziladi. `static` esa program davomida yashaydigan bitta storage location sifatida tushuntiriladi; mutable `static` esa concurrent mutation tufayli unsafe boundaryga kiradi. Source storage segmentlari haqida ham gapiradi, lekin wiki uchun bu qismni faqat mental model sifatida olish kerak.

Oxirgi ikki pattern amaliy: `_name` unused warningni bosadi, lekin binding baribir mavjud bo'ladi; `_` esa discarded binding bo'lib, qiymatni ataylab e'tiborsiz qoldirish uchun ishlatiladi va unga keyin murojaat qilib bo'lmaydi.

## Key Concepts

- [[variables-and-mutability|variables and mutability]]
- [[immutability]]
- [[constants]]
- [[static-items]]
- [[raw-identifiers]]
- [[discarded-binding]]
- [[type-inference|type inference]]
- [[type-annotations|type annotations]]
- [[unsafe-rust|unsafe Rust]]
- [[data-race|data race]]
- [[e0384-cannot-assign-twice|E0384 cannot assign twice]]

## Code Examples

```rust
let a: i32 = 5;
let b = 5;

let c;
c = 5;
```

```rust
let mut counter = 1;
counter = 5;
```

```rust
const PI: f32 = 3.14;
static APP_NAME: &str = "backend-service";
```

```rust
let r#if = 5;
let _reserved_for_later = 10;
let _ = compute_value();
```

## Exercises or Practice Ideas

- `let`, `mut`, `const`, va `static` uchun bir xil domain misolida to'rtta alohida declaration yozing.
- `_name` va `_` orasidagi farqni ko'rsatadigan minimal misol yozing.
- `mut` bilan hal bo'ladigan holat va [[shadowing]] kerak bo'ladigan holatni yonma-yon solishtiring.

## Questions Raised

- Qachon `mut` yetarli, qachon [[shadowing]] ma'qul?
- Ignored `Result` uchun `let _ = ...;` ishlatish signalni pasaytirib yubormaydimi?
- `const` va `static` orasidagi farqni compile-time vs single storage modeli bilan qanchalik aniq tushuntirish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-variables]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[variables-and-mutability|variables and mutability]]
- [[constants]]
- [[static-items]]
- [[raw-identifiers]]
- [[discarded-binding]]
