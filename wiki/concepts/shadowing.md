---
title: "Shadowing"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, variables]
source_count: 2
---

# Shadowing

## Short Definition

Shadowing bir xil variable nomini yangi `let` binding bilan qayta ishlatish. Yangi binding oldingisini scope ichida "yopadi".

## Why It Matters

Type conversion paytida nomni saqlab qolish qulay. Guessing gameda `guess` avval `String`, keyin `u32` bo'ladi.

Shadowing `mut`dan farq qiladi: `let` bilan yangi binding yaratiladi, shu sababli type o'zgarishi mumkin.

## Mental Model

```rust
let guess = String::from("42");
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

Bu `mut` emas: yangi binding yaratiladi.

Scope tugaganda inner shadowing ham tugaydi.

## Syntax and Examples

```rust
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

```rust
let spaces = "   ";
let spaces = spaces.len();
```

## Common Mistakes

- Shadowingni mutation bilan adashtirish.
- Bir xil nomni haddan tashqari ko'p ishlatib, code o'qilishini qiyinlashtirish.
- `mut` bilan variable typeini o'zgartirishga urinish; bu [[e0308-mismatched-types|E0308 mismatched types]]ga olib keladi.

## Related Concepts

- [[variables-and-mutability|variables and mutability]]
- [[result|Result]]
- [[type-inference|type inference]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Sources

- [[wiki/sources/2-programming-a-guessing-game]]
- [[3-1-variables-and-mutability]]
