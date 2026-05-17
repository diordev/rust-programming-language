---
title: "Operator Overloading"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-13
tags: [rust, traits, operators, std-ops]
source_count: 2
---

# Operator Overloading

## Short Definition

`+`, `-`, `*`, `==`, `<`, va boshqa standart operatorlar uchun custom xatti-harakat — `std::ops` ichidagi maxsus trait'larni implement qilish orqali.

## Why It Matters

Custom tiplar uchun matematik ifodalarni tabiiy yozish: `vector_a + vector_b`, `matrix * vec`, `path / "subdir"`. Rust faqat **mavjud operatorlarni** overload qilishga ruxsat beradi — yangi operator yaratish mumkin emas.

## Mental Model

Har operator — bitta trait. Trait'ni implement qilsang, operator ishlaydi:

```
+   →  std::ops::Add
-   →  std::ops::Sub
*   →  std::ops::Mul
/   →  std::ops::Div
==  →  PartialEq
<   →  PartialOrd
[]  →  Index / IndexMut
```

To'liq ro'yxat: [`std::ops`](https://doc.rust-lang.org/std/ops/) moduli.

Appendix B bu mapping'ni jadval shaklida ko'rsatadi va yana muhim chegarani eslatadi: ko'p syntax belgilari operator bo'lsa ham, hammasi overload bo'lavermaydi.

## Syntax and Examples

### `+` ni overload qilish — `Add`

```rust
use std::ops::Add;

#[derive(Debug, Copy, Clone, PartialEq)]
struct Point { x: i32, y: i32 }

impl Add for Point {
    type Output = Point;

    fn add(self, other: Point) -> Point {
        Point { x: self.x + other.x, y: self.y + other.y }
    }
}

fn main() {
    assert_eq!(
        Point { x: 1, y: 0 } + Point { x: 2, y: 3 },
        Point { x: 3, y: 3 }
    );
}
```

### Turli xil tiplar bilan — `Add<Rhs>`

`Add` trait'i [[default-generic-parameters|default generic parameter]] (`Rhs = Self`) bilan ta'riflangan. Rhs ni o'zgartirib, turli tiplar orasida qo'shish:

```rust
use std::ops::Add;

struct Millimeters(u32);
struct Meters(u32);

impl Add<Meters> for Millimeters {
    type Output = Millimeters;

    fn add(self, other: Meters) -> Millimeters {
        Millimeters(self.0 + (other.0 * 1000))
    }
}
```

`Millimeters(50) + Meters(2)` → `Millimeters(2050)`.

### Boshqa operatorlar

```rust
use std::ops::{Add, Sub, Mul, Neg};

#[derive(Copy, Clone, Debug, PartialEq)]
struct Vec2 { x: f64, y: f64 }

impl Add for Vec2 {
    type Output = Self;
    fn add(self, o: Self) -> Self { Vec2 { x: self.x + o.x, y: self.y + o.y } }
}

impl Sub for Vec2 {
    type Output = Self;
    fn sub(self, o: Self) -> Self { Vec2 { x: self.x - o.x, y: self.y - o.y } }
}

impl Mul<f64> for Vec2 {
    type Output = Self;
    fn mul(self, k: f64) -> Self { Vec2 { x: self.x * k, y: self.y * k } }
}

impl Neg for Vec2 {
    type Output = Self;
    fn neg(self) -> Self { Vec2 { x: -self.x, y: -self.y } }
}
```

### Representative operator -> trait jadvali

- `+` -> `Add`
- `-` -> `Sub`, unary `-` -> `Neg`
- `*` -> `Mul`, unary `*` -> [[deref-trait|Deref]]
- `/` -> `Div`
- `%` -> `Rem`
- `==`, `!=` -> [[partial-eq|PartialEq]]
- `<`, `>`, `<=`, `>=` -> [[partial-ord|PartialOrd]]
- `[]` indexing -> `Index`, `IndexMut`

Overload bo'lmaydigan mashhur syntax'lar ham bor: borrow `&expr`, logical `&&` va `||`, error propagation `?`, field access `.`, va macro `!` invocation formi.

## Common Mistakes

- **Yangi operator yaratishga urinish.** Rust faqat mavjudlarini ruxsat beradi — `<>>` kabi narsani aniqlab bo'lmaydi.
- **Operator semantikasini buzish.** `Add` matematik qo'shish ma'nosini saqlashi kerak — `commutative`, `associative` (mumkin bo'lganda). String concat'ni `+` orqali ham OK, lekin `Vec` push'ni `+` qilish chalkashtiradi.
- **`impl Add for &Point` yozmaslik.** Default `Add` `self` (move) oladi. Reference uchun: `impl Add for &Point` yoki `&'a Point` lifetime bilan.
- **`PartialEq` ni `Eq` bilan adashtirish.** `Eq` totallikni va'da qiladi (NaN'siz). Float'lar `Eq` bo'la olmaydi.

## Related Concepts

- [[traits]]
- [[default-generic-parameters|default generic parameters]] — `Rhs = Self`
- [[associated-types|associated types]] — `type Output;`
- [[derive-attribute|derive attribute]] — `#[derive(PartialEq, ...)]` ko'p operatorlarni avtomatik beradi
- [[partial-ord|PartialOrd]] — `<`, `>`, `<=`, `>=`
- [[partial-eq|PartialEq]] — `==`, `!=`
- [[newtype-pattern|newtype pattern]] — operator faqat sizning typingiz uchun
- [[rust-operators-and-symbols]]

## Sources

- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
- [[wiki/sources/22-2-b-operators-and-symbols|22.2]]
