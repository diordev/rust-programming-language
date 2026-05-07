---
title: "8.1. Storing Lists of Values with Vectors - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, collections, vectors]
source_count: 1
source_path: "raw/books/the_rust_programming_language/8.1 Storing Lists of Values with Vectors - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch08-01-vectors.html"
---

# 8.1. Storing Lists of Values with Vectors - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/8.1 Storing Lists of Values with Vectors - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch08-01-vectors.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section birinchi common collection sifatida [[vector|Vec<T>]]ni tushuntiradi. Vector bitta data structure ichida bir nechta value saqlaydi va elementlarni memoryda yonma-yon joylashtiradi. Vector faqat bir xil typedagi elementlarni saqlaydi; shuning uchun `Vec<i32>` ichiga `i32`lar, `Vec<String>` ichiga `String`lar kiradi. Bu [[generics]] bilan bog'liq: `Vec<T>`dagi `T` vector element type'ini bildiradi.

Empty vector yaratish uchun `Vec::new()` ishlatiladi. Agar vector bo'sh bo'lsa va keyingi context type'ni ko'rsatmasa, Rust `T`ni infer qila olmaydi, shuning uchun type annotation kerak bo'ladi:

```rust
let v: Vec<i32> = Vec::new();
```

Initial values bilan vector yaratishda [[vec-macro|vec! macro]] ko'proq ishlatiladi. `let v = vec![1, 2, 3];` code'ida integer literalsdan Rust `Vec<i32>`ni infer qiladi. Bu [[type-inference|type inference]]ning collection contextidagi oddiy misoli.

Vector update qilish uchun binding mutable bo'lishi kerak. `let mut v = Vec::new();` dan keyin `v.push(5);` kabi `push` methodlari vector oxiriga element qo'shadi. Elementlardan compiler type'ni infer qila oladi, shuning uchun bo'sh vector declarationida ham keyingi `push`lar type information berishi mumkin.

Vector elementini o'qishning ikki asosiy yo'li bor: indexing va `get`. `&v[2]` uchinchi elementga reference beradi, chunki vector indexlari 0dan boshlanadi. `v.get(2)` esa `Option<&T>` qaytaradi. Bu farq out-of-bounds accessda muhim: `&v[100]` [[panic|panic]] qiladi, `v.get(100)` esa `None` qaytaradi. Agar invalid index normal user input yoki recoverable holat bo'lishi mumkin bo'lsa, `get` bilan `match` qilib `Some`/`None`ni handle qilish user-friendlyroq.

Vector references [[borrowing]] rulesga bo'ysunadi. Listing 8-6da `let first = &v[0];` immutable reference olgandan keyin `v.push(6);` qilish va keyin `first`ni ishlatish [[e0502-mutable-and-immutable-borrow-conflict|E0502]] beradi. Sabab faqat generic "mutable and immutable borrow overlap" emas; vector implementation detaili ham bor: `push` capacity yetmasa yangi heap allocation qilishi, eski elementlarni ko'chirishi, va eski memoryni deallocate qilishi mumkin. Shunda oldingi elementga reference dangling bo'lib qolardi. Borrow checker buni compile time'da to'xtatadi.

Vector bo'ylab index bilan yurish o'rniga [[for-loop|for loop]] bilan iteration qilish odatda xavfsiz va idiomaticroq. Immutable iteration:

```rust
for i in &v {
    println!("{i}");
}
```

Mutable iteration esa `&mut v` bilan elementlarga mutable references beradi. Element qiymatini o'zgartirish uchun [[dereference-operator|dereference operator]] `*` kerak bo'ladi:

```rust
for i in &mut v {
    *i += 50;
}
```

For loop vectorga reference ushlab turgani uchun loop body ichida vectorga element insert/remove qilish ham borrow checker tomonidan rad qilinadi. Bu rule iteration paytida element references valid qolishini saqlaydi.

Vector faqat bitta concrete type saqlagani uchun "har xil type" kabi ko'rinadigan data kerak bo'lsa, [[enums]] pattern ishlatiladi. Spreadsheet row example'da `SpreadsheetCell` enum `Int(i32)`, `Float(f64)`, va `Text(String)` variantsga ega. `Vec<SpreadsheetCell>` bitta type saqlaydi, lekin variant payloadlari orqali row ichida turli data shakllarini model qiladi. Keyin [[match]] exhaustive handlingni enforce qiladi.

