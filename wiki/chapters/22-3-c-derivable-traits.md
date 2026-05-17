---
title: "22.3. C - Derivable Traits"
type: chapter
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, traits, derive, reference]
source_count: 1
---

# 22.3. C - Derivable Traits

## Learning Goal

`#[derive(...)]` orqali standard library'dan qaysi traitlar mechanical tarzda implement qilinishini, ularning semantik shartlari va practical use case'larini tushunish.

## Main Ideas

- `derive` default behavior aniq bo'lgan traitlar uchun avtomatik impl generatsiya qiladi.
- Standard library'dagi derivable traitlar chekli to'plam: `Debug`, `PartialEq`, `Eq`, `PartialOrd`, `Ord`, `Clone`, `Copy`, `Hash`, `Default`.
- `Display` kabi user-facing formatting traitlari derive qilinmaydi, chunki meaningful default output domen knowledge talab qiladi.
- Derive "free magic" emas: fieldlar ham o'sha traitni implement qilishi kerak bo'lgan shartlar bor.

## Trait Families

- Debugging: [[debug-trait|Debug]]
- Equality: [[partial-eq|PartialEq]], [[eq-trait|Eq]]
- Ordering: [[partial-ord]], [[ord-trait|Ord]]
- Duplication: [[clone]], [[copy-trait|Copy]]
- Hashing: [[hash-trait|Hash]]
- Defaults: [[default-trait|Default]]

## Practical Rules

- `assert_eq!` odatda [[partial-eq|PartialEq]] va [[debug-trait|Debug]] talab qiladi.
- `Eq` uchun type self-equalityni buzmasligi kerak; `NaN` sabab floatlar misol bo'la olmaydi.
- `Ord` total orderingni va'da qiladi, shu sabab `PartialOrd`dan kuchliroq.
- `Copy` derive bo'lishi uchun type `Clone` ham bo'lishi kerak.
- `Default` derive struct update syntax va `unwrap_or_default` bilan ko'p ishlatiladi.

## Concepts

- [[derive-attribute]]
- [[debug-trait]]
- [[partial-eq]]
- [[eq-trait]]
- [[partial-ord]]
- [[ord-trait]]
- [[clone]]
- [[copy-trait]]
- [[hash-trait]]
- [[default-trait]]

## Examples

- [[derive-debug-partialeq-assert-eq|derive Debug + PartialEq for assert_eq!]]

## Exercises

1. `Display` nega derive qilinmasligini yozing.
2. `Eq` va `PartialEq`ni `NaN` misoli bilan solishtiring.
3. `Ord` va `PartialOrd` uchun collection misollarini yozing.
4. `Copy` derive bo'ladigan va bo'lmaydigan ikki type misolini yozing.
5. `Default` bilan `..Default::default()` ishlatadigan struct misoli yarating.

## Review Questions

1. Qaysi standard traitlar derive qilinadi?
2. `Debug` nimaga kerak?
3. `Eq`ning `PartialEq`dan farqi nima?
4. `Copy` nega method bermaydi?
5. `Hash` va `Ord` qaysi collection use case'lariga ulanadi?
6. `Default` qaysi patternlarda ko'p ishlatiladi?

## Related Pages

- [[wiki/sources/22-3-c-derivable-traits|Source: 22.3]]
- [[wiki/chapters/22-appendix|Chapter 22]]
- [[derive-attribute]]
- [[default-trait]]
