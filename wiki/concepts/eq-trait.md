---
title: "Eq Trait"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, traits, equality]
source_count: 1
---

# Eq Trait

## Short Definition

`Eq` trait'i type qiymatlari o'ziga har doim teng bo'lishini signal qiladigan marker trait.

## Why It Matters

Bu total equality expectationini bildiradi. Ayniqsa [[hash-map|HashMap]] keylari kabi use case'larda "bir xil key" tushunchasi mustahkam bo'lishi kerak.

## Mental Model

`PartialEq` tenglikni ta'riflaydi; `Eq` esa shu tenglik reflexive ekanini va'da qiladi: har qiymat o'ziga teng.

## Syntax and Examples

```rust
#[derive(PartialEq, Eq)]
enum Status {
    Ready,
    Running,
    Done,
}
```

`Eq`ning methodi yo'q; u semantic marker sifatida ishlaydi.

## Common Mistakes

- Float typelarni `Eq` deb tasavvur qilish.
- `Eq`ni `PartialEq`siz ishlatish mumkin deb o'ylash.

## Related Concepts

- [[partial-eq]]
- [[hash-trait]]
- [[hash-map|HashMap]]
- [[derive-attribute]]

## Sources

- [[wiki/sources/22-3-c-derivable-traits|22.3]]
