---
title: "6.3. Concise Control Flow with if let and let...else - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, if-let, let-else, pattern-matching]
source_count: 1
source_path: "raw/books/the_rust_programming_language/6.3. Concise Control Flow with if let and let...else - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch06-03-if-let.html"
---

# 6.3. Concise Control Flow with if let and let...else - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/6.3. Concise Control Flow with if let and let...else - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch06-03-if-let.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[if-let|if let]] va [[let-else|let...else]] syntaxlarini `match`ga nisbatan qisqaroq control flow vositalari sifatida tushuntiradi. Ular [[pattern-matching|pattern matching]]dan foydalanadi, lekin har xil trade-off bilan.

`if let` bitta patternga qiziqqanda, qolgan casesni ignore qilmoqchi bo'lganda ishlatiladi. Masalan, `Option<u8>` ichida `Some(max)` bo'lsa print qilish, `None` bo'lsa hech narsa qilmaslik:

```rust
let config_max = Some(3u8);

match config_max {
    Some(max) => println!("The maximum is configured to be {max}"),
    _ => (),
}
```

Shu kodni `if let` bilan qisqaroq yozish mumkin:

```rust
let config_max = Some(3u8);

if let Some(max) = config_max {
    println!("The maximum is configured to be {max}");
}
```

Syntax: `if let PATTERN = EXPRESSION { ... }`. Pattern mos kelsa, bindinglar block ichida ishlaydi; mos kelmasa, block bajarilmaydi. Bu `match`ning "bitta interesting arm + qolgan hammasi ignore" shakli uchun syntax sugar.

Asosiy trade-off: `if let` kamroq typing, kamroq indentation va kamroq boilerplate beradi, lekin [[exhaustive-matching|exhaustive checking]]ni yo'qotadi. Demak hamma casesni handle qilish muhim bo'lsa, [[match]] yaxshiroq; faqat bitta case muhim bo'lsa, `if let` o'qilishi osonroq.

`if let`ga `else` ham qo'shish mumkin. `else` block `match`dagi `_` armga o'xshaydi:

```rust
if let Coin::Quarter(state) = coin {
    println!("State quarter from {state:?}!");
} else {
    count += 1;
}
```

Bu "agar quarter bo'lsa state'ni ishlat, aks holda count oshir" degan branchingni `match`dan qisqaroq ifodalaydi.

Section keyin "happy path"ni asosiy oqimda saqlash uchun `let...else`ni ko'rsatadi. Nested `if let` kodida asosiy ish block ichiga chuqur kirib ketadi:

```rust
fn describe_state_quarter(coin: Coin) -> Option<String> {
    if let Coin::Quarter(state) = coin {
        if state.existed_in(1900) {
            Some(format!("{state:?} is pretty old, for America!"))
        } else {
            Some(format!("{state:?} is relatively new."))
        }
    } else {
        None
    }
}
```

`let...else` pattern mos kelganda bindingni outer scope'ga olib chiqadi; mos kelmasa, `else` arm early return qiladi:

```rust
fn describe_state_quarter(coin: Coin) -> Option<String> {
    let Coin::Quarter(state) = coin else {
        return None;
    };

    if state.existed_in(1900) {
        Some(format!("{state:?} is pretty old, for America!"))
    } else {
        Some(format!("{state:?} is relatively new."))
    }
}
```

Syntax: `let PATTERN = EXPRESSION else { ... };`. Pattern mos kelsa, binding outer scope'da mavjud bo'ladi. Pattern mos kelmasa, `else` block normal davom eta olmaydi; bu yerda functiondan `return None` qiladi. Shu sabab `let...else` "agar kerakli shape bo'lmasa, erta chiq; aks holda asosiy ishni chapdan o'ngga o'qiladigan flowda davom ettir" patterni uchun juda mos.

Chapter 6 shu bilan [[enums]], `Option<T>`, [[match]], `if let`, va `let...else` orqali domain conceptsni type-safe custom types bilan modellashtirishni yakunlaydi. Keyingi katta mavzu — [[module|modules]] orqali API'ni tartibli tashkil qilish.

## Key Concepts

- [[if-let|if let]]
- [[let-else|let...else]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[option|Option]]
- [[enums]]
- [[control-flow|control flow]]
- [[module|modules]]

## Code Examples

`match` o'rniga `if let`:

```rust
let config_max = Some(3u8);

if let Some(max) = config_max {
    println!("The maximum is configured to be {max}");
}
```

`if let` + `else`:

```rust
let mut count = 0;

if let Coin::Quarter(state) = coin {
    println!("State quarter from {state:?}!");
} else {
    count += 1;
}
```

`let...else` bilan early return:

```rust
fn describe_state_quarter(coin: Coin) -> Option<String> {
    let Coin::Quarter(state) = coin else {
        return None;
    };

    if state.existed_in(1900) {
        Some(format!("{state:?} is pretty old, for America!"))
    } else {
        Some(format!("{state:?} is relatively new."))
    }
}
```

## Exercises or Practice Ideas

- `Option<u8>` uchun avval `match`, keyin `if let` yozing; qaysi biri ko'proq boilerplate talab qilishini solishtiring.
- `if let Some(value) = maybe_value { ... } else { ... }` misolini `match`ga qayta yozing.
- `let Some(name) = maybe_name else { return; };` patternini yozib, `name` outer scope'da ishlashini tekshiring.
- `let...else`dagi `else` blockdan `return`ni olib tashlab compile errorni ko'ring.
- Nested `if let` bor funksiyani `let...else` bilan refactor qiling.

## Questions Raised

- `if let`dan foydalanish exhaustive checkingni yo'qotadi; qaysi casesda bu acceptable trade-off?
- `let...else` qachon `?` operatoridan yaxshiroq yoki aniqroq ko'rinadi?
- Early return flow API design va error handlingga qanday ta'sir qiladi?

## Links To Update

- [[if-let|if let]]
- [[let-else|let...else]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[6-enums-and-pattern-matching|6. Enums and Pattern Matching]]

