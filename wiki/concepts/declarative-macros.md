---
title: "Declarative Macros"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-16
tags: [rust, macros, macro-rules]
source_count: 2
---

# Declarative Macros

## Short Definition

`macro_rules!` bilan yoziladigan pattern/replacement macro turi. "Macros by example" deb ham ataladi.

## Why It Matters

Declarative macro Rust syntax patternlarini match qilib, code expansion yaratadi. `vec!`, `println!`, va ko'p oddiy macro'lar shu oilaga kiradi.

## Mental Model

`match` value patternlariga qaraydi; `macro_rules!` source code token structure patternlariga qaraydi. Pattern mos kelsa, replacement code compilerga beriladi.

Source declarative macro'larni procedural macro'lardan ergonomik jihatdan ajratadi: declarative macro syntax categories (`expr`, `ident`, `ty`, `stmt`, `block`, `item`, `tt`, va hokazo) bilan ishlaydi va backend code yozishda odatda aynan shu tur yetarli bo'ladi.

## Syntax and Examples

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

Pattern qismlari:

- `$x:expr` - expression fragment capture.
- `$()` - repetition group.
- `,` - separator.
- `*` - zero or more.

Delimiterlar ham moslashuvchan:

```rust
sum_nums!(1, 2);
sum_nums![1, 2];
sum_nums!{1, 2};
```

## Common Mistakes

- Macro pattern syntaxini Chapter 19 pattern matching bilan bir xil deb o'ylash.
- `$x`ni oddiy Rust variable deb tushunish; u macro variable.
- `#[macro_export]` bo'lmasa macro crate tashqarisiga chiqmaydi.
- Macro scope qoidalarini unutish: macro call qilinishidan oldin definition scope'da bo'lishi kerak.
- Macro patternni runtime `match` patterni bilan bir xil mexanizm deb tushunish.
- `item` generatsiya qiluvchi macro'larda semicolon qoidalari delimiterga qarab o'zgarishini unutish.

## Related Concepts

- [[macro|macros]]
- [[metaprogramming]]
- [[vec-macro|vec! macro]]
- [[simplified-vec-macro]]
- [[procedural-macros|procedural macros]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
