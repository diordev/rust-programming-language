---
title: "Methods"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, structs, methods, enums]
source_count: 5
---

# Methods

## Short Definition

Method type instance'i bilan bog'langan function bo'lib, odatda `self`, `&self`, yoki `&mut self` oladi. [[structs|Struct]] uchun ham, [[enums|enum]] uchun ham `impl` block orqali method qo'shish mumkin.

## Why It Matters

Methods behaviorni [[structs]] yoki [[enums]] data modeliga yaqin joyda ifodalashga yordam beradi. Free functionsni type capabilities sifatida organize qilish uchun `impl` block ichiga ko'chirish mumkin.

## Mental Model

Struct yoki enum data shape beradi; method shu data bilan bajariladigan behaviorni beradi. Birinchi parameter `self` bo'lgani uchun method qaysi instance ustida ishlayotganini biladi. Backend beginner source `&self`, `&mut self`, va `self` receiverlari orasidagi ownership farqini juda toza ko'rsatadi: faqat o'qish, mutation, yoki ownershipni olib ketish.

Generic types uchun method yozilganda `impl<T> Point<T>` kabi `impl` headerida generic parameter e'lon qilinadi. Concrete-only method kerak bo'lsa, `impl Point<f32>` kabi yoziladi va method faqat shu concrete instantiation uchun mavjud bo'ladi.

## Syntax and Examples

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

let area = rect1.area();
```

Method qo'shimcha parameters ham olishi mumkin:

```rust
impl Rectangle {
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```

Enum bilan:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

impl Message {
    fn call(&self) {
        // odatda `match self` bilan variantga qarab branch tanlanadi
    }
}

let m = Message::Write(String::from("hello"));
m.call();
```

Generic struct uchun method:

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

Faqat concrete instantiation uchun method:

```rust
impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
```

## Common Mistakes

- Methodni free functiondan farqlamaslik.
- `self` borrowing/mutation qoidalari bilan bog'lanishini unutish.
- `&self` ownershipni oladi deb o'ylash; u immutable borrow.
- Field access va method callni chalkashtirish.
- Generic type parameter ishlatilgan methodlarda `impl<T>` declarationni unutish.
- `impl Point<f32>`dagi method hamma `Point<T>` uchun mavjud deb o'ylash.

## Related Concepts

- [[associated-functions]]
- [[impl-block|impl block]]
- [[method-syntax|method syntax]]
- [[self-parameter|self parameter]]
- [[structs]]
- [[enums]]
- [[borrowing]]
- [[automatic-referencing-and-dereferencing|automatic referencing and dereferencing]]
- [[generics]]
- [[generic-methods|generic methods]]
- [[generic-structs|generic structs]]
- [[generic-type-parameters|generic type parameters]]

## Sources

- [[wiki/sources/5-using-structs-to-structure-related-data]]
- [[5-3-methods]]
- [[6-1-defining-an-enum]]
- [[10-1-generic-data-types]]
- [[wiki/sources/rust-for-backend-developers-structs]]
