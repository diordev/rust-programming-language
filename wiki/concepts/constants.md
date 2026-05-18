---
title: "Constants"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-18
tags: [rust, variables]
source_count: 3
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

Backend beginner source yana ikkita amaliy qoidani aniq qiladi: type annotation har doim majburiy va naming convention odatda `UPPER_SNAKE_CASE`.

Global-data source shu boundary'ni yana aniqroq qiladi: `const` global access berishi mumkin, lekin runtime'da quriladigan heap-owning value yoki mutable shared state uchun vosita emas.

## Syntax and Examples

```rust
const MAX_POINTS: u32 = 100_000;
```

```rust
const ANONYMOUS_NAME: &str = "anonymous";
```

## Common Mistakes

- Constantga `mut` qo'shishga urinish.
- Type annotationni unutish.
- Runtime computed valueni constant sifatida e'lon qilishga urinish.
- `HashMap::new()` kabi non-const initializer'ni `const`ga tiqishga urinish.

## Related Concepts

- [[variables-and-mutability|variables and mutability]]
- [[immutability]]
- [[type-annotations|type annotations]]
- [[static-items]]
- [[global-data]]

## Sources

- [[3-1-variables-and-mutability]]
- [[wiki/sources/rust-for-backend-developers-variables]]
- [[wiki/sources/rust-for-backend-developers-global-data]]
