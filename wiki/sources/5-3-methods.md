---
title: "5.3. Methods - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, methods, structs]
source_count: 1
source_path: "raw/books/the_rust_programming_language/5.3. Methods.md"
source_url: "https://doc.rust-lang.org/stable/book/ch05-03-method-syntax.html"
---

# 5.3. Methods - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/5.3. Methods.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch05-03-method-syntax.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[methods]]ni tushuntiradi. Methods functionsga o'xshaydi: `fn`, name, parameters, return value, va body bor. Farq: method [[impl-block|impl block]] ichida type contextida define qilinadi va birinchi parameter doim [[self-parameter|self]] bo'ladi. `self` method chaqirilayotgan struct instance'ni bildiradi.

Rectangle exampleda free function `fn area(rectangle: &Rectangle) -> u32` methodga aylantiriladi:

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

`impl Rectangle` ichidagi hamma itemlar `Rectangle` type bilan associated. Method call [[method-syntax|method syntax]] bilan yoziladi: `rect1.area()`. Bu code `area(&rect1)`ga nisbatan type behaviorni yaxshiroq organize qiladi.

`&self` aslida `self: &Self` shorthand. `Self` `impl` qilinayotgan type aliasi, bu yerda `Rectangle`. Method birinchi parameter sifatida `self`, `&self`, yoki `&mut self` olishi mumkin:

- `&self`: instance'ni immutable borrow qiladi, read-only behavior uchun;
- `&mut self`: instance'ni mutable borrow qiladi, mutation uchun;
- `self`: instance ownershipini oladi, odatda transform consuming methods uchun.

`area` `&self` ishlatadi, chunki u rectangle data'sini faqat o'qiydi va caller ownershipni saqlashi kerak.

Methods organization foydasi beradi: type bilan bajariladigan behaviorlar bitta `impl` block ichida turadi, userlar `Rectangle` capabilitiesni library bo'ylab qidirmaydi. Method nomi field nomi bilan bir xil bo'lishi mumkin. `rect1.width()` parentheses bilan method call; `rect1.width` parentheses'siz field access. Getter pattern public APIda fieldni private qilib, read-only access berish uchun keyin module/privacy mavzusida muhim bo'ladi.

Rustda C/C++dagi `->` operator yo'q. Method calllarda [[automatic-referencing-and-dereferencing|automatic referencing and dereferencing]] ishlaydi. `object.something()` chaqirilganda Rust method signaturega moslash uchun kerakli `&`, `&mut`, yoki `*`ni qo'shadi. Bu ownershipni method syntax bilan ergonomic qiladi, chunki receiver type aniq.

Methods qo'shimcha parameters olishi mumkin. `can_hold(&self, other: &Rectangle) -> bool` self rectangle boshqa rectangle'ni sig'dira oladimi, tekshiradi. `other` immutable borrow bo'ladi, chunki method faqat o'qiydi va caller `rect2`/`rect3` ownershipini saqlashi kerak.

[[associated-functions]] `impl` block ichidagi hamma functions uchun umumiy nom. Agar function birinchi parameter sifatida `self` olmasa, u method emas. Bunday functions ko'pincha constructor-like helpers sifatida ishlatiladi. Example: `Rectangle::square(3)` bitta size'dan square rectangle yaratadi. `Self` return type va bodyda `Rectangle` aliasi sifatida ishlatiladi. Associated function `::` syntax bilan chaqiriladi.

Rust bir type uchun [[multiple-impl-blocks|multiple impl blocks]]ga ruxsat beradi. Bu sectionda `area` va `can_hold`ni alohida `impl Rectangle` blocklarda yozish equivalent ekani ko'rsatiladi. Hozir alohida qilish shart emas, lekin keyingi [[generics]] va [[traits]] mavzularida foydali bo'ladi.

Chapter summary: structs meaningful domain types yaratadi; `impl` blocks type bilan associated behaviorni belgilaydi; methods instance behaviorni ifodalaydi. Keyingi logical tool [[enum]] bo'ladi.

## Key Concepts

- [[methods]]
- [[impl-block|impl block]]
- [[method-syntax|method syntax]]
- [[self-parameter|self parameter]]
- [[self-type|Self type]]
- [[automatic-referencing-and-dereferencing|automatic referencing and dereferencing]]
- [[associated-functions]]
- [[multiple-impl-blocks|multiple impl blocks]]
- [[constructor-functions|constructor functions]]
- [[getters]]
- [[borrowing]]
- [[structs]]

## Code Examples

`area` method:

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

let area = rect1.area();
```

Method with another parameter:

```rust
impl Rectangle {
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```

Associated function:

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

let sq = Rectangle::square(3);
```

Multiple impl blocks:

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

impl Rectangle {
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```

## Exercises or Practice Ideas

- `area(&Rectangle)` free functionni `area(&self)` methodga refactor qilish.
- `can_hold(&self, other: &Rectangle)` yozib, `true` va `false` casesni tekshirish.
- `&self`, `&mut self`, va `self` receiver forms uchun kichik examples yozish.
- `Rectangle::square(size)` associated function yaratish.
- Bitta `impl` blockni ikkita `impl` blockga ajratib, behavior o'zgarmasligini tekshirish.

## Questions Raised

- Method qachon free functiondan yaxshiroq organization beradi?
- `self`, `&self`, va `&mut self` tanlovi ownership va borrowingni qanday bildiradi?
- Automatic referencing/dereferencing qaysi method calllarni ergonomic qiladi?
- Associated function constructor emas, lekin constructor-like bo'lishi mumkin degan farq nimada?

## Links To Update

- [[wiki/chapters/5-using-structs-to-structure-related-data|5. Using Structs to Structure Related Data]]
- [[methods]]
- [[impl-block|impl block]]
- [[associated-functions]]
- [[borrowing]]
- [[generics]]
- [[traits]]
