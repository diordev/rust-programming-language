---
title: "22.2. B - Operators and Symbols"
type: source
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, syntax, operators, symbols, reference]
source_count: 1
---

# 22.2. B - Operators and Symbols

## Source

- File: `raw/books/the_rust_programming_language/22.2. B - Operators and Symbols.md`
- URL: <https://doc.rust-lang.org/stable/book/appendix-02-operators.html>
- Book: *The Rust Programming Language*

## Detailed Summary

Appendix B Rust syntax'ni bitta joyga jamlaydigan reference sahifa. Bu bo'lim oddiy operator ro'yxatidan kengroq: operatorlar, non-operator symbols, path syntax, generics syntax, trait bound syntax, macros va attributes syntax, comment forms, hamda qavslarning turli ishlatilishlarini jadval ko'rinishida beradi.

### Operatorlar va overloadability

Birinchi katta jadval Rust operatorlarini to'rtta practical savol bo'yicha beradi:

1. operatorning o'zi;
2. u code ichida qanday ko'rinadi;
3. qisqa semantic ma'nosi;
4. overload qilinishi mumkinmi, mumkin bo'lsa qaysi trait orqali.

Bu joyning eng muhim takeaway'i: Rustdagi hamma belgi overload bo'lavermaydi. Faqat ayrim operatorlar `std::ops` yoki comparison traitlari orqali overload qilinadi. Masalan:

- `!expr` -> `Not`
- `expr != expr` va `expr == expr` -> [[partial-eq|PartialEq]]
- `expr % expr` -> `Rem`
- `expr & expr` -> `BitAnd`
- `expr * expr` -> `Mul`
- `expr + expr` -> `Add`
- `expr - expr` -> `Sub`
- `expr / expr` -> `Div`
- `expr << expr` -> `Shl`
- `expr >> expr` -> `Shr`
- `expr ^ expr` -> `BitXor`
- `expr | expr` -> `BitOr`
- comparisonlar `<`, `<=`, `>`, `>=` -> [[partial-ord|PartialOrd]]
- indexing `expr[expr]` -> `Index` yoki `IndexMut`

Shu bilan birga bir xil belgi turli kontekstda turli ma'no beradi. `*` bunga aniq misol:

- `expr * expr` — multiplication;
- `*expr` — [[dereference-operator|dereference]];
- `*const T`, `*mut T` — raw pointer type syntax.

`!` ham ikki rolga ega:

- `ident!(...)` — [[macro]] invocation;
- `!expr` — logical/bitwise complement.

### Non-operator symbols

Keyingi jadvallar operator bo'lmagan, lekin syntax'da muhim bo'lgan belgilarni beradi:

- `'ident` — named lifetime yoki loop label;
- `"..."` — string literal;
- `r"..."`, `r#"..."#` — [[raw-string-literals]];
- `b"..."` — byte string;
- `br"..."` — raw byte string;
- `'c'` va `b'c'` — char va byte literal;
- `|...| expr` — closure;
- `!` — never type;
- `_` — ignored binding yoki integer readability marker.

Bu appendix ayniqsa string literal oilasini compact ko'rsatgani bilan foydali: oddiy string, raw string, byte string, va raw byte string bir jadvalda yonma-yon turadi.

### Path syntax

Path bo'limi `::` oilasini reference shaklida beradi:

- `ident::ident` — namespace path;
- `::path` — crate-root relative absolute path;
- `self::path` — current module relative path;
- `super::path` — parent module relative path;
- `type::ident`, `<type as trait>::ident` — associated items;
- `trait::method(...)`, `type::method(...)`, `<type as trait>::method(...)` — method disambiguation.

Bu jadval fully qualified syntax va path resolution sahifalarini bir joyga bog'laydi.

### Generics va turbofish

Generics jadvalidagi amaliy eng muhim elementlardan biri:

```rust
"42".parse::<i32>()
```

ya'ni expression context'dagi `::<...>` syntax, odatda [[turbofish]] deb ataladi. Book buni generic type, function, yoki method parameterlarini expression ichida aniq ko'rsatish usuli sifatida beradi.

Shu jadval yana:

- `path<...>` — type context'dagi generic parameterlar;
- `fn ident<...>` — generic function definition;
- `struct ident<...>`, `enum ident<...>`, `impl<...>` — generic item definitions;
- `for<...>` — higher-ranked lifetime bounds;
- `Type<Item=T>` — associated type assignment.

### Trait bounds

Appendix B trait bound syntax'ni ham birdan ko'rsatadi:

- `T: U`
- `T: 'a`
- `T: 'static`
- `'b: 'a`
- `T: ?Sized`
- `'a + Trait`, `Trait + Trait`

Bu [[where-clauses]], [[static-lifetime]], [[sized-trait|Sized trait]], va compound bounds konseptlarini syntax reference sifatida mustahkamlaydi.

### Macros, attributes, comments, va qavslar

Keyingi bo'limlar syntax lookup uchun juda qulay:

- `#[meta]` va `#![meta]` — outer/inner attribute;
- `$ident`, `$ident:kind`, `$(...)...` — macro pattern syntax;
- `ident!(...)`, `ident!{...}`, `ident![...]` — macro invocationning uch delimiter varianti;
- `//`, `//!`, `///`, `/* */`, `/*! */`, `/** */` — comment va doc comment oilasi;
- `()`, `{}`, `[]` — tuple, block/struct literal, va array/index/slicing kontekstlari.

### Yakuniy takeaway

Appendix B "Rust syntax atlas" sifatida ishlaydi. Undan eng ko'p qayta ishlatiladigan practical nuqtalar:

- operator qaysi trait bilan overload bo'lishi;
- `::<...>` turbofish;
- raw string literal syntax;
- comment/doc comment va macro/attribute shape'lari;
- `*`, `!`, `..`, `::` kabi belgilar contextga qarab ma'no o'zgartirishi.

## Key Concepts

- [[rust-operators-and-symbols]]
- [[operator-overloading]]
- [[turbofish]]
- [[raw-string-literals]]
- [[question-mark-operator]]
- [[dereference-operator]]
- [[comments]]
- [[macro]]

## Code Examples

- [[turbofish-parse-example|turbofish parse example]]

## Exercises or Practice Ideas

1. `*` operatorining Appendix B dagi uch xil rolini misol bilan yozing.
2. Qaysi operatorlar overload bo'lishi, qaysilari bo'lmasligini 5 ta misolda taqqoslang.
3. `path<...>` va `path::<...>` o'rtasidagi kontekst farqini tushuntiring.
4. `r#"..."#` va `"..."` string literal'lari qaysi vaziyatda farq qilishini yozing.
5. Comment syntax bo'limidan inner va outer doc commentlar farqini qisqacha izohlang.

## Questions Raised

- Rust syntax ichida qaysi belgilar context-sensitive ma'noga eng ko'p ega?
- Operator overloadability bo'yicha qaysi design qarorlari intentionally cheklangan?
- `attributes` uchun alohida durable concept page kerak bo'ladimi?

## Links To Update

- [[index|Rust Wiki Index]]
- [[overview]]
- [[log]]
- [[wiki/chapters/22-2-b-operators-and-symbols|Chapter 22.2]]
- [[rust-operators-and-symbols]]
- [[turbofish]]
- [[raw-string-literals]]
