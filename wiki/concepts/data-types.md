---
title: "Data Types"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, types]
source_count: 1
---

# Data Types

## Short Definition

Har bir Rust value ma'lum data typega ega. Type compilerga value bilan qanday ishlashni bildiradi.

## Why It Matters

Rust statically typed: compile time'da barcha variable typelari ma'lum bo'lishi kerak. Bu compilerga type mistakesni runtime oldidan topishga yordam beradi.

## Mental Model

Rust type inference qiladi, lekin noaniqlik bo'lsa annotation kerak:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

Primitive types ikki guruh:

- [[scalar-types|scalar types]]: single value.
- [[compound-types|compound types]]: multiple values grouped together.

## Syntax and Examples

```rust
let x = 2.0; // f64
let y: f32 = 3.0;
let t = true;
let c = 'z';
```

## Common Mistakes

- `parse()` natijasiga type annotation bermaslik.
- `usize` va numeric domain typelarini aralashtirish.
- `char`ni "human character" bilan bir xil deb o'ylash; Rust `char` Unicode scalar value.

## Related Concepts

- [[type-inference|type inference]]
- [[type-annotations|type annotations]]
- [[scalar-types|scalar types]]
- [[compound-types|compound types]]
- [[e0284-type-annotations-needed|E0284 type annotations needed]]

## Sources

- [[3-2-data-types-the-rust-programming-language]]
