---
title: "Перечисления - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, enums]
source_count: 1
---

# Перечисления - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/33. Перечисления.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/enums.html

## Detailed Summary

Bu source `enum`ni ikki qatlamda o'qitadi: C-like variant ro'yxati va payload tashiy oladigan ADT. Muhim signal shuki, Rust enum'ni "bir nechta nomlangan constant" deb tushunib qolish xato. `IpAddrKind::V4` va `IpAddrKind::V6` soddaroq ko'rinish, lekin source tez orada `IpAddr::V4(u8, u8, u8, u8)` va `IpAddr::V6(String)`ga o'tib, enum'ni "bitta type ostida turli shape" modeli sifatida ko'rsatadi.

`HttpStatus` misoli source'dagi birinchi amaliy nuance: C-like enum varianti bilan numeric discriminant bog'lash mumkin va `as usize` orqali ko'rish mumkin. Lekin wiki uchun caveat muhim: bu discriminant/value mental modeli foydali bo'lsa ham, enum memory layoutini ortiqcha aniq qilib yuborish noto'g'ri; real representation `repr` atributlari va compiler optimizatsiyalariga bog'liq bo'lishi mumkin.

Source `Shape` enum'i bilan tuple-like va struct-like payloadlar orasidagi farqni juda ochiq beradi. Bu yerda saqlanadigan durable nuqta: enum "qiymatlar ro'yxati" emas, "variants ro'yxati". Har variant alohida data shape tashishi mumkin, lekin hammasi baribir bitta outer type bo'lib qoladi. Shu model keyingi `Option` va `Result`ni ham tabiiy qiladi.

`match` enum bilan ishlashning asosiy instrumenti sifatida ko'rsatiladi. `calc_area(shape: &Shape)` misoli exhaustive handling nega foydali ekanini amaliy qiladi: `Square` va `Rectangle` ikkalasi ham ko'rilishi kerak. Source enum'ga methods qo'shish mumkinligini ham eslatadi, ya'ni `impl Shape { fn calc_area(&self) -> f32 { ... } }` orqali behavior ham enum type yoniga ko'chadi.

`if let` bo'limi source'da alohida markazga ega. Muhim signal: `if let` `match`ning o'rnini to'liq bosmaydi; u faqat bitta variant qiziq bo'lganda ixcham syntax beradi. `Shape::Square { width }` misoli branchning qaysi variantga qaratilganini aniq qiladi va qolgan casesni explicit exhaustivenesssiz tashlab ketadi.

Source oxiridagi memory layout bo'limi beginner uchun foydali, lekin ehtiyotkor yozilishi kerak. Enum value discriminant va payload mental modelida ishlaydi, katta payloadli variant butun enum size'iga ta'sir qilishi mumkin. Buni "xotirada har doim aynan shunday yotadi" deb emas, taxminiy operational model sifatida saqlash to'g'ri.

Yana bitta foydali nuance: source C-like enum'larni qayta sharhlaydi. `V4` yoki `V6` shunchaki "raqam" emas; semantik jihatdan singleton/unit-like variant. Elementga `= 200` kabi qiymat berilganda esa bu aslida discriminantni explicit belgilash bo'ladi.

## Key Concepts

- [[enums]]
- [[enum-variants|enum variants]]
- [[match]]
- [[if-let|if let]]
- [[methods]]
- [[generic-enums|generic enums]]

## Code Examples

```rust
enum HttpStatus {
    Ok = 200,
    NotModified = 304,
    NotFound = 404,
}
```

```rust
enum Shape {
    Square { width: f32 },
    Rectangle { width: f32, height: f32 },
}

impl Shape {
    fn calc_area(&self) -> f32 {
        match self {
            Shape::Square { width } => width * width,
            Shape::Rectangle { width, height } => width * height,
        }
    }
}
```

```rust
if let Shape::Square { width } = s {
    println!("This is square of width {width}");
}
```

## Exercises or Practice Ideas

- `Shape`ga `Circle { radius: f32 }` variantini qo'shib, qaysi `match` joylari compiler tomonidan majburiy yangilanishini ko'ring.
- C-like enum va payload enum orasidagi farqni ko'rsatadigan bitta mini-API yozing.
- `if let` va `match` bilan bir xil enum value'ni ikki xil yo'l bilan handle qilib, qaysi holatda qaysi syntax yaxshiroq ekanini solishtiring.
- Numeric discriminantli enum yozib, `as usize` qaysi signalni beradi, qaysi signalni bermasligini izohlang.

## Questions Raised

- Qachon C-like enum yetadi, qachon payloadli enum kerak bo'ladi?
- `if let` qachon readabilityni oshiradi, qachon `match` exhaustiveness muhimroq?
- Enum size va payload size orasidagi bog'liqlikni performance taxmini sifatida qayergacha ishlatish mumkin?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-enums]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[enums]]
- [[enum-variants|enum variants]]
- [[match]]
- [[if-let|if let]]
- [[methods]]
