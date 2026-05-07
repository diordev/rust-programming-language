---
title: "6.1. Defining an Enum - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, enums]
source_count: 1
source_path: "raw/books/the_rust_programming_language/6.1. Defining an Enum - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch06-01-defining-an-enum.html"
---

# 6.1. Defining an Enum - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/6.1. Defining an Enum - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch06-01-defining-an-enum.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[enums]]ni define qilish, variantlarga data biriktirish, `impl` orqali method qo'shish, va `Option<T>` enumi orqali "value bor yoki yo'q" holatini type system'da ifodalashni tushuntiradi.

[[structs]] related fieldlarni *birga* group qilsa, enum value *qaysidir bitta* set'dan ekanligini aytadi. `Rectangle` shapeni mumkin bo'lgan shapelar to'plamining biri sifatida ko'rsatish (Rectangle / Circle / Triangle) — enum uchun mos use case. IP address misolida har qanday addresss yo `V4` yoki `V6` bo'ladi, hech qachon ikkalasi bir vaqtda emas. Aynan shu xususiyat enum data structure'ni mos qiladi.

Eng oddiy enum:

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

Variantlar enum identifier ostida namespacelangan: `IpAddrKind::V4`, `IpAddrKind::V6`. Ikkalasi ham bir xil typeda — `IpAddrKind`. Shu sabab `fn route(ip_kind: IpAddrKind)` funksiyasi har qanday variantni qabul qila oladi.

Boshlang'ich struct yondashuvi `kind: IpAddrKind` va `address: String` fieldlari bor strukturadan iborat bo'lishi mumkin (Listing 6-1). Lekin enum'ning kuchli xususiyati: data'ni to'g'ridan-to'g'ri variant ichiga joylashtirish mumkin:

```rust
enum IpAddr {
    V4(String),
    V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));
let loopback = IpAddr::V6(String::from("::1"));
```

Bunda struct shart emas. Bundan tashqari muhim detal: har bir enum variant nomi avtomatik ravishda *constructor function* bo'lib qoladi. Ya'ni `IpAddr::V4` aslida `String` qabul qiladigan va `IpAddr` qaytaradigan funksiya — enum define qilinishidan kelib chiqib, Rust uni avtomatik beradi.

Enumning struct'dan farqiga oid yana bir afzallik: har bir variant *farqli* type va *farqli miqdor*da data tutishi mumkin. V4 to'rtta `u8` saqlasa, V6 esa `String` saqlashi mumkin:

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}
```

Standard library `IpAddr`ni shunga o'xshash, lekin har bir variant uchun alohida struct ishlatib define qilgan: `V4(Ipv4Addr)`, `V6(Ipv6Addr)`. Bu enum variant ichiga *istalgan* turdagi data — strings, numbers, structs, hatto boshqa enum — joylashtirish mumkinligini ko'rsatadi. Library enumi kontekstga olib kelinmaganidek (Chapter 7'da batafsil), bizning custom `IpAddr` library bilan to'qnashmaydi.

Listing 6-2 — `Message` enum to'rt xil variant shaklini ko'rsatadi:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

- `Quit` — datasiz, unit-like.
- `Move { x: i32, y: i32 }` — named fields, structga o'xshash.
- `Write(String)` — bitta `String`ni o'rab oladi.
- `ChangeColor(i32, i32, i32)` — uchta `i32`.

Bu to'rt variantni alohida-alohida struct bilan yozish ham mumkin (`QuitMessage`, `MoveMessage { x, y }`, `WriteMessage(String)`, `ChangeColorMessage(i32, i32, i32)`), lekin har bir struct alohida type bo'ladi va "har qanday Message qabul qil" deb funksiya yozish noqulay bo'ladi. Enum bitta type ostida hammasini birlashtiradi.

Structlarda bo'lgani kabi enum'larga ham `impl` orqali [[methods|method]] qo'shish mumkin:

```rust
impl Message {
    fn call(&self) {
        // method body would be defined here
    }
}

