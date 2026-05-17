---
title: "Ord Trait"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, traits, comparisons, ordering]
source_count: 1
---

# Ord Trait

## Short Definition

`Ord` trait'i qiymatlar orasida total ordering borligini bildiradi va `cmp` methodini beradi.

## Why It Matters

Sorting, ordered collectionlar, va deterministic comparison uchun `PartialOrd`dan kuchliroq contract kerak bo'ladi. `BTreeSet<T>` shunday misol.

## Mental Model

`PartialOrd` ba'zan "taqqoslab bo'lmaydi" deb `None` qaytarishi mumkin; `Ord` esa har ikkita qiymat uchun tartib borligini va'da qiladi.

## Syntax and Examples

```rust
#[derive(PartialEq, Eq, PartialOrd, Ord)]
struct Version {
    major: u8,
    minor: u8,
}
```

Derive bo'lganda struct field orderi va enum variant declaration orderi bo'yicha comparison qilinadi.

## Common Mistakes

- `Ord`ni `PartialOrd`siz yoki `Eq`siz tasavvur qilish.
- Derived ordering domen semantics'iga mos kelishini tekshirmaslik.

## Related Concepts

- [[partial-ord]]
- [[eq-trait]]
- [[ordering|Ordering]]
- [[derive-attribute]]

## Sources

- [[wiki/sources/22-3-c-derivable-traits|22.3]]
