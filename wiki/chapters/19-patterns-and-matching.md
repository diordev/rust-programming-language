---
title: "Chapter 19: Patterns and Matching"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, pattern-matching]
source_count: 1
---

# Chapter 19: Patterns and Matching

## Learning Goal

Rust'da pattern matching'ning barcha imkoniyatlarini tushunish: qayerlarda ishlatiladi, refutable/irrefutable farqi nima, va qanday sintaksis turlarini mavjud.

## Main Ideas

Pattern — Rust'da type strukturasiga mos keluvchi maxsus sintaksis. Dastur qiymatni pattern bilan solishtiradi; mos kelsa pattern ichidagi nomlar bog'lanadi, mos kelmasa o'sha kod ishlamaydi.

Pattern tarkibiy qismlari:

| Komponent | Misol | Izoh |
|---|---|---|
| Literal | `5`, `'a'` | Aniq qiymat |
| Destructured | `(a, b)`, `Some(x)` | Murakkab typeni "yechish" |
| Variable | `x`, `name` | Istalgan qiymatni bog'lash |
| Wildcard | `_` | Ignore qilish |
| Placeholder | `..` | Qolganlarni o'tkazib yuborish |

Bob 6'dagi `match` misollari allaqachon patternlardan foydalangan. Bu bob esa ularning to'liq manzarasini beradi.

## Concepts

- [[pattern-matching|pattern matching]]
- [[match]]
- [[if-let|if let]]
- [[while-let|while let]]
- [[pattern-destructuring|pattern destructuring]]
- [[irrefutable-pattern|irrefutable pattern]]
- [[refutable-pattern|refutable pattern]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]

## Examples

```rust
let x = 5;              // x — irrefutable pattern
let (a, 3) = ...;       // literal + variable pattern
Some(Color::Red)        // enum variant destructuring
```

## Review Questions

1. Pattern nima va qanday komponentlardan iborat?
2. `match`da patternlar qanday ishlaydi?
3. Refutable va irrefutable pattern nima?

## Related Pages

- [[wiki/sources/19-patterns-and-matching]]
- [[wiki/chapters/19-1-all-the-places-patterns-can-be-used]]
- [[wiki/chapters/19-2-refutability-whether-a-pattern-might-fail-to-match]]
- [[wiki/chapters/6-enums-and-pattern-matching]]
