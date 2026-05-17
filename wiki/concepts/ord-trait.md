---
title: "Ord Trait"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-17
tags: [rust, traits, comparisons, ordering]
source_count: 2
---

# Ord Trait

## Short Definition

`Ord` trait'i qiymatlar orasida total ordering borligini bildiradi va `cmp` methodini beradi.

## Why It Matters

Sorting, `BTree*` collectionlar, va deterministic comparison uchun `PartialOrd`dan kuchliroq contract kerak bo'ladi.

## Mental Model

`PartialOrd` ba'zan `None` qaytarishi mumkin. `Ord` esa bunday noaniqlikka joy qoldirmaydi: har ikki qiymat uchun `Less`, `Equal`, yoki `Greater` bo'lishi kerak.

Shu sabab `Ord` `Eq + PartialOrd`ga tayanadi.

## Syntax and Examples

```rust
#[derive(PartialEq, Eq, PartialOrd, Ord)]
struct Version {
    major: u8,
    minor: u8,
}
```

```rust
impl PartialOrd for Cargo {
    fn partial_cmp(&self, other: &Self) -> Option<std::cmp::Ordering> {
        Some(self.cmp(other))
    }
}
```

## Common Mistakes

- `Ord`ni `PartialOrd`siz yoki `Eq`siz tasavvur qilish.
- Derived ordering domen semantics'iga mos kelishini tekshirmaslik.
- `Ord` implement qilingan type uchun `PartialOrd` boshqa order logic bilan yozish.

## Related Concepts

- [[partial-ord]]
- [[eq-trait]]
- [[ordering|Ordering]]
- [[derive-attribute]]

## Sources

- [[wiki/sources/22-3-c-derivable-traits|22.3]]
- [[wiki/sources/rust-for-backend-developers-common-traits]]
