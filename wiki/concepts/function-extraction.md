---
title: "Function Extraction"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, functions, refactoring]
source_count: 1
---

# Function Extraction

## Short Definition

Function extraction - duplicated yoki conceptually distinct code'ni alohida function body'ga ko'chirish refactoringi.

## Why It Matters

Function extraction code duplicationni kamaytiradi, intentni nomlaydi, va inputs/outputsni function signature orqali aniq qiladi.

## Mental Model

Extraction steps:

- Duplicate code'ni identify qiling.
- Inputs va return valuesni function signature'da ko'rsating.
- Duplicate code sitesni function call bilan almashtiring.

Bu genericsga tayyorlaydi: value darajasida parameter ishlatganimiz kabi, type darajasida generic parameter ishlatiladi.

## Syntax and Examples

```rust
fn largest(list: &[i32]) -> &i32 {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

## Common Mistakes

- Function inputlarini ownership bilan olish, lekin borrow yetarli bo'lgan joyda.
- Return value reference ekanida u qaysi inputga bog'langanini tushunmaslik.
- Function nomi conceptni emas, implementation detailni ifodalashi.

## Related Concepts

- [[functions]]
- [[code-duplication|code duplication]]
- [[slices]]
- [[borrowing]]
- [[lifetimes]]
- [[generics]]

## Sources

- [[wiki/sources/10-generic-types-traits-and-lifetimes]]
