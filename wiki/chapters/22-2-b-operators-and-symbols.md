---
title: "22.2. B - Operators and Symbols"
type: chapter
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, syntax, operators, symbols, reference]
source_count: 1
---

# 22.2. B - Operators and Symbols

## Learning Goal

Rust syntax'dagi operatorlar va boshqa belgilarni tez topiladigan reference sifatida tushunish, ayniqsa qaysi operatorlar overload bo'lishi, `::<...>` [[turbofish]] syntax'i, va raw string/comment/macro shakllarini farqlash.

## Main Ideas

- Appendix B Rust syntax atlasidir: operatorlar, paths, generics, trait bounds, macros, comments, va qavslar bir joyda jamlangan.
- Operatorning ma'nosi ko'pincha contextga bog'liq: `*`, `!`, `..`, `::` kabi belgilar turli joyda turli rol o'ynaydi.
- Hamma operatorlar overload bo'lavermaydi; faqat ayrimlari relevant traitlar orqali overload qilinadi.
- `path::<...>` syntax'i expression context'dagi generic argumentlarni ko'rsatadi va odatda [[turbofish]] deb ataladi.
- `r"..."`, `r#"..."#` syntax'lari [[raw-string-literals]] uchun reference beradi.

## Syntax Clusters

- Operators: arithmetic, comparison, bitwise, logical, ranges, assignment, `?`.
- Path syntax: `::`, `self::`, `super::`, `<Type as Trait>::item`.
- Generics: `Vec<u8>`, `"42".parse::<i32>()`, `impl<T>`, `for<'a>`.
- Trait bounds: `T: U`, `T: 'static`, `T: ?Sized`, `Trait + Trait`.
- Macros va comments: `ident!(...)`, `#[meta]`, `///`, `//!`, `/** */`.
- Brackets: `()`, `{}`, `[]`.

## Concepts

- [[rust-operators-and-symbols]]
- [[operator-overloading]]
- [[turbofish]]
- [[raw-string-literals]]
- [[question-mark-operator]]
- [[dereference-operator]]
- [[comments]]
- [[macro]]
- [[where-clauses]]
- [[static-lifetime]]

## Examples

- [[turbofish-parse-example|turbofish parse example]]

## Exercises

1. `*` operatorining Appendix B dagi barcha rollarini yozing.
2. `!` operatorining macro va logical negation kontekstlarini taqqoslang.
3. `Vec<u8>` va `"42".parse::<i32>()` syntax farqini tushuntiring.
4. `///` va `//!` commentlari qayerga tegishli ekanini misol bilan yozing.
5. `expr[expr]` va `expr[a..b]` syntax'larining collection semanticsini farqlang.

## Review Questions

1. Appendix B nimani reference qiladi?
2. Operator overloadability nimani anglatadi?
3. Turbofish nima?
4. Raw string literal qanday yoziladi?
5. `#[meta]` va `#![meta]` orasida qanday farq bor?

## Related Pages

- [[wiki/sources/22-2-b-operators-and-symbols|Source: 22.2]]
- [[wiki/chapters/22-appendix|Chapter 22]]
- [[rust-operators-and-symbols]]
- [[turbofish]]
