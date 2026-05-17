---
title: "10.1. Generic Data Types - The Rust Programming Language"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, rust-book, source, generics]
source_count: 1
source_path: "raw/books/the_rust_programming_language/10.1. Generic Data Types.md"
source_url: "https://doc.rust-lang.org/stable/book/ch10-01-syntax.html"
---

# 10.1. Generic Data Types - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/10.1. Generic Data Types.md`
- Type: book chapter section
- URL: https://doc.rust-lang.org/stable/book/ch10-01-syntax.html
- Date processed: 2026-05-07

## Detailed Summary

Chapter 10.1 [[generics]]ni real Rust definitions ichida ishlatishni boshlaydi: [[generic-functions|generic functions]], [[generic-structs|generic structs]], [[generic-enums|generic enums]], va [[generic-methods|generic methods]]. Maqsad bir xil logic yoki data shape'ni har xil concrete type bilan qayta yozmasdan ifodalash.

Function definitionsda generic parameterlar parameter va return type yoziladigan joyda ishlatiladi. `largest_i32(list: &[i32]) -> &i32` va `largest_char(list: &[char]) -> &char` faqat type signature bo'yicha farq qiladi; body bir xil. Bu duplication `fn largest<T>(list: &[T]) -> &T` shaklida umumlashtiriladi. `T` function nomidan keyin, parameter listdan oldin `<>` ichida e'lon qilinadi. Shunda `list` `T` elementlar slice'i, return value esa shu `T` elementga reference bo'ladi.

Ammo generic signature yozishning o'zi body har qanday `T` uchun ishlaydi degani emas. `largest<T>` ichida `item > largest` ishlatilgani uchun compiler [[e0369-binary-operation-cannot-be-applied|E0369]] beradi: `>` operation `&T` uchun har doim mavjud emas. Compiler `T`ni [[partial-ord|PartialOrd]] trait bilan restrict qilishni taklif qiladi. Bu keyingi traits sectioniga ko'prik: generic type ustida qaysi behavior kerak bo'lsa, u trait bound orqali aytiladi.

Struct definitionsda generic parameter struct nomidan keyin yoziladi. `struct Point<T> { x: T, y: T }` ikkala field ham bir xil concrete type bo'lishini bildiradi. `Point { x: 5, y: 10 }` va `Point { x: 1.0, y: 4.0 }` alohida instancelarda ishlaydi, lekin `Point { x: 5, y: 4.0 }` [[e0308-mismatched-types|E0308]] beradi, chunki bitta instance ichida `T` avval integer deb infer qilingan.

Turli field typelar kerak bo'lsa, bir nechta generic parameter ishlatiladi: `struct Point<T, U> { x: T, y: U }`. Bu `Point { x: 5, y: 4.0 }` kabi instance'larni ruxsat qiladi. Source ogohlantiradi: juda ko'p generic parameter code readabilityni pasaytiradi va ba'zan type/designni kichikroq qismlarga ajratish kerakligini bildiradi.

Enum definitionsda generics variants payload type'larini abstract qiladi. Standard library `Option<T>` bitta generic parameter ishlatadi: `Some(T)` value borligini, `None` value yo'qligini bildiradi. `Result<T, E>` ikkita generic parameter ishlatadi: `Ok(T)` success value, `Err(E)` error value. Bu Chapter 6 va Chapter 9dagi `Option`/`Result` endi syntax darajasida tushunarli bo'lishini ta'minlaydi.

Method definitionsda generic type ishlatish uchun `impl<T> Point<T>` kabi `impl`dan keyin ham type parameter e'lon qilinadi. `impl<T> Point<T> { fn x(&self) -> &T { &self.x } }` har qanday `Point<T>` instance uchun method beradi. Agar method faqat bitta concrete instantiation uchun kerak bo'lsa, `impl Point<f32>` kabi yoziladi; masalan `distance_from_origin` faqat `Point<f32>` uchun mavjud.

Methodning o'z generic parameterlari struct generic parameterlaridan farq qilishi mumkin. `Point<X1, Y1>` uchun `fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2>` methodi `self`dan `x`ni, boshqa pointdan `y`ni olib yangi type kombinatsiya yaratadi. `X1` va `Y1` `impl` bilan bog'langan, `X2` va `Y2` esa faqat method signature uchun relevant.

Performance section generics runtime cost keltirmasligini tushuntiradi. Rust compile time'da [[monomorphization]] qiladi: generic code ishlatilgan concrete typelarga qarab specialized codega aylantiriladi. Masalan `Option<T>` `Some(5)` va `Some(5.0)` bilan ishlatilsa, compiler `Option<i32>` va `Option<f64>`ga o'xshash specialized definitions yaratadi. Natijada generic abstraction runtime'da hand-written duplicated concrete code kabi ishlaydi.

## Key Concepts

- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[generic-functions|generic functions]]
- [[generic-structs|generic structs]]
- [[generic-enums|generic enums]]
- [[generic-methods|generic methods]]
- [[partial-ord|PartialOrd]]
- [[monomorphization]]
- [[zero-cost-abstractions|zero-cost abstractions]]
- [[traits]]
- [[structs]]
- [[enums]]
- [[methods]]
- [[option|Option<T>]]
- [[result|Result<T, E>]]
- [[e0369-binary-operation-cannot-be-applied|E0369]]
- [[e0308-mismatched-types|E0308]]

## Code Examples

Generic `largest` signature:

```rust
fn largest<T>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

This does not compile until `T` is restricted to comparable values:

```rust
fn largest<T: std::cmp::PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

Generic structs:

```rust
struct Point<T> {
    x: T,
    y: T,
}

struct MixedPoint<T, U> {
    x: T,
    y: U,
}
```

Generic method and concrete-only method:

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

Method-specific generic parameters:

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

## Exercises or Practice Ideas

- `largest_i32` va `largest_char` orasidagi duplicationni belgilang, keyin `largest<T>` signature'ini yozing.
- `largest<T>` nega `PartialOrd` talab qilishini `item > largest` line'i orqali tushuntiring.
- `Point<T>` va `Point<T, U>` orasidagi farqni uchta instance bilan tekshiring.
- `impl<T> Point<T>` va `impl Point<f32>` method availability farqini yozing.
- `mixup` methodida qaysi generic parameter qayerdan kelayotganini jadvalga ajrating.
- `Option<i32>` va `Option<f64>` monomorphization qilinganda mental modelda qanday specialized definitions paydo bo'lishini yozing.

## Questions Raised

- Trait bounds syntax'i qaysi holatlarda inline `T: Trait`, qaysi holatlarda `where` bilan yozilishi yaxshi?
- `PartialOrd` va `Ord` orasidagi farq nima?
- Monomorphization compile time va binary size'ga qanday ta'sir qilishi mumkin?
- Generic parameterlar ko'payib ketsa, qaysi refactor signallari bor?

## Links To Update

- [[wiki/chapters/10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[generic-functions|generic functions]]
- [[generic-structs|generic structs]]
- [[generic-enums|generic enums]]
- [[generic-methods|generic methods]]
- [[partial-ord|PartialOrd]]
- [[monomorphization]]
- [[generic-largest-function|generic largest function]]
- [[generic-point|generic Point]]
- [[e0369-binary-operation-cannot-be-applied|E0369 binary operation cannot be applied]]
- [[e0308-mismatched-types|E0308 mismatched types]]
