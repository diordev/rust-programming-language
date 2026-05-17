---
title: "Array Destructuring"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, arrays, patterns]
source_count: 1
---

# Array Destructuring

## Short Definition

Fixed-length array value'ni pattern orqali elementlarga ajratib olish.

## Why It Matters

`[T; N]` length'i compile time'da ma'lum bo'lgani uchun array patternlar tuplega o'xshash aniqlik beradi. Header/tail parsing yoki fixed-format input uchun qulay.

## Mental Model

Array pattern shape'ni ham tekshiradi. `[a, b, c]` faqat aynan 3 elementli arrayga mos keladi. `..` qolgan qismini tashlaydi, `rest @ ..` esa qolgan qismini bindingga bog'laydi.

## Syntax and Examples

```rust
let [a1, a2, a3] = [1, 2, 3];
```

```rust
let [first, _, third, rest @ ..] = [1, 2, 3, 4, 5];
println!("{first} {third} {rest:?}");
```

## Common Mistakes

- Array patternni general slice/vector pattern bilan aralashtirish.
- `..` bilan `rest @ ..` farqini e'tiborsiz qoldirish.
- Fixed-length guarantee yo'qligida shu modelni ishlatishga urinish.

## Related Concepts

- [[array]]
- [[pattern-destructuring|pattern destructuring]]
- [[slice-patterns|slice patterns]]
- [[slices]]

## Sources

- [[wiki/sources/rust-for-backend-developers-destructuring]]
