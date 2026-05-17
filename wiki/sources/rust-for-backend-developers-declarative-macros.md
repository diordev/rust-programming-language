---
title: "Декларативные макросы - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, macros]
source_count: 1
---

# Декларативные макросы - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/23. Декларативные макросы.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/declarative-macro.html

## Detailed Summary

Bu source Rust macro oilasini ikkiga ajratadi: declarative va procedural. Wiki uchun muhim, durable boundary shuki: `macro_rules!` pattern/replacement usulida Rust syntax shakllarini match qiladi, procedural macros esa token streamlar bilan ishlaydi. Source ichidagi "kompilyatorning aniq qaysi bosqichida ishlaydi" tafsilotlari soddalashtirilgan, shuning uchun wiki semantik farqni saqlaydi, ichki compiler pipeline tafsilotlarini emas.

Declarative macros `#[macro_export]` va `macro_rules!` bilan e'lon qilinadi. Asosiy model:

```rust
macro_rules! name {
    pattern => { expansion }
}
```

Macro chaqiruvi compile-time'da Rust kodiga expand bo'ladi. `sum_nums!(1, 2)` misoli aynan shuni ko'rsatadi: generated code keyin oddiy Rust sifatida compile qilinadi. Source bu expansionni ko'rish uchun `cargo expand` utilitasini ham tilga oladi.

Fragment specifierlar qismi foydali reference bo'lib keladi: `expr`, `stmt`, `ty`, `path`, `pat`, `item`, `block`, `meta`, `ident`, `tt`, `literal`, `vis`. Bular macro argument capture'lari "har qanday token" emasligini ko'rsatadi; aynan qaysi Rust syntactic kategoriya kutilayotgani aniq yoziladi.

Keyingi muhim qism repetition: `$( ... ),*` va shu oiladagi syntax variadic-like chaqiruvlarni yozishga imkon beradi. Source bu nuqtani Rust functionlar variadic bo'lmagani bilan bog'laydi va `vec![]` macro nega function emasligini tushuntiradi.

Delimiter qismi practical: `!()`, `![]`, `!{}` hammasi macro invocation. Asosiy farq expression bo'lishida emas, ba'zi item-generating macro'larda `!{}` ishlatilganda trailing semicolon keraksiz bo'lishi mumkin.

Oxirida source o'quv `vec2!` macro'sini beradi. Bu `vec![]`ning real std implementationi emas, lekin expansion mental modelini aniq ko'rsatadi: `Vec::new()`, repeated `push`, va final temporary return.

## Key Concepts

- [[macro]]
- [[declarative-macros|declarative macros]]
- [[procedural-macros|procedural macros]]
- [[vec-macro|vec! macro]]
- [[simplified-vec-macro]]
- [[statements-and-expressions|statements and expressions]]
- [[pattern-matching]]
- [[identifiers]]

## Code Examples

```rust
#[macro_export]
macro_rules! sum_nums {
    ( $x:expr, $y:expr ) => { $x + $y }
}
```

```rust
#[macro_export]
macro_rules! sum_nums {
    ( $( $rest:expr ),* ) => { 0 $( + $rest )* }
}
```

```rust
sum_nums!(1, 2);
sum_nums![1, 2];
sum_nums!{1, 2};
```

```rust
#[macro_export]
macro_rules! make_empty_func {
    ($func_name:ident) => {
        fn $func_name() {}
    }
}
```

## Exercises or Practice Ideas

- `expr`, `ident`, `ty`, va `literal` fragmentlari uchun to'rttadan macro chaqiruv misoli yozing.
- `$(...),*` repetition bilan bo'sh input va ko'p input holatlarini sinab ko'ring.
- `vec![]` nima uchun function emasligini bitta paragraph bilan tushuntiring.

## Questions Raised

- `macro_rules!` patternlari runtime pattern matchingdan qaysi nuqtada keskin farq qiladi?
- Fragment specifierlar noto'g'ri tanlansa compiler xatosi qanday yaxshilanadi yoki yomonlashadi?
- Item-generating macro uchun `!{}` va `!()` ergonomik farqi qachon seziladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-declarative-macros]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[macro]]
- [[declarative-macros|declarative macros]]
- [[vec-macro|vec! macro]]
