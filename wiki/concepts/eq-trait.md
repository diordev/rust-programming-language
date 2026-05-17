---
title: "Eq Trait"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-17
tags: [rust, traits, equality]
source_count: 2
---

# Eq Trait

## Short Definition

`Eq` trait'i type qiymatlari uchun total equality contractini signal qiladigan marker trait.

## Why It Matters

Ba'zi algorithm va collectionlar equality reflexive bo'lishini kutadi. `Eq` shu expectationni type darajasida belgilaydi.

## Mental Model

`PartialEq` tenglik operatsiyasini beradi. `Eq` esa qo'shimcha semantic va'da beradi: har qiymat o'ziga teng bo'lishi kerak. Shuning uchun `Eq`ning o'z methodi yo'q; u contract marker.

Floatlarda `NaN != NaN`, shu sabab `f32` va `f64` `Eq` emas.

## Syntax and Examples

```rust
#[derive(PartialEq, Eq)]
enum Status {
    Ready,
    Running,
    Done,
}
```

## Common Mistakes

- Float typelarni `Eq` deb tasavvur qilish.
- `Eq` `PartialEq`siz ishlaydi deb o'ylash.
- `Eq` derive bo'lsa semantic contract avtomatik hamma domainga mos tushadi deb taxmin qilish.

## Related Concepts

- [[partial-eq]]
- [[hash-trait]]
- [[ordering]]
- [[derive-attribute]]

## Sources

- [[wiki/sources/22-3-c-derivable-traits|22.3]]
- [[wiki/sources/rust-for-backend-developers-common-traits]]
