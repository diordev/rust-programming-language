---
title: "Option"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-13
tags: [rust, enums, option]
source_count: 9
---

# Option

## Short Definition

`Option<T>` — value "nimadir" yoki "hech nima" bo'lishi mumkin degan holatni ifodalovchi standard library enumi. Variantlari: `Some(T)` va `None`; u odatda `match`, `if let`, `let...else`, yoki helper methodlar bilan ochiladi.

## Why It Matters

Rustda *null yo'q*. Tony Hoare null'ni "billion dollar mistake" deb atagan, chunki null ko'p tildan kutilmagan crash va vulnerabilitylarni keltirib chiqargan. Rust shu konseptni type system orqali ifodalaydi: agar value yo'q bo'lishi mumkin bo'lsa, type'i `Option<T>` bo'lishi *shart*. Aks holda value har doim valid `T` deb ishonish mumkin. Bu eng keng tarqalgan null-related buglarni compile time'da tutadi.

## Mental Model

`Option<T>` — generic enum. `<T>` ichidagi data turi har xil bo'lishi mumkin: `Option<i32>`, `Option<String>`, `Option<&str>` — har biri *boshqa-boshqa* type. Compiler `Option<T>`ni `T` deb qabul qilmaydi, shuning uchun `T` operatsiyasini bajarishdan oldin `Option<T>`ni "ochish" kerak ([[match]], [[if-let|if let]], [[let-else|let...else]], `unwrap_or`, `?` va boshqa methodlar orqali).

`Option<T>` shu darajada keng ishlatiladiki, prelude'ga kiritilgan: `Some(T)` va `None`'ni `Option::` prefix'siz yozish mumkin.

`Option<T>` faqat "value bor/yo'q" uchun emas. Ownership-sensitive cleanup code'da ham u juda foydali: struct fieldni vaqtincha `Some(...)` qilib saqlab, keyin `take()` bilan owned qiymatni xavfsiz chiqarib olish mumkin.

Chapter 10.1 `Option<T>`ni [[generic-enums|generic enum]] syntax'i sifatida qayta ko'rsatadi. `T` `Some(T)` ichidagi value type'ini bildiradi, `None` esa data saqlamaydi.

Vector accessda `get` methodi `Option<&T>` qaytaradi: index bor bo'lsa `Some(&element)`, out-of-bounds bo'lsa `None`.

[[hash-map|HashMap]] lookupda ham `get` `Option<&V>` qaytaradi: key mavjud bo'lsa `Some(&value)`, missing key bo'lsa `None`. Shu sabab map lookupdan keyin absence case explicit handle qilinadi.

`?` operator `Option` qaytaradigan function ichida ishlatilsa, `None` bo'lganda early return qiladi, `Some(value)` bo'lsa value'ni ochadi. Bu `Result` ustidagi `?`ga o'xshaydi, lekin `Result` va `Option` avtomatik bir-biriga convert bo'lmaydi.

Type-system guideline: function parameteri `Option<T>` emas, `T` bo'lsa, function body `None` case'ni runtime handle qilmaydi. Compiler caller haqiqiy `T` berganini tekshiradi.

Performance modeli: compiler [[monomorphization]] orqali `Option<i32>` va `Option<f64>` kabi concrete uses uchun specialized code yaratadi.

## Syntax and Examples

Definition:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

Yaratish:

```rust
let some_number = Some(5);          // Option<i32>
let some_char = Some('e');          // Option<char>
let absent_number: Option<i32> = None;
```

`absent_number`'ga annotation kerak: bo'sh `None`'dan compiler `T`ni infer qila olmaydi.

Type-safety:

```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y; // compile error E0277
```

Idiomatik handle qilish:

```rust
let value: Option<i32> = Some(7);

match value {
    Some(n) => println!("Got {n}"),
    None => println!("Nothing"),
}

let doubled = value.map(|n| n * 2);
let or_zero = value.unwrap_or(0);
```

`match` bilan transform qilish:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

Bitta case qiziq bo'lsa:

```rust
if let Some(max) = config_max {
    println!("The maximum is configured to be {max}");
}
```

Early return:

```rust
let Some(name) = maybe_name else {
    return;
};
```

`?` bilan early return:

```rust
fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
}
```

Vector `get`:

```rust
let v = vec![1, 2, 3];

match v.get(2) {
    Some(value) => println!("{value}"),
    None => println!("missing"),
}
```

Hash map `get`:

```rust
let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);
```

`Option::take()` ownership extraction:

```rust
let mut sender = Some(tx);
let tx = sender.take(); // sender endi None
drop(tx);
```

Bu pattern ayniqsa `Drop` impl ichida foydali, chunki `&mut self` bilan borrowed struct field'dan owned value'ni olish kerak bo'ladi. Qarang: [[option-take|Option::take()]].

## Common Mistakes

- `Option<T>` ichidagi `T`ni to'g'ridan-to'g'ri ishlatishga urinish (E0277).
- `let x = None;` deb yozib annotation bermaslik (compiler `T`ni bilolmaydi).
- Hamma joyda `unwrap()` ishlatish — bu `None` paytda panic. Beginner kod uchun ham `expect("...")` to'g'riroq, chunki message debugging uchun foydali.
- `Option<&T>` va `&Option<T>` orasidagi farqni hisobga olmaslik.
- `match`da `None` case'ni unutish ([[e0004-non-exhaustive-patterns|E0004]]).
- Out-of-bounds vector access uchun `get` o'rniga indexing ishlatib, recoverable holatda panic qilish.
- Hash mapda missing key bo'lishi mumkinligini handle qilmaslik.
- `Result` qaytaradigan function ichida `Option` ustida `?` avtomatik ishlaydi deb o'ylash.
- `Option` va `Result` conversion uchun `ok` yoki `ok_or` kabi explicit methodlar kerak bo'lishi mumkinligini unutish.
- `Option::take()` faqat convenience method deb o'ylash — u ko'pincha ownership-safe cleanup patternining markazi bo'ladi.

## Related Concepts

- [[enums]]
- [[null-billion-dollar-mistake|null va billion dollar mistake]]
- [[generics]]
- [[generic-enums|generic enums]]
- [[generic-type-parameters|generic type parameters]]
- [[monomorphization]]
- [[match]]
- [[if-let|if let]]
- [[let-else|let...else]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[result|Result]]
- [[question-mark-operator|question mark operator]]
- [[type-checking|type checking]]
- [[prelude]]
- [[standard-library|standard library]]
- [[drop]]
- [[vector-indexing|vector indexing]]
- [[hash-map|HashMap]]
- [[panic]]
- [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]

## Sources

- [[6-1-defining-an-enum]]
- [[6-2-the-match-control-flow-construct]]
- [[6-3-concise-control-flow-with-if-let-and-let-else]]
- [[8-1-storing-lists-of-values-with-vectors]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[9-2-recoverable-errors-with-result]]
- [[9-3-to-panic-or-not-to-panic]]
- [[10-1-generic-data-types]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
