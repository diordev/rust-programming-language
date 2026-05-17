---
title: "Rust Operators and Symbols"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, syntax, operators, reference]
source_count: 1
---

# Rust Operators and Symbols

## Short Definition

Rust syntax'da ishlatiladigan operatorlar va boshqa belgilar familyasi: arithmetic/comparison operatorlar, path/generic syntax belgilari, macro/comment/attribute markerlari, va qavs shakllari.

## Why It Matters

Rust'ni o'qish va yozishda bir xil belgi turli kontekstda turli ma'no berishi mumkin. Reference sahifa sifatida operator va symbol oilasini bir joyda tutish parsing mental modelini mustahkamlaydi.

## Mental Model

`*`, `!`, `::`, `..`, `#`, `?` kabi belgilar "single meaning token" emas. Ular code kontekstiga qarab:

- operator,
- type syntax marker,
- macro/attribute delimiter,
- path/generic separator,
- yoki pattern shorthand bo'lishi mumkin.

## Syntax and Examples

Representative misollar:

```rust
!flag
value?
*ptr
"42".parse::<i32>()
r#"C:\temp\file.txt"#
ident!(a, b, c)
/// outer doc comment
```

## Common Mistakes

- Bitta belgining hamma joyda bir xil ma'nosi bor deb o'ylash.
- Overload bo'lmaydigan syntax'ni operator overloading bilan aralashtirish.
- Type context va expression context generic syntax'ini farqlamaslik.

## Related Concepts

- [[operator-overloading]]
- [[turbofish]]
- [[raw-string-literals]]
- [[question-mark-operator]]
- [[dereference-operator]]
- [[comments]]
- [[macro]]
- [[derive-attribute]]

## Sources

- [[wiki/sources/22-2-b-operators-and-symbols|22.2]]
