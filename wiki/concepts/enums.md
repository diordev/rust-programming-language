---
title: "Enums"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, enums]
source_count: 8
---

# Enums

## Short Definition

Enum (enumeration) — value qaysi mumkin bo'lgan variantdan ekanini ifodalovchi Rust custom type. Har bir variant alohida shape va data tutishi mumkin.

## Why It Matters

[[structs]] related fields'ni *birga* group qiladi; enum esa value *qaysidir bittasi* bo'lishini bildiradi. Modeling uchun "yo X yo Y" tabiati bor data uchun (IP address V4/V6, message type, command result) enum eng to'g'ri vositadir. Compiler [[exhaustive-matching|exhaustive case handling]]ni majburlay olishi sababli runtime'da unutilgan branchlar bo'lmaydi.

## Mental Model

Enum — *bitta type* ostida birlashtirilgan variantlar to'plami. Variantlar enum ostida namespacelangan (`Color::Red`), shu sababli ulardagi data alohida-alohida boshqa typelar emas, hammasi bir xil enum type. Bu "har qanday Color qabul qiladigan funksiya" yoki "har qanday Message uchun method" yozishni qulay qiladi.

Variantni "data tutadigan tag" deb ko'rish mumkin: Rust runtime'da kichik discriminant + variant payload saqlaydi. Shu bois har bir variant alohida structdan farqli ravishda bitta type ostida qoladi.

Enum value'dan foydalanish odatda pattern matching bilan bo'ladi: [[match]] hamma variantsni handle qiladi, [[if-let|if let]] bitta variantga qiziqqanda qisqa bo'ladi, [[let-else|let...else]] esa kerakli variant bo'lmasa early return qilishga mos.

Module privacy kontekstida `pub enum` variantsni public qiladi. Bu structsdan farq qiladi: `pub struct` fieldsni public qilmaydi, lekin public enum variants public bo'ladi.

Collections kontekstida enum bir [[vector|Vec<T>]] ichida turli value shape'larini bitta concrete type sifatida saqlashga yordam beradi. Masalan, `Vec<SpreadsheetCell>` ichida `Int(i32)`, `Float(f64)`, va `Text(String)` variants bo'lishi mumkin.

Generic enums variant payload typelarini [[generic-type-parameters|generic type parameters]] bilan abstract qiladi. `Option<T>` bitta payload type'i orqali optional value'ni, `Result<T, E>` esa success type va error type'ni alohida ifodalaydi.

## Syntax and Examples

Datasiz variantlar:

```rust
enum IpAddrKind {
    V4,
    V6,
}

let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
```

Variantlarda data:

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(String::from("::1"));
```

Har xil shape — `Message`:

```rust
enum Message {
    Quit,                              // unit-like
    Move { x: i32, y: i32 },           // named fields
    Write(String),                     // tuple-like
    ChangeColor(i32, i32, i32),        // multi-tuple
}
```

`impl` orqali method:

```rust
impl Message {
    fn call(&self) {}
}

Message::Write(String::from("hi")).call();
```

Variantni `match` bilan handle qilish:

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {state:?}!");
            25
        }
    }
}
```

Enum-backed vector:

```rust
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
```

Generic standard library enums:

```rust
enum Option<T> {
    Some(T),
    None,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

## Common Mistakes

- Variantni qabul qiluvchi funksiyada enum type emas, alohida variant typeini kutish (variant — type emas, qiymat shakli).
- Har bir variant uchun alohida struct yozib, keyin `match` qilish noqulayligini sezganda bo'lmagan abstraction qidirish; ko'pincha yechim — bitta enum.
- Enum'ni `Option`/`Result` kabi prelude variantlaridan ajrata olmaslik: ular ham oddiy enum.
- Variantlar har xil shape tutgan paytda destructuring patterningini yodda tutmaslik (`Move { x, y }` o'zgacha pattern talab qiladi).
- Yangi variant qo'shib, mavjud `match` expressionsni yangilash kerakligini unutish; compiler buni catch-all yo'q bo'lsa eslatadi.
- Public enum variants alohida `pub` talab qiladi deb o'ylash.
- Vector ichida har xil typelar kerak bo'lganda enum bilan bitta concrete type yaratish mumkinligini unutish.
- Generic enum variants payload type'lari concrete call site yoki annotation orqali aniqlanishini unutish.

## Related Concepts

- [[enum-variants|enum variants]]
- [[option|Option]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[if-let|if let]]
- [[let-else|let...else]]
- [[methods]]
- [[structs]]
- [[generics]]
- [[generic-enums|generic enums]]
- [[generic-type-parameters|generic type parameters]]
- [[null-billion-dollar-mistake|null va billion dollar mistake]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[vector|Vec<T>]]
- [[spreadsheet-cell-vector|spreadsheet cell vector]]

## Sources

- [[6-enums-and-pattern-matching-the-rust-programming-language]]
- [[6-1-defining-an-enum-the-rust-programming-language]]
- [[6-2-the-match-control-flow-construct-the-rust-programming-language]]
- [[6-3-concise-control-flow-with-if-let-and-let-else-the-rust-programming-language]]
- [[5-3-methods-the-rust-programming-language]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
- [[10-1-generic-data-types-the-rust-programming-language]]
