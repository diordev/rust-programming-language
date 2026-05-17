---
title: "Metaprogramming"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, macros, code-generation]
source_count: 1
---

# Metaprogramming

## Short Definition

Code yozadigan code. Rustda metaprogrammingning asosiy ko'rinishi macro'lar: ular compile-time'da Rust code generatsiya qiladi.

## Why It Matters

Metaprogramming repetitive code'ni kamaytiradi va compile-time'da pattern asosida implementation yaratishga imkon beradi. `println!`, `vec!`, `#[derive(Debug)]`, va custom derive macro'lar shunga misol.

## Mental Model

Oddiy function value'lar ustida ishlaydi. Macro esa source code tokenlari yoki patternlari ustida ishlaydi va compiler ko'radigan yangi code hosil qiladi.

```
macro input tokens -> macro expansion -> Rust compiler continues
```

## Syntax and Examples

```rust
let v = vec![1, 2, 3];
```

Soddalashtirilgan expansion:

```rust
{
    let mut temp_vec = Vec::new();
    temp_vec.push(1);
    temp_vec.push(2);
    temp_vec.push(3);
    temp_vec
}
```

## Common Mistakes

- Macro har doim functiondan yaxshi deb o'ylash. Function, generic, trait yetarli bo'lsa, avval shularni tanlash kerak.
- Macro runtime'da ishlaydi deb o'ylash; macro expansion compile-time bosqich.
- Generated code'ni ko'rmasdan macro behaviorini taxmin qilish.

## Related Concepts

- [[macro|macros]]
- [[declarative-macros|declarative macros]]
- [[procedural-macros|procedural macros]]
- [[compiler]]
- [[code-duplication|code duplication]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