Bu enum technique faqat possible types seti compile time'da ma'lum bo'lsa ishlaydi. Agar runtime'da oldindan bilib bo'lmaydigan heterogeneous values kerak bo'lsa, keyin Chapter 18da ko'riladigan trait objects kerak bo'lishi mumkin.

Vector scope'dan chiqqanda vectorning o'zi ham, ichidagi elementlar ham dropped bo'ladi. Bu [[drop]] va [[ownership]] rules bilan mos: vector elementlarning owner'i bo'lsa, vector drop bo'lganda elementlar cleanup qilinadi. Borrow checker vector elementlariga references vectorning o'zidan uzoqroq yashab qolmasligini ta'minlaydi.

## Key Concepts

- [[vector|Vec<T>]]
- [[collections]]
- [[vec-macro|vec! macro]]
- [[generics]]
- [[type-inference|type inference]]
- [[type-annotations|type annotations]]
- [[vector-indexing|vector indexing]]
- [[option|Option]]
- [[panic]]
- [[borrowing]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
- [[for-loop|for loop]]
- [[mutable-reference|mutable reference]]
- [[dereference-operator|dereference operator]]
- [[enums]]
- [[match]]
- [[drop]]

## Code Examples

Empty vector:

```rust
fn main() {
    let v: Vec<i32> = Vec::new();
}
```

`vec!` macro:

```rust
fn main() {
    let v = vec![1, 2, 3];
}
```

Updating with `push`:

```rust
fn main() {
    let mut v = Vec::new();

    v.push(5);
    v.push(6);
    v.push(7);
    v.push(8);
}
```

Indexing vs `get`:

```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5];

    let third: &i32 = &v[2];
    println!("The third element is {third}");

    let third: Option<&i32> = v.get(2);
    match third {
        Some(third) => println!("The third element is {third}"),
        None => println!("There is no third element."),
    }
}
```

Borrow conflict after element reference:

```rust
fn main() {
    let mut v = vec![1, 2, 3, 4, 5];

    let first = &v[0];

    v.push(6);

    println!("The first element is: {first}");
}
```

Mutable iteration:

```rust
fn main() {
    let mut v = vec![100, 32, 57];
    for i in &mut v {
        *i += 50;
    }
}
```

Enum values in one vector:

```rust
fn main() {
    enum SpreadsheetCell {
        Int(i32),
        Float(f64),
        Text(String),
    }

    let row = vec![
        SpreadsheetCell::Int(3),
        SpreadsheetCell::Text(String::from("blue")),
        SpreadsheetCell::Float(10.12),
    ];
}
```

Vector drop:

```rust
fn main() {
    {
        let v = vec![1, 2, 3, 4];
    } // v and its elements are dropped here
}
```

## Exercises or Practice Ideas

- Empty `Vec::new()` bilan type annotation kerak bo'ladigan va kerak bo'lmaydigan ikki example yozing.
- `&v[100]` va `v.get(100)` behaviorini alohida tushuntiring.
- `let first = &v[0]; v.push(6); println!("{first}")` nima uchun E0502 berishini vector reallocation mental modeli bilan izohlang.
- `for i in &mut v { *i += 50; }` code'ida `*` nima uchun kerakligini yozing.
- Spreadsheet row uchun `enum SpreadsheetCell`ga `Bool(bool)` variant qo'shib, `match` bilan valuesni print qiling.
- Vector scope'dan chiqqanda elementlar qanday dropped bo'lishini ownership diagrammasida chizing.

## Questions Raised

- Qachon indexing panic qilishini xohlash mumkin, qachon `get` + `Option` yaxshiroq?
- Vector capacity/reallocation borrowing qoidalarini amalda qanday tushuntiradi?
- Vector ichida heterogeneous data kerak bo'lsa enum yetarlimi yoki trait object kerakmi?
- Iteration paytida vectorni o'zgartirish nima uchun xavfli bo'lishi mumkin?
- `Vec<T>` va slice (`&[T]`) API designda qanday farq qiladi?

## Links To Update

- [[8-common-collections|8. Common Collections]]
- [[vector|Vec<T>]]
- [[collections]]
- [[vec-macro|vec! macro]]
- [[vector-indexing|vector indexing]]
- [[panic]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
- [[dereference-operator|dereference operator]]
- [[enums]]
- [[drop]]
