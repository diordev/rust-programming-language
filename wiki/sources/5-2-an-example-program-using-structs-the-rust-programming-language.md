---
title: "5.2. An Example Program Using Structs - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, structs, debug]
source_count: 1
source_path: "raw/books/the_rust_programming_language/5.2. An Example Program Using Structs - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch05-02-example-structs.html"
---

# 5.2. An Example Program Using Structs - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/5.2. An Example Program Using Structs - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch05-02-example-structs.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[rectangle-example|rectangle area example]] orqali [[structs]] qachon kerak bo'lishini amaliy refactoring bilan ko'rsatadi. Program rectangle width va height qiymatlaridan area hisoblaydi.

Birinchi version separate variables ishlatadi: `width1` va `height1`. `area(width1, height1)` ishlaydi, lekin function signature `fn area(width: u32, height: u32) -> u32` ikkala parameter bitta rectangle'ga tegishli ekanini type yoki name levelda kuchli ifodalamaydi. Shu sababli related data alohida yurib qoladi.

Ikkinchi version [[tuple]] ishlatadi: `let rect1 = (30, 50);` va `fn area(dimensions: (u32, u32)) -> u32`. Bu bitta argumentga o'tkazadi, lekin meaning indexga bog'lanadi: `dimensions.0` width, `dimensions.1` height. Area hisobida width/height almashsa natija bir xil, lekin drawing kabi boshqa domain behaviorlarda order muhim bo'ladi. Tuple data relationni qisman ifodalaydi, lekin field names bermaydi.

Uchinchi version `Rectangle` [[structs|struct]] ishlatadi:

```rust
struct Rectangle {
    width: u32,
    height: u32,
}
```

`area` endi `fn area(rectangle: &Rectangle) -> u32` bo'ladi. Bu signature aniqroq: function rectangle area hisoblaydi. Parameter immutable borrow bo'lgani uchun `main` `rect1` ownershipini saqlaydi. Borrowed struct fieldlarini o'qish field valuesni move qilmaydi; shuning uchun `rectangle.width * rectangle.height` safe.

Debugging qismida `println!("rect1 is {rect1}")` ishlamaydi, chunki custom `Rectangle` type [[display-formatting|Display formatting]] uchun `std::fmt::Display` implement qilmagan. Compiler [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]] chiqaradi. Primitive types uchun `Display` obvious bo'lishi mumkin, lekin struct uchun user-facing output qanday bo'lishi kerakligini Rust taxmin qilmaydi.

Developer debugging uchun [[debug-formatting|Debug formatting]] ishlatiladi: `println!("rect1 is {rect1:?}")`. Lekin custom struct `Debug` ham avtomatik implement qilmaydi va yana [[e0277-trait-bound-not-satisfied|E0277]] chiqishi mumkin. Buni `#[derive(Debug)]` bilan opt in qilish kerak. `{:?}` compact debug output beradi, `{:#?}` pretty debug output beradi.

[[dbg-macro|dbg! macro]] expressionni debug qilish uchun qulay. U expression ownershipini oladi, file va line number bilan value'ni chiqaradi, keyin ownershipni qaytaradi. Shu sababli `width: dbg!(30 * scale)` expression natijasini fieldga beradi. Lekin `dbg!(rect1)` rectangleni move qilishi mumkin; ownershipni saqlash uchun `dbg!(&rect1)` ishlatiladi. `dbg!` outputni [[stderr|stderr]]ga, [[println-macro|println!]] esa [[stdout|stdout]]ga yozadi.

Section oxirida `area` hali free function bo'lgani aytiladi. U faqat `Rectangle` bilan ishlaydi, shuning uchun behaviorni `Rectangle` type'iga yaqinroq bog'lash yaxshiroq. Bu keyingi sectionda [[methods]]ga olib boradi.

## Key Concepts

- [[rectangle-example|rectangle area example]]
- [[structs]]
- [[tuple]]
- [[borrowing]]
- [[debug-trait|Debug trait]]
- [[derive-attribute|derive attribute]]
- [[display-formatting|Display formatting]]
- [[debug-formatting|Debug formatting]]
- [[dbg-macro|dbg! macro]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]
- [[stdout]]
- [[stderr]]
- [[methods]]

## Code Examples

Separate variables:

```rust
fn area(width: u32, height: u32) -> u32 {
    width * height
}
```

Tuple refactor:

```rust
fn area(dimensions: (u32, u32)) -> u32 {
    dimensions.0 * dimensions.1
}
```

Struct refactor:

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```

Derived Debug:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

println!("rect1 is {rect1:?}");
println!("rect1 is {rect1:#?}");
```

`dbg!` macro:

```rust
let scale = 2;
let rect1 = Rectangle {
    width: dbg!(30 * scale),
    height: 50,
};

dbg!(&rect1);
```

## Exercises or Practice Ideas

- Rectangle area programini separate variables, tuple, va struct versionsda yozib readabilityni solishtirish.
- `area(rectangle: &Rectangle)` o'rniga `area(rectangle: Rectangle)` qilib ownership effectini kuzatish.
- `println!("{rect1}")` bilan `E0277`ni ko'rish, keyin `#[derive(Debug)]` va `{:?}` bilan tuzatish.
- `{:#?}` pretty outputni katta struct bilan sinab ko'rish.
- `dbg!(rect1)` va `dbg!(&rect1)` farqini ownership nuqtayi nazaridan tekshirish.

## Questions Raised

- Tuple qachon yetarli, struct qachon yaxshiroq?
- `Display` va `Debug` formatting mental modeli qanday farq qiladi?
- `dbg!` ownershipni olib qaytarsa ham, qachon reference berish kerak?
- `area` free function bo'lib qolsa, type behavior organizationi qanday zaiflashadi?

## Links To Update

- [[5-using-structs-to-structure-related-data|5. Using Structs to Structure Related Data]]
- [[rectangle-example|rectangle area example]]
- [[debug-trait|Debug trait]]
- [[derive-attribute|derive attribute]]
- [[dbg-macro|dbg! macro]]
- [[methods]]
