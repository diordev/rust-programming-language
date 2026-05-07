---
title: "Orphan Rule"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, traits, coherence]
source_count: 1
---

# Orphan Rule

## Short Definition

Orphan rule trait implementation faqat trait yoki type'dan kamida bittasi current crate'ga local bo'lsa yozilishi mumkin degan Rust coherence qoidasi.

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

## Sources

- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
