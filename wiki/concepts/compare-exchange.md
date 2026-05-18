---
title: "compare_exchange"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, concurrency, atomics, cas]
source_count: 1
---

# compare_exchange

## Short Definition

`Atomic*::compare_exchange` CAS operatsiyasi: "kutilgan eski qiymat hali ham turgan bo'lsa yangi qiymatni yoz".

## Why It Matters

`load` va `store` orasida boshqa thread yozib yuborishi mumkin bo'lgan read-modify-write scenariylar uchun kerak.

## Mental Model

Optimistic update: eski qiymatni o'qidingiz, yangi qiymatni hisobladingiz, keyin hali ham eski qiymat turgan bo'lsa commit qilasiz; bo'lmasa qayta urinib ko'rasiz.

## Syntax and Examples

```rust
let result = atomic.compare_exchange(
    expected,
    new,
    std::sync::atomic::Ordering::Relaxed,
    std::sync::atomic::Ordering::Relaxed,
);
```

## Common Mistakes

- Failure branch'da actual value'ni olib loop'ni yangilamaslik.
- `load + store` shu bilan bir xil deb o'ylash.

## Related Concepts

- [[atomic-types]]
- [[atomic-memory-ordering]]

## Sources

- [[wiki/sources/rust-for-backend-developers-multithreading]]
