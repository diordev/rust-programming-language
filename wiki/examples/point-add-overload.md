---
title: "Point + Add operator overloading"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, operator-overloading]
source_count: 1
---

# Point + Add Operator Overloading

## Listing 20-15 — `Add` traitini implement qilish

```rust
use std::ops::Add;

#[derive(Debug, Copy, Clone, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}

impl Add for Point {
    type Output = Point;

    fn add(self, other: Point) -> Point {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

fn main() {
    assert_eq!(
        Point { x: 1, y: 0 } + Point { x: 2, y: 3 },
        Point { x: 3, y: 3 }
    );
}
```

## Tahlil

- `impl Add for Point` — bu `impl Add<Point> for Point` ga teng, chunki `Add` `<Rhs = Self>` [[default-generic-parameters|default]] bilan ta'riflangan.
- `type Output = Point;` — [[associated-types|associated type]] qaytish tipini belgilaydi.
- `fn add(self, other: Point) -> Point` — `self` move bo'ladi (yangi `Point` qaytariladi).
- `#[derive(Copy, Clone, PartialEq)]` — Point Copy bo'lgani uchun `+` ifoda bir nechta marta xato bermay ishlaydi.

## `Add` Traitining Haqiqiy Ta'rifi

```rust
trait Add<Rhs = Self> {
    type Output;
    fn add(self, rhs: Rhs) -> Self::Output;
}
```

`Rhs = Self` default — agar `impl Add for Point` desangiz, `Rhs = Point`. Boshqa tip uchun: `impl Add<Meters> for Millimeters`.

## Variantlar

### Reference uchun overload — Copy bo'lmagan tiplar

```rust
use std::ops::Add;

#[derive(Debug, PartialEq)]
struct BigPoint { x: i64, y: i64, label: String }

impl Add for &BigPoint {
    type Output = BigPoint;

    fn add(self, other: &BigPoint) -> BigPoint {
        BigPoint {
            x: self.x + other.x,
            y: self.y + other.y,
            label: format!("{}+{}", self.label, other.label),
        }
    }
}
```

`&Self` uchun implement qilish — clone'siz qo'shish.

## Related

- [[operator-overloading|operator overloading]]
- [[default-generic-parameters|default generic parameters]]
- [[associated-types|associated types]]
- [[copy-trait|Copy trait]]
- [[traits]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
