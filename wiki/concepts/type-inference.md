---
title: "Type Inference"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, types]
source_count: 2
---

# Type Inference

## Short Definition

`type inference` compiler contextdan kelib chiqib type'ni o'zi aniqlashi.

## Why It Matters

Rust ko'p joyda type yozishni kamaytiradi, lekin ambiguity bo'lsa [[type-annotations|type annotations]] talab qiladi.

## Mental Model

Compiler expressionlar, assignments, function signatures, va method callsdan type constraints yig'adi.

## Syntax and Examples

```rust
let x = 42; // i32 default bo'lishi mumkin
let y: u32 = "42".parse().expect("number");
```

Vector initial valuesdan type infer qilinadi:

```rust
let v = vec![1, 2, 3]; // Vec<i32>
```

Empty vector uchun annotation kerak bo'lishi mumkin:

```rust
let v: Vec<i32> = Vec::new();
```

## Common Mistakes

- `"42".parse()` kabi ambiguous expression type'ni har doim topadi deb o'ylash.
- Inference code clarityni pasaytirayotgan joyda explicit annotation yozmaslik.
- Empty collectionda element type'i contextdan kelmasa annotation kerakligini unutish.

## Related Concepts

- [[data-types|data types]]
- [[type-annotations|type annotations]]
- [[e0284-type-annotations-needed|E0284 type annotations needed]]
- [[vector|Vec<T>]]
- [[vec-macro|vec! macro]]

## Sources

- [[3-2-data-types-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
