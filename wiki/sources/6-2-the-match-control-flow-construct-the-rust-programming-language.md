---
title: "6.2. The match Control Flow Construct - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, match, pattern-matching]
source_count: 1
source_path: "raw/books/the_rust_programming_language/6.2. The match Control Flow Construct - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch06-02-match.html"
---

# 6.2. The match Control Flow Construct - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/6.2. The match Control Flow Construct - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch06-02-match.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[match]] control flow constructini chuqurroq tushuntiradi. `match` value'ni ketma-ket [[pattern-matching|patterns]] bilan solishtiradi va birinchi mos arm codeini bajaradi. `if` condition faqat `bool` bo'lishi kerak, `match` esa istalgan type'dagi expression ustida ishlay oladi. Shu sabab enum variantlari, `Option<T>`, `Result<T, E>` va literal qiymatlar uchun juda qulay.

`match` arm ikki qismdan iborat: pattern va bajariladigan expression. Pattern va expression `=>` bilan ajratiladi, arm'lar comma bilan ajratiladi. Qisqa arm code odatda braces'siz yoziladi:

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

`match` o'zi expression: mos arm qaytargan value butun `match` expression'ining value'siga aylanadi. Agar arm bir nechta statement bajarishi kerak bo'lsa, braces ishlatiladi va blockning oxirgi expression'i arm value'si bo'ladi:

```rust
match coin {
    Coin::Penny => {
        println!("Lucky penny!");
        1
    }
    Coin::Nickel => 5,
    Coin::Dime => 10,
    Coin::Quarter => 25,
}
```

[[enum-variants|Enum variants]] ichida data bo'lsa, `match` pattern shu data'ni bind qila oladi. `Coin::Quarter(UsState)` misolida `Coin::Quarter(state)` patterni quarter ichidagi `UsState` value'ni `state` variable'iga bog'laydi. Bu enum ichidagi payloadni extract qilishning asosiy yo'li:

```rust
match coin {
    Coin::Penny => 1,
    Coin::Nickel => 5,
    Coin::Dime => 10,
    Coin::Quarter(state) => {
        println!("State quarter from {state:?}!");
        25
    }
}
```

`Option<T>` bilan `match` ayniqsa muhim. `plus_one` funksiyasi `Some(i)` bo'lsa ichki `i`ni bind qilib `Some(i + 1)` qaytaradi, `None` bo'lsa `None` qaytaradi:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

Bu yerda `Some(i)` patterni `Some(5)`ga mos kelganda `i = 5` bo'ladi. `None` ichida value yo'q, shuning uchun qo'shish bajarilmaydi.

Rust `match` uchun [[exhaustive-matching|exhaustive matching]] talab qiladi: barcha ehtimoliy cases handle qilinishi kerak. Agar `Option<i32>` uchun faqat `Some(i)` arm yozilib, `None` unutilsa, compiler [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]] beradi va aynan `None` case yopilmaganini aytadi. Bu `Option<T>`ning null o'rnini bosishdagi asosiy safety mexanizmi: "value bor deb taxmin qilish" compile time'da ushlanadi.

Oxirida section [[catch-all-patterns|catch-all patterns]]ni ko'rsatadi. Agar aniq bir nechta qiymatga alohida action, qolgan hammasiga default action kerak bo'lsa, oxirgi arm named binding bo'lishi mumkin:

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    other => move_player(other),
}
```

`other` barcha qolgan qiymatlarga mos keladi va shu qiymatni bind qiladi. Agar qolgan qiymatning o'zi kerak bo'lmasa, `_` ishlatiladi:

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => reroll(),
}
```

Hech narsa qilmaslik kerak bo'lsa, `_ => ()` yoziladi. Bu "qolgan hammasini ongli ravishda ignore qilyapman" degan signal. Catch-all arm oxirida turishi kerak, chunki patterns yuqoridan pastga tekshiriladi; catch-all oldin kelsa, undan keyingi arm'lar unreachable bo'ladi.

## Key Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[enums]]
- [[enum-variants|enum variants]]
- [[option|Option]]
- [[unit-type|unit type]]
- [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]]

## Code Examples

Enum variantlariga qarab value qaytarish:

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

`Option<T>`ni handle qilish:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

Catch-all binding va `_`:

```rust
let dice_roll = 9;

match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    other => move_player(other),
}

match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => reroll(),
}
```

## Exercises or Practice Ideas

- `enum TrafficLight { Red, Yellow, Green }` yarating va `match` bilan har bir rang uchun action string qaytaring.
- `plus_one` misolini yozing, keyin `None => None` arm'ini olib tashlab [[e0004-non-exhaustive-patterns|E0004]] xabarini kuzating.
- `Coin::Quarter(UsState)` misolini kengaytirib, `UsState`ga kamida uchta variant qo'shing va `state` binding bilan print qiling.
- Dice roll misolida `other` bindingni `_` bilan almashtirib ko'ring; qaysi holatda value kerak, qaysi holatda kerak emasligini yozib chiqing.
- `_ => ()` ishlatib "default branchda hech narsa qilmaslik"ni explicit qiling.

## Questions Raised

- Qachon `match` ishlatish, qachon `if let` yoki `let...else` ishlatish yaxshiroq?
- `_` catch-all patterni muhim variantlarni yashirib yubormasligi uchun qanday code review odati kerak?
- Chapter 19'dagi advanced patterns bu chapterdagi basic patternlardan qanday farq qiladi?

## Links To Update

- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[option|Option]]
- [[6-enums-and-pattern-matching|6. Enums and Pattern Matching]]

