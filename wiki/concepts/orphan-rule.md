---
title: "Orphan Rule"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-16
tags: [rust, traits, coherence]
source_count: 3
---

# Orphan Rule

## Short Definition

Orphan rule trait implementation faqat trait yoki type'dan kamida bittasi current crate'ga local bo'lsa yozilishi mumkin degan Rust coherence qoidasi. "Current crate" degani faqat bitta file emas; shu crate root va uning modulelari ichida e'lon qilingan type/traitlar.

## Why It Matters

Orphan rule crates orasidagi implementation conflictsni oldini oladi. Aks holda ikki external crate bir xil external traitni bir xil external type uchun implement qilib, compiler qaysi implementationni ishlatishni bilmay qolishi mumkin edi.

## Mental Model

Allowed:

- Local type uchun external trait implement qilish: `impl Display for SocialPost`.
- External type uchun local trait implement qilish: `impl Summary for Vec<T>`.

Not allowed:

- External traitni external type uchun implement qilish: `impl Display for Vec<T>`.

Bu qoida "parent" siz qolgan implni rad qiladi: na trait, na type sizning crate'ingizga tegishli bo'lsa, implementation orphan hisoblanadi.

## Syntax and Examples

Allowed because `SocialPost` is local:

```rust
impl Display for SocialPost {
    // ...
}
```

Allowed because `Summary` is local:

```rust
impl<T> Summary for Vec<T> {
    // ...
}
```

Not allowed because both are external:

```rust
impl<T> Display for Vec<T> {
    // ...
}
```

## Newtype Pattern — Orphan Rule'dan O'tish

Tashqi trait + tashqi tip kombinatsiyasi uchun yagona yo'l — [[newtype-pattern]]. Tashqi tipni mahalliy tuple struct ichiga o'rab oling:

```rust
use std::fmt;

struct Wrapper(Vec<String>);   // <-- mahalliy tip

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}
```

`Wrapper` mahalliy bo'lgani uchun orphan rule rioya etiladi. Compile time'da `Wrapper` qatlami yo'qoladi. Batafsil: [[wrapper-newtype-display]].

## Common Mistakes

- External traitni external type uchun implement qilib bo'ladi deb o'ylash.
- Orphan rule restrictionini privacy bilan chalkashtirish.
- Trait local bo'lsa, external type uchun implementation yozish mumkinligini unutish.
- Type local bo'lsa, external trait uchun implementation yozish mumkinligini unutish.

## Related Concepts

- [[traits]]
- [[trait-implementations|trait implementations]]
- [[crate]]
- [[public-api|public API]]
- [[display-formatting|Display]]
- [[vector|Vec<T>]]
- [[newtype-pattern|newtype pattern]] — orphan rule'dan o'tish vositasi
- [[tuple-structs|tuple structs]]

## Sources

- [[10-2-defining-shared-behavior-with-traits]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
- [[wiki/sources/rust-for-backend-developers-traits]]
