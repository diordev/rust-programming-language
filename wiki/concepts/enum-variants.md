---
title: "Enum Variants"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, enums]
source_count: 5
---

# Enum Variants

## Short Definition

Variant — enum ichidagi mumkin bo'lgan qiymat shakli. Variantlar enum identifier ostida namespacelangan va har biri o'z data shape'ga (datasiz, tuple-like, struct-like) ega bo'lishi mumkin.

## Why It Matters

Variantlar enumning asosiy ifoda kuchini ta'minlaydi: ular har xil shape va data miqdorida bo'lishi mumkin, shu bois bitta type ostida turli case'larni modellashtirish imkonini beradi. Bundan tashqari, har bir variant — *konstruktor funksiya* sifatida ham ishlatiladi, bu kod yozishni qisqartiradi.

## Mental Model

Variant — "tagged" qiymat shakli. Enum runtime'da ikki qismdan iborat: discriminant (qaysi variant ekanini bildiruvchi tag) va payload (variant ichidagi data). Nazariy darajada variant bo'sh tag, tuple, yoki named-fields struct bo'lishi mumkin — Rust uchalasini ham qo'llaydi.

Variant nomi avtomatik ravishda bir nechta narsa bo'ladi:

- Pattern (`match Message::Write(text) => ...`).
- Constructor function (`IpAddr::V4` — `String` qabul qilib `IpAddr` qaytaradi).
- Path (`Color::Red` — namespaced literal qiymat).

Chapter 20.4 shu constructor function xususiyatini iterator contextida ishlatadi: `Status::Value` `u32 -> Status` initializer sifatida `map(Status::Value)` ga berilishi mumkin.

Variant payload tutsa, pattern shu payloadni bind qilishi mumkin. Masalan `Coin::Quarter(state)` patterni `Quarter` variantiga mos keladi va ichidagi `UsState` value'ni `state`ga bog'laydi.

Module privacy kontekstida `pub enum` variantsni ham public qiladi. Struct fieldsdan farqli ravishda enum variants uchun alohida `pub` yozilmaydi.

Backend beginner source yana ikki nuance beradi. Birinchisi, unit-like variantlar semantik jihatdan singleton shape sifatida o'qilishi mumkin: `V4` va `V6` "son" emas, datasiz variantlar. Ikkinchisi, explicit numeric value berilganda variantning discriminanti belgilanadi; bu variantning butun ma'nosini faqat raqamga tushirib yubormaydi.

## Syntax and Examples

Datasiz variant:

```rust
enum IpAddrKind {
    V4,
    V6,
}

let kind = IpAddrKind::V4;
```

Tuple-like variant:

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
```

Struct-like variant:

```rust
enum Message {
    Move { x: i32, y: i32 },
}

let m = Message::Move { x: 10, y: 20 };
```

Payloadni `match`da ochish:

```rust
match coin {
    Coin::Quarter(state) => println!("State quarter from {state:?}!"),
    _ => (),
}
```

Variant — constructor function:

```rust
let make_v4 = IpAddr::V4;            // funksiya tipi: fn(u8, u8, u8, u8) -> IpAddr
let home = make_v4(127, 0, 0, 1);
```

Iterator mapping:

```rust
enum Status {
    Value(u32),
    Stop,
}

let statuses: Vec<Status> = (0u32..20).map(Status::Value).collect();
```

Explicit discriminantli variant:

```rust
enum HttpStatus {
    Ok = 200,
    NotFound = 404,
}
```

## Common Mistakes

- Variantni alohida type deb tushunish: `IpAddr::V4` o'zicha type emas, u `IpAddr` qiymatini hosil qiluvchi shakl/funksiya.
- Struct-like variant uchun `Message::Move(x, y)` deb yozish — to'g'risi `Message::Move { x, y }`.
- Datasiz variantni constructor function deb chaqirish (`IpAddrKind::V4()` xato; to'g'risi `IpAddrKind::V4`).
- Tuple-like va struct-like variantlar uchun mos pattern syntaxini aralashtirib yuborish.
- `pub enum` variants alohida `pub` talab qiladi deb o'ylash.
- Explicit discriminant variantni oddiy integer bilan teng deb o'ylash.

## Related Concepts

- [[enums]]
- [[constructor-functions|constructor functions]]
- [[function-pointers|function pointers]]
- [[map-with-named-functions|map with named functions]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[tuple-structs]]
- [[unit-like-structs]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[match]]

## Sources

- [[6-1-defining-an-enum]]
- [[6-2-the-match-control-flow-construct]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree]]
- [[wiki/sources/20-4-advanced-functions-and-closures]]
- [[wiki/sources/rust-for-backend-developers-enums]]
