---
title: "Generic Structs"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, generics, structs]
source_count: 1
---

# Generic Structs

## Short Definition

Generic struct field typelaridan biri yoki bir nechtasini generic type parameter orqali ifodalaydigan struct.

## Why It Matters

Generic structs bir xil data shape'ni turli concrete typelar bilan qayta ishlatadi. Masalan `Point<i32>`, `Point<f64>`, yoki `Point<i32, f64>` uchun alohida struct definition yozish shart emas.

## Mental Model

`struct Point<T> { x: T, y: T }` bitta generic parameter ishlatadi. Shu sabab bitta `Point<T>` instance ichida `x` va `y` bir xil concrete type bo'lishi kerak.

`struct Point<T, U> { x: T, y: U }` esa `x` va `y` turli concrete type bo'lishiga ruxsat beradi. Generic parameterlar soni design signalidir: ular juda ko'payib ketsa, type'ni kichikroq qismlarga ajratish kerak bo'lishi mumkin.

## Syntax and Examples

Bir xil field type:

```rust
struct Point<T> {
    x: T,
    y: T,
}

let integer = Point { x: 5, y: 10 };
let float = Point { x: 1.0, y: 4.0 };
```

Turli field typelar:

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

let integer_and_float = Point { x: 5, y: 4.0 };
```

## Common Mistakes

- `Point<T>` ichida `x` va `y` turli type bo'lishi mumkin deb o'ylash.
- `Point { x: 5, y: 4.0 }` uchun bitta `T` yetarli deb o'ylash; bu [[e0308-mismatched-types|E0308]] beradi.
- Har bir fieldga alohida generic parameter qo'shaverish; readability va designni tekshirish kerak.
- Generic struct concrete instancelari bir-biriga implicit convert bo'ladi deb o'ylash.

## Related Concepts

- [[structs]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[generic-methods|generic methods]]
- [[type-inference|type inference]]
- [[e0308-mismatched-types|E0308 mismatched types]]
- [[generic-point|generic Point]]

## Sources

- [[10-1-generic-data-types]]
