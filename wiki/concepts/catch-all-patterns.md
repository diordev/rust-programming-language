---
title: "Catch-All Patterns"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, match, pattern-matching]
source_count: 1
---

# Catch-All Patterns

## Short Definition

Catch-all pattern — `match`da oldingi patternsga mos kelmagan barcha qiymatlarni qamrab oladigan oxirgi arm.

## Why It Matters

[[exhaustive-matching|Exhaustive matching]] talabini bajarishda ba'zan barcha qolgan cases uchun bitta default behavior kerak bo'ladi. Catch-all pattern buni aniq ifodalaydi.

## Mental Model

Patterns yuqoridan pastga tekshiriladi. Catch-all "shu paytgacha mos kelmagan hammasi" degani, shuning uchun odatda oxirgi arm bo'ladi.

- Named binding (`other`) qolgan qiymatni ishlatish uchun kerak.
- `_` qolgan qiymat kerak emasligini bildiradi va unused variable warning bermaydi.
- `_ => ()` qolgan casesda explicit "hech narsa qilma" degani.

## Syntax and Examples

Qolgan qiymatni ishlatish:

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    other => move_player(other),
}
```

Qolgan qiymatni ignore qilish:

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => reroll(),
}
```

Qolgan qiymatlar uchun hech narsa qilmaslik:

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => (),
}
```

## Common Mistakes

- Catch-all armni yuqoriga qo'yish; keyingi arm'lar unreachable bo'ladi.
- `_` ishlatib, keyin default value kerak bo'lib qolganini sezmaslik; bunday paytda named binding kerak.
- `_` bilan yangi enum variants compiler tomonidan eslatilishini yo'qotish.

## Related Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[unit-type|unit type]]

## Sources

- [[6-2-the-match-control-flow-construct-the-rust-programming-language]]

