---
title: "Higher-Ranked Trait Bounds"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, generics, traits, lifetimes]
source_count: 1
---

# Higher-Ranked Trait Bounds

## Short Definition

`for<'a>` kabi syntax bilan trait bound har qanday lifetime uchun ishlashini bildiradigan advanced bound.

## Why It Matters

Serde `DeserializeOwned` `for<'de> Deserialize<'de>` orqali input lifetime parametrini callerga yuklamasdan ifodalaydi.

## Mental Model

Oddiy lifetime bound bitta lifetime haqida gapiradi; HRTB "barcha mos lifetimelar uchun" degan universal talab qo'yadi.

## Syntax and Examples

```rust
pub trait DeserializeOwned: for<'de> serde::Deserialize<'de> {}
```

## Common Mistakes

- HRTBni beginner lifetime annotationlari bilan bir darajada o'rganishga urinish.
- `for<'de>`ni value lifetime'ini uzaytiradi deb o'ylash.

## Related Concepts

- [[lifetimes]]
- [[trait-bounds|trait bounds]]
- [[deserializeowned|DeserializeOwned]]
- [[generics]]

## Sources

- [[wiki/sources/rust-for-backend-developers-serialization]]

