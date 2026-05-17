---
title: "Generic Point"
type: example
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, example, generics, structs, methods]
source_count: 1
---

# Generic Point

## Summary

`Point<T>` bitta generic parameter bilan ikkala coordinate bir xil type bo'lishini talab qiladi. `Point<T, U>` esa `x` va `y` uchun turli typelarni ruxsat qiladi. Methods `impl<T> Point<T>`, concrete `impl Point<f32>`, yoki method-level generics bilan yozilishi mumkin.

## Code

Same-type point:

```rust
struct Point<T> {
    x: T,
    y: T,
}

let integer = Point { x: 5, y: 10 };
let float = Point { x: 1.0, y: 4.0 };
```

Different-type point:

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

let integer_and_float = Point { x: 5, y: 4.0 };
```

Generic and concrete methods:

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
```

Mixing generic parameters:

```rust
struct Point<X1, Y1> {
    x: X1,
    y: Y1,
}

impl<X1, Y1> Point<X1, Y1> {
    fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}
```

## Explanation

- `Point<T>`: `x` va `y` bitta concrete typega resolve bo'ladi.
- `Point<T, U>`: `x` type'i `T`, `y` type'i `U`.
- `impl<T> Point<T>` methodlari barcha concrete `Point<T>` instances uchun mavjud.
- `impl Point<f32>` methodlari faqat `Point<f32>` uchun mavjud.
- `mixup` `self`dan `x`ni, `other`dan `y`ni olib, yangi `Point<X1, Y2>` yaratadi.

## Related Concepts

- [[generic-structs|generic structs]]
- [[generic-methods|generic methods]]
- [[generic-type-parameters|generic type parameters]]
- [[structs]]
- [[methods]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Sources

- [[10-1-generic-data-types]]
