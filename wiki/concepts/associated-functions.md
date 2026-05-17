---
title: "Associated Functions"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, structs, functions]
source_count: 3
---

# Associated Functions

## Short Definition

Associated functions type bilan bog'langan functions bo'lib, instance orqali emas, type namespace orqali chaqirilishi mumkin.

## Why It Matters

Associated functions type bilan bog'langan behaviorni type namespace ichida tashkil qiladi. `self` olmaydigan associated functions constructor-like helpers uchun qulay.

## Mental Model

Associated function type'ga tegishli function. Agar birinchi parameter `self` bo'lmasa, u method emas; ko'pincha constructor-like helpers uchun ishlatiladi. Backend struct source `new` patternini aynan shu rolda ko'rsatadi.

## Syntax and Examples

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

let sq = Rectangle::square(3);
```

## Common Mistakes

- Associated function va methodni bir xil deb o'ylash.
- Type namespace orqali chaqirilishini unutish.
- `new` nomi Rustda maxsus majburiy constructor deb o'ylash.

## Related Concepts

- [[methods]]
- [[impl-block|impl block]]
- [[constructor-functions|constructor functions]]
- [[self-type|Self type]]
- [[structs]]
- [[functions]]

## Sources

- [[wiki/sources/5-using-structs-to-structure-related-data]]
- [[5-3-methods]]
- [[wiki/sources/rust-for-backend-developers-structs]]
