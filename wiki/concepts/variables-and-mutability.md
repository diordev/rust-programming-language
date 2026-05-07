---
title: "Variables and Mutability"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, variables]
source_count: 2
---

# Variables and Mutability

## Short Definition

Rustda variables default immutable. Qiymat o'zgarishi kerak bo'lsa, variable `mut` bilan e'lon qilinadi.

## Why It Matters

Default immutability code reasoningni osonlashtiradi. O'zgarishi kerak bo'lgan data alohida belgilanadi.

Rust compile time'da immutable bindingga qayta assignment qilishni to'xtatadi. Bu `E0384 cannot assign twice to immutable variable` xatosi bilan ko'rinadi.

## Mental Model

```rust
let apples = 5; // immutable
let mut bananas = 5; // mutable
```

`mut` value o'zgarishini ruxsat qiladi va code readerga intentni ko'rsatadi.

Guessing gameda input string `read_line` tomonidan o'zgartirilishi kerak, shuning uchun:

```rust
let mut guess = String::new();
```

## Syntax and Examples

```rust
let mut guess = String::new();
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

```rust
let mut x = 5;
x = 6;
```

## Common Mistakes

- O'zgaradigan variable uchun `mut`ni unutish.
- `mut`ni hamma joyda odat bo'yicha qo'yish. Rustda faqat haqiqatan o'zgaradigan binding mutable bo'lishi kerak.
- [[shadowing]] kerak bo'lgan joyda `mut` bilan type o'zgartirishga urinish.

## Related Concepts

- [[shadowing]]
- [[constants]]
- [[e0384-cannot-assign-twice|E0384 cannot assign twice]]
- [[mutable-reference|mutable reference]]
- [[reference]]

## Sources

- [[2-programming-a-guessing-game-the-rust-programming-language]]
- [[3-1-variables-and-mutability-the-rust-programming-language]]
