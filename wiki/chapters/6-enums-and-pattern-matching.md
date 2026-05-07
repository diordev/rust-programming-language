---
title: "6. Enums and Pattern Matching"
type: chapter
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, chapter, enums]
source_count: 4
---

# 6. Enums and Pattern Matching

## Learning Goal

Rustda mumkin bo'lgan variantlardan birini ifodalash uchun [[enums|enum]] type yaratish, variantlar ichida turli shape va miqdordagi data tashish, [[option|Option<T>]] orqali "value bor / yo'q" holatini null'siz ifodalash, va [[match]], [[if-let|if let]], hamda [[let-else|let...else]] orqali enumlarni control flowda ishlatish.

## Main Ideas

- [[enums]] mumkin bo'lgan variantlarni sanab type yaratadi; struct related fields'ni group qilsa, enum value qaysi variantda ekanini bildiradi.
- [[enum-variants|Variantlar]] enum identifier ostida namespacelangan: `IpAddrKind::V4`, `IpAddrKind::V6`. Hammasi bir xil typeda.
- Variantlarga to'g'ridan-to'g'ri data biriktirish mumkin: `enum IpAddr { V4(String), V6(String) }`. Alohida struct kerak emas.
- Har bir variant — *constructor function*. `IpAddr::V4("127.0.0.1".to_string())` aslida `String`dan `IpAddr` yasovchi funksiya chaqiriqi.
- Variantlar har xil shape va miqdorda data tutishi mumkin: `V4(u8,u8,u8,u8)`, `V6(String)`, struct-like `Move { x, y }`, unit-like `Quit`.
- Standard library `IpAddr`'i variant ichida struct ishlatadi (`V4(Ipv4Addr)`, `V6(Ipv6Addr)`); custom `IpAddr` library bilan to'qnashmaydi — Chapter 7 scoping.
- `Message` enum to'rt xil variant shaklini bitta type ostida birlashtiradi; alohida structlar bilan bir xil "har qanday Message" funksiyasini yozish qiyin bo'lar edi.
- Enum'ga `impl` orqali [[methods|method]] qo'shish mumkin; `&self` enum instance'ini borrow qiladi.
- [[option|Option<T>]] standard libraryda: `enum Option<T> { None, Some(T) }`. Prelude'da, prefix kerak emas.
- Rustda *null yo'q*. Tony Hoare null'ni "billion dollar mistake" deb atagan; Rust uni type system orqali olib tashlab, [[null-billion-dollar-mistake|null o'rniga `Option`]]ni qo'ydi.
- `Option<T>` va `T` boshqa type. Compiler `i8 + Option<i8>` ga ruxsat bermaydi — [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]].
- Type'i `Option<T>` bo'lmasa value null bo'lmasligiga ishonch bilan tayanish mumkin; bu Rust'ning ataylab tanlangan safety dizayni.
- `<T>` — generic type parameter (Chapter 10). Hozircha: `Some` istalgan turdagi bitta data tutishi mumkin va har bir concrete `T` `Option<T>`ni boshqa type qiladi.
- `Option<T>`ni ishlatish uchun ichki value bor/yo'qligini handle qilish kerak; asosiy vositalar [[match]], [[if-let|if let]], va [[let-else|let...else]].
- [[match]] expression value'ni patterns bilan solishtiradi; har bir arm pattern va code expression'dan iborat.
- `match` arm code'i expression bo'lgani uchun mos arm qaytargan value butun `match` expression'ining value'si bo'ladi.
- Pattern binding enum variant ichidagi data'ni ochadi: `Coin::Quarter(state)` patterni quarter payloadini `state`ga bind qiladi.
- [[option|Option<T>]] uchun `match` `Some(i)` va `None` casesni explicit ajratadi; `Some(i)` ichki `T`ni bind qiladi.
- [[exhaustive-matching|Exhaustive matching]] Rust safety modelining bir qismi: `Option<T>`da `None` unutilsa [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]] chiqadi.
- [[catch-all-patterns|Catch-all patterns]] qolgan casesni qamrab oladi: `other` value'ni bind qiladi, `_` esa value'ni ignore qiladi.
- [[if-let|if let]] bitta interesting pattern uchun `match`ga qisqa alternativa; boilerplateni kamaytiradi, lekin exhaustive checking bermaydi.
- `if let ... else` `match`dagi `_` armga o'xshash fallback branch beradi.
- [[let-else|let...else]] kerakli pattern mos kelmasa early return qilib, mos kelsa bindingni outer scope'da beradi; bu happy pathni asosiy flowda saqlaydi.
- Chapter 6 yakunida custom types API'ni type-safe qiladi; keyingi mavzu [[module|modules]] orqali API'ni tartibli expose qilish.

