---
title: "Type Annotations"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, types]
source_count: 2
---

# Type Annotations

## Short Definition

`type annotations` binding, parameter, yoki return value uchun type'ni explicit yozishdir.

## Why It Matters

Annotations ambiguous code uchun compilerga kerakli information beradi va public API ni aniq qiladi.

## Mental Model

Annotation compilerga "mana shu joyda type shunday bo'lishi kerak" degan constraint qo'shadi.

## Syntax and Examples

```rust
let guess: u32 = "42".parse().expect("number");
fn add_one(x: i32) -> i32 { x + 1 }
```

Empty vector uchun element type'i annotation bilan berilishi mumkin:

```rust
let v: Vec<i32> = Vec::new();
```

## Common Mistakes

- Constant uchun type annotation majburiy ekanini unutish.
- Function parametersda type yozishni qoldirib ketish.
- Empty collectionda compiler element type'ni bilmasligini unutish.

## Related Concepts

- [[type-inference|type inference]]
- [[constants]]
- [[vector|Vec<T>]]
- [[e0284-type-annotations-needed|E0284 type annotations needed]]

## Sources

- [[3-2-data-types-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
