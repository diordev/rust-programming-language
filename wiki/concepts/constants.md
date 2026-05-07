---
title: "Constants"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, variables]
source_count: 1
---

# Constants

## Short Definition

Constants `const` bilan e'lon qilinadigan, har doim immutable, type annotation talab qiladigan named values.

## Why It Matters

Domain value yoki magic numberni nomlash code o'qilishini yaxshilaydi va bitta joydan update qilish imkonini beradi.

## Mental Model

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Constants compile-time constant expressionga bog'lanadi va declared scope ichida program davomida valid bo'ladi.

## Syntax and Examples

```rust
const MAX_POINTS: u32 = 100_000;
```

## Common Mistakes

- Constantga `mut` qo'shishga urinish.
- Type annotationni unutish.
- Runtime computed valueni constant sifatida e'lon qilishga urinish.

## Related Concepts

- [[variables-and-mutability|variables and mutability]]
- [[immutability]]
- [[type-annotations|type annotations]]

## Sources

- [[3-1-variables-and-mutability-the-rust-programming-language]]
