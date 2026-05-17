---
title: "Abstraction"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, abstraction]
source_count: 1
---

# Abstraction

## Short Definition

Abstraction - repeated concrete details o'rniga umumiy concept yoki interface bilan ishlash.

## Why It Matters

Rustda abstraction code duplicationni kamaytiradi va intentni aniqroq qiladi. Functionlar value-level abstraction, generics esa type-level abstraction beradi.

## Mental Model

`largest` functioni har safar concrete list code'ini yozish o'rniga "listdagi eng kattani topish" conceptini nomlaydi. Generic function esa "faqat i32 emas, kerakli behaviorga ega har qanday type" ustida ishlashga tayyorlaydi.

## Syntax and Examples

```rust
fn largest(list: &[i32]) -> &i32 {
    // implementation hidden behind the function name
}
```

## Common Mistakes

- Har duplicate code uchun darhol murakkab abstraction yaratish.
- Abstraction qaysi conceptni ifodalayotganini nomda ko'rsatmaslik.
- Abstraction ownership va lifetime relationshipsni yashiradi deb o'ylash; Rust signature'da bu relationships baribir ko'rinadi.

## Related Concepts

- [[functions]]
- [[generics]]
- [[traits]]
- [[code-duplication|code duplication]]
- [[zero-cost-abstractions|zero-cost abstractions]]

## Sources

- [[wiki/sources/10-generic-types-traits-and-lifetimes]]
