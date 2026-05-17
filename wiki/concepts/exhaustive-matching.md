---
title: "Exhaustive Matching"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, match, pattern-matching]
source_count: 1
---

# Exhaustive Matching

## Short Definition

Exhaustive matching — `match` arms barcha ehtimoliy input values yoki enum variantsni qamrab olishi kerak degan Rust talabi.

## Why It Matters

Rust unutilgan branchlarni runtime bugga aylantirmasdan compile time'da ushlaydi. `Option<T>` uchun bu ayniqsa muhim: `Some(T)` bilan birga `None` ham handle qilinishi kerak, aks holda value bor deb noto'g'ri taxmin qilish mumkin.

## Mental Model

`match` compilerga "mana shu typening hamma shakllari uchun nima qilishni bilaman" degan contract beradi. Har bir variant explicit arm bilan yopilishi mumkin yoki oxirgi [[catch-all-patterns|catch-all pattern]] qolgan hammasini qamrab oladi.

## Syntax and Examples

To'g'ri:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

Xato:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        Some(i) => Some(i + 1),
    }
}
```

Bu `None` case yopilmagani uchun [[e0004-non-exhaustive-patterns|E0004]] beradi.

## Common Mistakes

- `Option<T>`da `None`ni unutish.
- Yangi enum variant qo'shilgandan keyin eski `match`larni yangilamaslik.
- `_` catch-all patternni juda erta ishlatib, keyingi muhim variants compile-time eslatmasini yo'qotish.

## Related Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[option|Option]]
- [[enums]]
- [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]]

## Sources

- [[6-2-the-match-control-flow-construct]]

