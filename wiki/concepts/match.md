---
title: "match"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, control-flow]
source_count: 7
---

# match

## Short Definition

`match` value'ni patterns bilan solishtirib, mos arm codeini bajaradigan, barcha casesni qamrab olishni talab qiladigan Rust expression.

## Why It Matters

`match` enum variants bilan ishlashda juda muhim. Guessing game chapterida u `Ordering` va `Result` variantsni handle qilish uchun ishlatiladi; Chapter 6.2 esa `Coin` va `Option<T>` misollari orqali enum payloadini bind qilish va [[exhaustive-matching|exhaustive matching]]ni ko'rsatadi.

## Mental Model

`match` value oladi, armsni yuqoridan pastga tekshiradi, birinchi mos patternni bajaradi. Har bir arm pattern va code expression'dan iborat. Mos arm qaytargan value butun `match` expression'ining value'si bo'ladi.

Rust barcha kerakli holatlar ko'rib chiqilganini tekshiradi. Agar `Option<T>`da `None` unutilsa, compiler [[e0004-non-exhaustive-patterns|E0004]] beradi. Qolgan casesni ongli default bilan yopish uchun [[catch-all-patterns|catch-all pattern]] ishlatiladi.

Chapter 9.2da `match` [[result|Result<T, E>]]ni handle qilishning primitive tool'i sifatida ishlatiladi: `Ok(file)` success value'ni ochadi, `Err(error)` esa recovery yoki panic branchiga o'tadi.

Keyinroq pattern familyasi ranges, `|`, [[at-binding|@ binding]], [[match-guard|match guard]], struct patterns, va [[slice-patterns|slice patterns]] bilan kengayadi. String content matchingda esa `String`ni ko'pincha `as_str()` qilib literal patternlarga solishtirish kerak bo'ladi.

## Syntax and Examples

```rust
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```

Enum variants:

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
match coin {
    Coin::Quarter(state) => {
        println!("State quarter from {state:?}!");
        25
    }
    _ => 0,
}
```

`Option<T>`:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

`Result` bilan:

```rust
let greeting_file = match File::open("hello.txt") {
    Ok(file) => file,
    Err(error) => panic!("Problem opening the file: {error:?}"),
};
```

## Common Mistakes

- Arm patternsni exhaustive qilmaslik.
- `_` catch-all patternni juda erta ishlatib, muhim casesni yashirish.
- Arm expressionlari turli type qaytarishiga yo'l qo'yish.
- Bitta case qiziq bo'lgan joyda haddan tashqari verbose `match` yozish; bu holatda [[if-let|if let]] aniqroq bo'lishi mumkin.
- Nested `Result` handling juda chuqurlashsa, [[question-mark-operator|?]] yoki helper methods borligini unutish.

## Related Concepts

- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[if-let|if let]]
- [[let-else|let...else]]
- [[option|Option]]
- [[enums]]
- [[result|Result]]
- [[error-kind|ErrorKind]]
- [[question-mark-operator|question mark operator]]
- [[ordering|Ordering]]
- [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]]
- [[never-type|never type (!)]] — `match` arm coercion
- [[diverging-functions|diverging functions]] — `panic!`, `continue` arm

## Sources

- [[wiki/sources/2-programming-a-guessing-game]]
- [[6-2-the-match-control-flow-construct]]
- [[6-3-concise-control-flow-with-if-let-and-let-else]]
- [[9-2-recoverable-errors-with-result]]
- [[wiki/sources/19-1-all-the-places-patterns-can-be-used]]
- [[wiki/sources/rust-for-backend-developers-pattern-matching]]
- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