## Concepts

- [[enums]]
- [[enum-variants|enum variants]]
- [[option|Option]]
- [[null-billion-dollar-mistake|null va billion dollar mistake]]
- [[generics]]
- [[methods]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[if-let|if let]]
- [[let-else|let...else]]
- [[standard-library|standard library]]
- [[prelude]]
- [[module|modules]]
- [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]

## Examples

Bo'sh variantlar bilan:

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

Variantlarda data:

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}
```

`Message` enumning variant shakllari:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

Enum bilan method:

```rust
impl Message {
    fn call(&self) {}
}

let m = Message::Write(String::from("hello"));
m.call();
```

`Option<T>` ishlatishi:

```rust
let some_number = Some(5);
let some_char = Some('e');
let absent_number: Option<i32> = None;
```

Type-safety tutib qoladigan xato:

```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y; // E0277
```

Enum variantlari bilan `match`:

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

Variant payloadini bind qilish:

```rust
#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

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

`Option<T>` bilan `match`:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

`if let`:

```rust
let config_max = Some(3u8);

if let Some(max) = config_max {
    println!("The maximum is configured to be {max}");
}
```

`let...else`:

```rust
fn print_name(name: Option<String>) {
    let Some(name) = name else {
        return;
    };

    println!("{name}");
}
```

## Exercises

- `IpAddrKind` enumini yozing va `route(...)` funksiyani har ikki variant bilan chaqiring.
- `IpAddr`ni struct bilan, keyin enum-with-data bilan, keyin variantlar har xil tipdagi data tutadigan shaklda yozib solishtiring.
- `Message` enum + `call(&self)` method yozing; har bir variantdan instance hosil qilib `call`ni chaqiring.
- O'z domeningiz uchun enum o'ylab toping (masalan `enum Shape { Rectangle { width, height }, Circle(f64), Triangle }`) va minimal funksional kod yozing.
- `Some(5_i8) + 5_i8` ni compile qilib E0277 xabarini ko'ring; `match` yoki `unwrap_or` bilan to'g'rilang.
- `let none_value = None;` deb yozib type annotation talabini ko'ring.
- `Option<T>` documentation'idan 5 ta method tanlab har biriga minimal misol yozing.
- `TrafficLight` enumini `match` bilan handle qiling; har bir variant alohida string qaytarsin.
- `plus_one` misolidan `None => None` arm'ini olib tashlab E0004 xabarini ko'ring.
- Dice roll misolida `other` binding va `_` pattern farqini yozib ko'ring.
- `Option<u8>` uchun `match`ni `if let`ga qayta yozing.
- Nested `if let` bor funksiyani `let...else` bilan refactor qiling.

## Review Questions

- Enum struct'dan qaysi jihatlari bilan farq qiladi va qachon to'g'ri tanlov bo'ladi?
- Variant nima uchun avtomatik constructor function bo'lib qoladi?
- Variantlar har xil shape va data miqdorida bo'lishi nima uchun struct'dan ustun?
- Enum ichidagi data nima uchun struct ichidagi `enum + struct` patternidan ko'pincha qisqaroq?
- `Option<T>` null'dan qanday afzalliklar beradi?
- `Some(T)` va `None` qaysi case'da type annotation talab qiladi?
- `<T>` generic parameteri qanday qilib `Option<T>`ni "har qanday data uchun" type qila oladi?
- Compiler nima uchun `i8 + Option<i8>`ga ruxsat bermaydi?
- Type'i `Option<T>` bo'lmagan value uchun null check qilish kerakmi?
- `match` va `if` orasidagi eng muhim type-level farq nima?
- `match` nima uchun exhaustive bo'lishi kerak?
- `Some(i)` patterni qanday qilib `Option<T>` ichidagi value'ni ochadi?
- `other` catch-all pattern va `_` placeholder orasidagi farq nima?
- Qachon `if let`, qachon `match` ishlatish kerak?
- `let...else` happy pathni qanday soddalashtiradi?

## Related Pages

- [[6-enums-and-pattern-matching-the-rust-programming-language]]
- [[6-1-defining-an-enum-the-rust-programming-language]]
- [[6-2-the-match-control-flow-construct-the-rust-programming-language]]
- [[6-3-concise-control-flow-with-if-let-and-let-else-the-rust-programming-language]]
- [[5-using-structs-to-structure-related-data|5. Using Structs to Structure Related Data]]
- [[ip-addr-enum|IP address enum example]]
- [[message-enum|Message enum example]]
- [[coin-match|Coin match example]]
- [[option|Option]]
- [[enums]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[if-let|if let]]
- [[let-else|let...else]]
- [[generics]]
- [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]
- [[rust-learning-roadmap]]
