---
title: "E0004 non-exhaustive patterns"
type: error
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, compiler-error, match, pattern-matching]
source_count: 1
---

# E0004 non-exhaustive patterns

## Symptom

`match` expression barcha mumkin bo'lgan casesni qamrab olmaganda compiler `error[E0004]: non-exhaustive patterns` beradi. Chapter 6.2 misolida `Option<i32>` uchun `None` case handle qilinmagan:

```text
error[E0004]: non-exhaustive patterns: `None` not covered
```

## Cause

Rust [[match]] uchun [[exhaustive-matching|exhaustive matching]] talab qiladi. `Option<i32>` ikki variantga ega: `Some(i32)` va `None`. Faqat `Some(i)` arm yozilsa, `None` kelganda nima bo'lishi noma'lum bo'lib qoladi.

## Fix Pattern

Unutilgan variantni explicit arm bilan handle qiling:

```rust
match x {
    None => None,
    Some(i) => Some(i + 1),
}
```

Agar qolgan cases bir xil handle qilinsa, oxirgi armda [[catch-all-patterns|catch-all pattern]] ishlatish mumkin:

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => reroll(),
}
```

## Minimal Example

Failing:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        Some(i) => Some(i + 1),
    }
}
```

Fixed:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

## Related Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[option|Option]]
- [[enums]]
- [[6-2-the-match-control-flow-construct]]

