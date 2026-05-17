---
title: "Ordering"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, enum, comparisons]
source_count: 2
---

# Ordering

## Short Definition

`std::cmp::Ordering` comparison natijasini bildiradigan enum: `Less`, `Equal`, yoki `Greater`.

## Why It Matters

`cmp()` total ordering natijasini, `partial_cmp()` esa `Option<Ordering>` ko'rinishida partial ordering natijasini beradi. Sorting va branching logic shu enumga tayanadi.

## Mental Model

Comparison Rust'da "manfiy/nol/musbat son" emas, named semantic variantlar bilan ifodalanadi.

## Syntax and Examples

```rust
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

```rust
println!("{:?}", 4.0.partial_cmp(&5.0)); // Some(Less)
```

## Common Mistakes

- `partial_cmp` natijasi har doim `Some(Ordering)` bo'ladi deb o'ylash.
- `cmp` bilan `partial_cmp` semanticsini aralashtirish.

## Related Concepts

- [[ord-trait|Ord]]
- [[partial-ord]]
- [[match]]
- [[pattern-matching|pattern matching]]

## Sources

- [[wiki/sources/2-programming-a-guessing-game]]
- [[wiki/sources/rust-for-backend-developers-common-traits]]
