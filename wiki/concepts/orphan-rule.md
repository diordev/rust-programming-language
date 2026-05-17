---
title: "Orphan Rule"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, traits, coherence]
source_count: 4
---

# Orphan Rule

## Short Definition

Trait implementation faqat trait yoki type'dan kamida bittasi current crate'ga local bo'lsa yozilishi mumkin degan coherence qoidasi.

## Why It Matters

Bu qoida crates orasida bir xil `impl` kolliziyasini oldini oladi.

## Mental Model

Allowed:

- local type uchun external trait
- external type uchun local trait

Not allowed:

- external trait uchun external type

Advance newtype source bu qoidani `std::fs::File` uchun `Ord` implement qilish misolida amaliy ko'rsatadi. To'g'ri workaround `[[newtype-pattern|newtype pattern]]`.

## Syntax and Examples

```rust
impl std::fmt::Display for SocialPost {
    // local type => allowed
}
```

```rust
struct FileWrapper(std::fs::File);

impl Ord for FileWrapper {
    fn cmp(&self, other: &Self) -> std::cmp::Ordering {
        std::cmp::Ordering::Equal
    }
}
```

## Common Mistakes

- Qoidani privacy bilan aralashtirish.
- Newtype workaround'ni faqat formatting case'lari uchun deb o'ylash.

## Related Concepts

- [[traits]]
- [[trait-implementations|trait implementations]]
- [[crate]]
- [[newtype-pattern|newtype pattern]]
- [[ord-trait|Ord]]

## Sources

- [[10-2-defining-shared-behavior-with-traits]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
- [[wiki/sources/rust-for-backend-developers-traits]]
- [[wiki/sources/rust-for-backend-developers-newtype-pattern]]

