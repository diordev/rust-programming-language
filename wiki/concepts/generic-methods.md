---
title: "Generic Methods"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, generics, methods]
source_count: 2
---

# Generic Methods

## Short Definition

Generic method generic type ustida yozilgan yoki o'z method-level generic parameterlariga ega bo'lgan method.

## Why It Matters

Generic methods behaviorni generic data modelga yaqin joyda saqlaydi. `Point<T>` kabi generic struct uchun methodlar har qanday `T`ga, faqat muayyan concrete instantiationga, yoki methodning o'z type parameterlariga bog'lanishi mumkin.

## Mental Model

`impl<T> Point<T>`dagi `T` struct type parameterini `impl` block ichida ishlatish uchun e'lon qilinadi. Bu blockdagi methodlar har qanday `Point<T>` uchun mavjud bo'ladi.

`impl Point<f32>` esa generic emas: u faqat concrete `Point<f32>` uchun method beradi. Method signature o'z generic parameterlarini ham e'lon qilishi mumkin, masalan `fn mixup<X2, Y2>(...)`.

Chapter 10.2 [[conditional-method-implementations|conditional method implementations]]ni ko'rsatadi: `impl<T: Display + PartialOrd> Pair<T>` methodni faqat `T` kerakli traitsni implement qilganda beradi.

## Syntax and Examples

Har qanday `Point<T>` uchun method:

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
```

Faqat `Point<f32>` uchun method:

```rust
impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
```

Method-level generic parameters:

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

Conditional method:

```rust
impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

## Common Mistakes

- `impl Point<T>` yozib, `impl<T>`da `T`ni e'lon qilmaslik.
- `impl Point<f32>` ichidagi method hamma `Point<T>` uchun mavjud deb o'ylash.
- Struct generic parameterlari va method-level generic parameterlarni bir xil scope deb chalkashtirish.
- Method `self`ni value bilan olsa, ownership move bo'lishini unutish.
- Conditional impl sabab method hamma `Pair<T>` instancelarda mavjud bo'lmasligini unutish.

## Related Concepts

- [[methods]]
- [[impl-block|impl block]]
- [[generic-structs|generic structs]]
- [[generic-type-parameters|generic type parameters]]
- [[trait-bounds|trait bounds]]
- [[conditional-method-implementations|conditional method implementations]]
- [[display-formatting|Display]]
- [[partial-ord|PartialOrd]]
- [[ownership]]
- [[self-parameter|self parameter]]
- [[generic-point|generic Point]]

## Sources

- [[10-1-generic-data-types-the-rust-programming-language]]
- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