let m = Message::Write(String::from("hello"));
m.call();
```

Method body `self`dan foydalanib qaysi instance ustida ishlayotganini biladi.

### `Option` enumi

`Option` standard library tomonidan define qilingan, juda keng qo'llaniladigan enum. U value "nimadir" yoki "hech nima" bo'lishi mumkin degan holatni encode qiladi — list bo'sh bo'lsa first item olishga urinish, qidiruv natijasini ifodalash, va h.k. Type system orqali ifodalanganda compiler barcha kerakli case handle qilinganligini tekshirib chiqadi.

Rustda *null* yo'q. Tony Hoare 2009 yildagi "Null References: The Billion Dollar Mistake" prezentatsiyasida null'ni "billion dollar mistake" deb atagan: u referencelar uchun safe type system loyihalashtirayotgan paytda, oddiy implement qilish vasvasasiga uchib, null reference qo'shgan; va keyingi qirq yilda bu ko'plab error, vulnerability, system crashlarni keltirib chiqargan, taxminan billion dollar zarar bergan.

Rust null'ni olib tashladi, lekin "value bor / yo'q" g'oyasini quyidagi enum bilan ifodalaydi:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

`Option<T>` shu darajada keng ishlatiladiki, u prelude'da. `Some(T)` va `None` ni `Option::` prefix'siz ishlatish mumkin. `<T>` — generic type parameter (Chapter 10'da batafsil); `Some` istalgan turdagi bitta data tashishi mumkinligini va har bir concrete `T` `Option<T>`ni boshqa type qilishini bildiradi.

```rust
let some_number = Some(5);          // Option<i32>
let some_char = Some('e');          // Option<char>
let absent_number: Option<i32> = None;
```

`absent_number` uchun annotation kerak: faqat `None` qaraganda, `Some` qanday `T` tutishini compiler bilolmaydi.

`Option<T>`ning null'dan qanday afzalligi bor? `Option<T>` va `T` — *boshqa-boshqa types*. Compiler `Option<T>`ni hech qachon valid `T` deb qabul qilmaydi. Demak quyidagi kod compile bo'lmaydi:

```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y;
```

Error xulosasi: `cannot add `Option<i8>` to `i8`` — bu [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]ning bir varianti, chunki `i8 + Option<i8>` uchun `Add` trait implement qilinmagan.

Boshqacha aytganda: `Option<T>` qiymati bilan `T` operatsiyasini bajarish uchun avval `Option<T>`ni `T`ga aylantirish kerak. Bu null bilan bog'liq eng keng tarqalgan xato — "bu null emas deb taxmin qilish" — ni tutadi.

Inverse holat: agar value type'i `Option<T>` *bo'lmasa*, u null emasligini ishonch bilan deyish mumkin. Bu Rust'ning ataylab tanlangan dizayn qarori — null'ning hamma joyga tarqalishini cheklash.

`Some` ichidan `T`ni qanday olish kerak? `Option<T>`ning [ko'p methodi bor](https://doc.rust-lang.org/stable/std/option/enum.Option.html). Lekin asosiy yo'l: code to'rtinchi conditionga `Some(T)` va `None` casesini alohida handle qilishi kerak. Buning eng keng tarqalgan vositasi — [[match]] expression'i, u variantga qarab har xil branch bajaradi va matching value ichidagi data'ni ishlatishga ruxsat beradi.

## Key Concepts

- [[enums]]
- [[enum-variants|enum variants]]
- [[option|Option]]
- [[null-billion-dollar-mistake|null va billion dollar mistake]]
- [[generics]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[methods]]
- [[standard-library|standard library]]
- [[prelude]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]

## Code Examples

Variantsiz enum:

```rust
enum IpAddrKind {
    V4,
    V6,
}

fn route(ip_kind: IpAddrKind) {}
```

Data tashuvchi enum:

```rust
enum IpAddr {
    V4(String),
    V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));
let loopback = IpAddr::V6(String::from("::1"));
```

Har xil shape va data miqdoridagi variantlar:

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}
```

Standard library style — variantlar struct tutadi:

```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

`Message` enum — to'rt xil variant shakli:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

Enum'ga method qo'shish:

```rust
impl Message {
    fn call(&self) {
        // method body would be defined here
    }
}

let m = Message::Write(String::from("hello"));
m.call();
```

`Option<T>` definition:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

`Option` ishlatishi:

```rust
let some_number = Some(5);
let some_char = Some('e');
let absent_number: Option<i32> = None;
```

Type-mismatch xato:

```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y; // E0277: no implementation for `i8 + Option<i8>`
```

## Exercises or Practice Ideas

- `IpAddrKind` enumini yozib, `route(IpAddrKind::V4)` va `route(IpAddrKind::V6)` chaqiriqlarini bajarib ko'ring.
- Avval `IpAddr` ni struct + enum bilan, keyin variant ichida data bilan yozib, ikki yondashuvning farqini his qiling.
- `Message` enumini va unga `call(&self)` methodini yozib, har bir variantdan instance yaratib `m.call()` chaqiring.
- `enum Shape { Rectangle { width: u32, height: u32 }, Circle(f64), Triangle }` kabi o'z enumingizni o'ylab toping va misol kod yozing.
- `Some(5_i8) + 5_i8` qilib compile errorini ko'ring, keyin `match` yoki `unwrap_or(0)` bilan tuzating.
- `let none_value = None;` (annotationsiz) yozib compiler nima talab qilishini ko'ring; type annotation bilan tuzating.
- `Option<T>` doc'idagi 5 ta methodni o'qib, har biriga bittadan minimal misol yozing.

## Questions Raised

- Variantlar ichidagi data qanday memory layoutda saqlanadi? Discriminant qancha joy egallaydi?
- `Option<T>` ichidan `T`ni olishning idiomatik usullari qanday? `match`, `if let`, `unwrap_or`, `?` — qachon qaysisini tanlash kerak?
- Standard library `IpAddr`'i nima uchun struct-in-enum patternini tanlagan, oddiy `V4(u8,u8,u8,u8)` o'rniga?
- Variant ichida boshqa enum tutish (nested enum) qachon foydali?
- `<T>` generic parameter Chapter 10'gacha qaysi yo'llar bilan tushuntiriladi?

## Links To Update

- [[6-enums-and-pattern-matching|6. Enums and Pattern Matching]]
- [[enums]]
- [[enum-variants|enum variants]]
- [[option|Option]]
- [[null-billion-dollar-mistake|null va billion dollar mistake]]
- [[generics]]
- [[methods]]
- [[prelude]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]
