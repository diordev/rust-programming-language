---
title: "Default Generic Parameters"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, generics]
source_count: 1
---

# Default Generic Parameters

## Short Definition

Generic parameter deklaratsiyasida `<T = ConcreteType>` sintaksisi bilan **default tip** belgilash. Foydalanuvchi tip bermasa — default ishlatiladi.

## Why It Matters

Aksariyat ishlatuvchilar bitta umumiy tipni xohlaydi. Default kompilyatorga ishonib boilerplateni qisqartiradi. Shuningdek, mavjud trait'ga yangi parameter qo'shganda — backward compatibility uchun zarur.

## Mental Model

```
trait Add<Rhs = Self>  →  Rhs ko'rsatilmasa, Rhs = Self
                       →  Add for Point  ⇔  Add<Point> for Point
```

Default — caller'ning soddalashtirilgan ergonomikasi.

## Syntax and Examples

### `Add` traitining haqiqiy ta'rifi

```rust
trait Add<Rhs = Self> {
    type Output;
    fn add(self, rhs: Rhs) -> Self::Output;
}
```

`Rhs` — "right-hand side." Default `Self` ekan, demak `T + T` (bir xil tipdagi qo'shish) — default holat.

### Default ishlatish — `Point + Point`

```rust
use std::ops::Add;

#[derive(Debug, Copy, Clone, PartialEq)]
struct Point { x: i32, y: i32 }

impl Add for Point {           // Add<Self> ham yozish mumkin
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

### Default'ni o'zgartirish — `Millimeters + Meters`

```rust
use std::ops::Add;

struct Millimeters(u32);
struct Meters(u32);

impl Add<Meters> for Millimeters {     // Rhs = Meters (Self emas)
    type Output = Millimeters;

    fn add(self, other: Meters) -> Millimeters {
        Millimeters(self.0 + (other.0 * 1000))
    }
}
```

Endi `Millimeters(50) + Meters(2)` ishlaydi va `Millimeters(2050)` qaytaradi.

## Default Generic Parameters Ikki Asosiy Foydalanish

1. **Mavjud kodni buzmasdan tip kengaytirish.**
   Trait'ga yangi generic parameter qo'shish kerak bo'lsa, default qiymat berish bilan eski impl'lar buzilmaydi.

2. **Maxsus holatlarda customizatsiya.**
   Aksariyat foydalanuvchi default'ni xohlaydi (`Point + Point`), lekin kerak bo'lsa boshqa tip bilan customize qilish mumkin (`Millimeters + Meters`).

`Add` trait'i ikkinchi maqsad uchun klassik misol.

## Common Mistakes

- **Default qiymatdan keyin generic'ni belgilamaslik.** `Add<Rhs=Self>` da `Self` qoidalarga rioya qilishi shart (`Self: Sized`).
- **Generic parameter bilan associated type'ni adash qilish.** Default generic — implementor tomonidan o'zgartirilishi mumkin (`Add<Meters>`); associated type — implementorda bitta mahkam tip.
- **Operator overload uchun default'ni o'zgartirib, klassik foydalanuvchini chalkashtirib qo'yish.** `Point + Point` o'rniga `Point + (i32, i32)` qilsangiz — kutilmagan API.

## Related Concepts

- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[traits]]
- [[associated-types|associated types]]
- [[operator-overloading|operator overloading]] — default'ning eng keng ko'rinadigan ishlatilishi
- [[newtype-pattern|newtype pattern]] — `Millimeters(u32)` misoli newtype

## Sources

- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
