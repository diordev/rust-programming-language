---
title: "Macro"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, macros]
source_count: 4
---

# Macro

## Short Definition

Macro Rustda code yozadigan code mexanizmi; macro call odatda `!` bilan ko'rinadi. Kengroq aytganda, macro compile-time [[metaprogramming]] vositasi.

## Why It Matters

`println!` beginner code'dagi birinchi macro bo'lib, function callga o'xshasa ham compile-time expansionga ega. Advanced Rust'da macro'lar boilerplateni kamaytirish, variable number of arguments qabul qilish, va trait implementation generatsiya qilish uchun ishlatiladi. Appendix B esa macro syntax'ning reference shaklini beradi.

## Mental Model

Function runtime'da chaqiriladi; macro esa compile jarayonida codega expand bo'lishi mumkin.

Declarative macro'larda bu expansion `macro_rules!` patternlari orqali yoziladi. Practical natija shuki, macro function qilolmaydigan ikki ishni ko'p qiladi: variable number of arguments qabul qilish va items/expressions generatsiya qilish.

Rust macro oilasi:

- [[declarative-macros|Declarative macros]] — `macro_rules!`, pattern/replacement.
- [[procedural-macros|Procedural macros]] — `TokenStream -> TokenStream`.
- [[custom-derive-macros|Custom derive macros]] — `#[derive(Name)]`.
- [[attribute-like-macros|Attribute-like macros]] — `#[route(...)]`.
- [[function-like-macros|Function-like macros]] — `sql!(...)`.

## Syntax and Examples

```rust
println!("Hello, world!");
```

```rust
let v = vec![1, 2, 3];
```

```rust
#[derive(Debug)]
struct Point {
    x: i32,
    y: i32,
}
```

Invocation delimiterlari:

```rust
ident!(...)
ident!{...}
ident![...]
```

Hammasi macro invocation; delimiter tanlovi macro dizayniga bog'liq.

Ba'zi item-generating macro'larda `!{ ... }` varianti trailing semicolonsiz ham ishlatiladi.

## Common Mistakes

- `println!`dagi `!` belgisini unutish.
- Macro va function bir xil deb o'ylash.
- Macro yozish kerak bo'lmagan joyda macro ishlatish; avval function, generic, trait yetarlimi tekshiriladi.
- Macro definition call qilinishidan oldin scope'da bo'lishi kerakligini unutish.

## Related Concepts

- [[println-macro|println! macro]]
- [[vec-macro|vec! macro]]
- [[metaprogramming]]
- [[declarative-macros|declarative macros]]
- [[procedural-macros|procedural macros]]
- [[tokenstream|TokenStream]]
- [[derive-attribute|derive attribute]]
- [[compiler]]
- [[rust-operators-and-symbols]]

## Sources

- [[1-2-hello-world]]
- [[wiki/sources/20-5-macros|20.5 Macros]]
- [[wiki/sources/22-2-b-operators-and-symbols|22.2]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
