---
title: "Ordering"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, enum]
source_count: 1
---

# Ordering

## Short Definition

`Ordering` comparison natijasini bildiradigan enum: `Less`, `Greater`, yoki `Equal`.

## Why It Matters

Guessing game `cmp` natijasini [[match]] bilan handle qilib, user guessini secret number bilan taqqoslaydi.

## Mental Model

Comparison bitta numeric result emas, named variants bilan ifodalanadi.

## Syntax and Examples

```rust
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

## Common Mistakes

- `Ordering` variantlarini import qilmasdan ishlatish.
- `cmp` references kutishini unutish.

## Related Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[result|Result]]

## Sources

- [[2-programming-a-guessing-game-the-rust-programming-language]]
